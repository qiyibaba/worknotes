基础条件：

- 硬件
  
  - 4路机器

- 软件
  
  - xa路由功能
  
  - 限流功能，并发控制

- 测试
  
  - 压测平台
  
  - 回放

数据拆分手段：

- tablegroup

- 人工运维

部署架构：4-4-4，primary zone=”zone1,zone2“（2路机器），按照地市+业务场景拆分

```plsql
MySQL [oceanbase]> select database_name,count(*) from gv$table where table_type=3 group by database_name order by 2;
+------------------+----------+
| database_name    | count(*) |
+------------------+----------+
| FWXY             |        1 |
| OB_TEST3         |        1 |
| LOGON            |        1 |
| sqlaudit_config2 |        3 |
| sqlaudit_config  |        3 |
| SYS              |        4 |
| BESREPORT        |        5 |
| OMS_TEST         |        6 |
| test             |        7 |
| CUST             |       20 |
| PROD             |       21 |
| REPLX            |       24 |
| sqlaudit         |       31 |
| BESOPM           |       36 |
| BESRECEIPT       |       42 |
| BESGRPCUST       |       68 |
| SYSMGR           |       76 |
| BESINVOICE       |       78 |
| BESSMS           |      112 |
| BESAUDIT         |      143 |
| BESCHARGE        |      144 |
| UNIFILE          |      198 |
| ICDPUB           |      384 |
| SUBS             |      642 |
| ORDERS           |     1139 |
| COMMON           |     3508 |
+------------------+----------+
26 rows in set (0.06 sec)
```

- orders(南通+泰州+盐城) 

- SUBS+ICDPUB(南通+泰州+盐城)

- common

- 其他

部署架构：3-3-3（4路机器）

- 单个地市一台机器

- 按照业务拆分（subs，orders）

分区规划建议：

```plsql
MySQL [oceanbase]> select count(distinct table_id) table_id from __all_virtual_column where tenant_id=1002 and column_name='be_id';
+----------+
| table_id |
+----------+
|     4020 |
+----------+
1 row in set (0.18 sec)
```

数据拆分规则：

南通+泰州+盐城

拆分租户

1. XA

2. GTS





CRM部署架构方案：

方案1：primary zone=”zone1,zone2“

- 节约机器，按照4-4-4架构部署CRM1

- zone级故障的时候，leader会全部迁移到另一个zone上，导致cpu会提升一倍，持续时间约2个小时，解决方案，将zone3迁移到南京，zone故障时将zone1的leader全部迁移到zone3

方案2：primary zone=”zone1“

- 部署架构为8-8-8，机器数比方案一增加一倍，zone级故障的切换后能够保证故障后，无需等待副本恢复，CPU也会保持稳定
  
  

CRM1按照地市拆分租户同步方案：

- 将oracle数据同时同步到三个租户中，同步结束后使用drop partition清理其他地市数据



回放性能统计：

1. 聚合

2. 打散
