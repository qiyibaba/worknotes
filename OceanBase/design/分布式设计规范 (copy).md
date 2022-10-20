# 分布式设计规范

不同类型的事务提交耗时不同，性能调优尽量降低跨机分布式事务的比例。事务模型可以从以下几个方面入手：

- 业务整体逻辑。
- 细化到具体的事务。
- 了解多表、单表事务的比例以及各类 SQL 的执行频率。

## 事务类型统计

SQL 的执行计划分为四种：Local 、Remote 、Distribute 和 Uncertain。其中 Local 表示当前语句所涉及的分区 Leader 与 Session 所在的机器相同；Remote表示当前语句所涉及的分区 Leader 与 Sesison 所在的机器不同；Distribute 和 Uncertain 计划不能确定 Leader 和 Session 的关系，可能在同一个机器，也可能跨多机。

对于事务的性能而言，优先使用单机事务，其次才是分布式事务；根据执行计划的类型统计信息，可以大致估算出分布式事务的比例，进而为调优提供数据支持。相关 SQL 如下：

```unknow
MySQL [oceanbase]> select plan_type, count(1) from gv$sql_audit where 
request_time > time_to_usec('2021-08-24 18:00:00') group by plan_type;
+-----------+----------+
| plan_type | count(1) |
+-----------+----------+
|         1 |    17119 |
|         0 |     9614 |
|         3 |     4400 |
|         2 |    23429 |
+-----------+----------+
4 rows in set (0.01 sec)
```

其中，`plan_type` = 1、2、3、4 分别表示 Local 、Remote 、Distribute 和 Uncertain 执行计划。一般来讲，0 代表无 plan 的 SQL 语句，比如：set autocommit=0/1，commit 等。

## 非 local 计划分析

非 Local 计划的请求（ plan_type = 0 除外），大概率会导致事务跨机，相对于单机事务，性能会有一定的影响。可按照如下几种情况进行检查：

1. primary zone 为单 Zone & 单 Unit。
2. primary zone 为单 Zone & 多 Unit。
3. primary zone 为 RANDOM。

### Primary Zone 为单 Zone & 单 Unit

单 Unit 部署的场景如果出现 Remote、Uncertain 计划，是不符合预期的，原因大概有几种：

1. 直连了该租户的 Follower。可根据执行计划的类型统计信息确认。

2. 应用连接了 OBProxy，但事务的第一条 SQL 无法被 OBProxy 进行正确的转发，导致 Session 和事务所涉及的分区 Leader 跨机。此时需要去检查本集群所有 OBProxy 的日志，关键日志信息如下：

   ```unknow
   fail to caculate partition id, just use tenant server
   ```

3. 部分分区刚刚切主，OBServer 或者 OBPoxy 维护的 Location cache 尚未刷新到最新。

如果是情况 1，需要让应用改成连接 OBProxy；如果是情况 2，说明当前 SQL 较为复杂，OBProxy 在 Parser 过程中无法计算出需要访问的分区，因此随机做了发送。此时需要对 SQL 写法进行调整，带上分区键；如果是情况 3，则不需要处理。

### Primary Zone 为单 Zone & 多 Unit

该场景下，同 Zone 的 OBServer 会自动进行分区负载均衡，事务跨机大概率是可能的。为了避免跨机事务，结合事务内语句执行情况，进行 table group 划分，尽量保证事务单机执行。

table group 的用法如下所示：

```unknow
--非分区场景
create tablegroup tg1;
create table t1 (id1 int, id2 int) tablegroup tg1;
create table t2 (id1 int, id2 int) tablegroup tg1;

--HASH分区(mysql mode)
create tablegroup tg2 binding true partition by hash partitions 2;
create table pg_trans_test2_1(id1 int, id2 int) 
  tablegroup tg2 partition by hash(id1 % 2) partitions 2;
create table pg_trans_test2_2(id1 int, id2 int) 
  tablegroup tg2 partition by hash(id1 % 2) partitions 2;

--HASH分区(oracle mode)
create tablegroup tg2 binding true partition by hash partitions 2;
create table pg_trans_test2_1(id1 int, id2 int) 
  tablegroup tg2 partition by hash(id1) partitions 2;
create table pg_trans_test2_2(id1 int, id2 int) 
  tablegroup tg2 partition by hash(id1) partitions 2;

--RANGE分区（mysql mode）
create tablegroup tg3 binding true 
  partition by range columns 1 (
          partition p0 values less than (10), 
          partition p1 values less than(20));
            
create table pg_trans_test3_1(id1 int, id2 int) tablegroup tg3 
  partition by range columns(id1) 
      (partition p0 values less than (10), 
         partition p1 values less than(20));
create table pg_trans_test3_2(id1 int, id2 int) tablegroup tg3 
  partition by range columns(id1) 
      (partition p0 values less than (10), 
         partition p1 values less than(20));

--RANGE分区（Oracle mode）
create tablegroup tg3 binding true 
  partition by range columns 1 
      (partition p0 values less than (10), 
         partition p1 values less than(20));
create table pg_trans_test3_1(id1 int, id2 int) 
  tablegroup tg3 partition by range (id1) 
      (partition p0 values less than (10), 
         partition p1 values less than(20));
create table pg_trans_test3_2(id1 int, id2 int) 
  tablegroup tg3 partition by range (id1) 
      (partition p0 values less than (10), 
         partition p1 values less than(20));

--LIST分区 (mysql mode)
create tablegroup tg4 binding true partition by list columns 1 (
    partition p0 values in (1, 2, 3, 4, 5, 6, 7, 8, 9, 10),
    partition p1 values in (11, 12, 13, 14, 15, 16, 17, 18, 19, 20)
);
create table pg_trans_test4_1(id1 int, id2 int) tablegroup tg4 partition by list columns(id1) (
    partition p0 values in (1, 2, 3, 4, 5, 6, 7, 8, 9, 10),
    partition p1 values in (11, 12, 13, 14, 15, 16, 17, 18, 19, 20)
);
create table pg_trans_test4_2(id1 int, id2 int) tablegroup tg4 partition by list columns(id1) (
    partition p0 values in (1, 2, 3, 4, 5, 6, 7, 8, 9, 10),
    partition p1 values in (11, 12, 13, 14, 15, 16, 17, 18, 19, 20)
);

--LIST分区 (oracle mode)
create tablegroup tg4 binding true partition by list columns 1 (
    partition p0 values (1, 2, 3, 4, 5, 6, 7, 8, 9, 10),
    partition p1 values (11, 12, 13, 14, 15, 16, 17, 18, 19, 20)
);
create table pg_trans_test4_1(id1 int, id2 int) tablegroup tg4 partition by list(id1) (
    partition p0 values (1, 2, 3, 4, 5, 6, 7, 8, 9, 10),
    partition p1 values (11, 12, 13, 14, 15, 16, 17, 18, 19, 20)
);
create table pg_trans_test4_2(id1 int, id2 int) tablegroup tg4 partition by list(id1) (
    partition p0 values (1, 2, 3, 4, 5, 6, 7, 8, 9, 10),
    partition p1 values (11, 12, 13, 14, 15, 16, 17, 18, 19, 20)
);
```

### Primary Zone 为 RANDOM

RANDOM 部署，该租户的表 Leader 随机打散，分布式跨机事务同样是大概率的。

## 事务提交优化方案

### 单表 & 多表单机事务

单机事务的提交流程，为了提升系统的吞吐能力，可以调整如下配置项：

- 对于读写混合的场景，可在系统租户下打开如下配置项：

  ```unknow
  alter system set _enable_clog_rpc_aggregation = true;
  ```

- 根据实际情况，在系统租户下适当调整未确认日志的滑动窗口大小（默认 1500，适当调大）：

  ```unknow
  alter system set clog_max_unconfirmed_log_count = 5000;
  ```

### 跨机事务

主要解决如下两个问题：

1. 尽可能利用多机能力。
2. OBServer 流量负载均衡。

具体手段：

- 为了避免跨机事务，结合事务内语句执行情况，进行 table group 划分，尽量保证事务单机执行。具体请参见 **primary zone 为单 zone & 多unit** 。
- 对于批量导入的场景，尽可能利用 PDML 并行执行的能力（ 3.2 及之后的版本）。
- 根据负载情况调整网络线程数量（ `net_thread_count` )，该配置项默认为 0，进程启动之后，根据当前机器 CPU 核数，自适应计算出本机需要的数量，公式如下：`min( 6, cpu_core/8 )`；根据实际的情况，手动调整预期值，进程重启生效。

### 注意事项

- V2.2.x 及之前的版本，不允许打开一阶段提交优化。
- V3.2.x 及之前的版本，不建议生产环境用 Partition Group 优化，性能调优可以提高性能。





主备库资源不对等