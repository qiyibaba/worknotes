# 绑定outline

1. 定位问题SQL：

root@sys登录集群，查看最近10分钟的TOP10 SQL（【租户ID】替换成真正的租户ID；ORDER BY时间排序的纬度可以按需换成SELECT LIST中的elapsed_time【总的执行时间】、execute_time【真正执行时间】、queue_time【排队时间】）：

```
select /*+READ_CONSISTENCY(WEAK), QUERY_TIMEOUT(100000000), PARALLEL(4)*/ sql_id, substr(query_sql, 1, 50) query_sql, t1.svr_ip, count(*)/600 as QPS,
  AVG(elapsed_time),
  AVG(execute_time),
  AVG(queue_time)
from oceanbase.gv$sql_audit t1, __all_server t2  
where t1.svr_ip = t2.svr_ip and IS_EXECUTOR_RPC = 0    
      and request_time > (time_to_usec(now()) - 600000000)    
      and request_time < time_to_usec(now())
      and tenant_id=【租户ID】
GROUP BY
  sql_id
ORDER BY
  AVG(execute_time) DESC
LIMIT 10;
```

2. root用户登录业务租户，绑定outline：

注：这里的sql_id填入上一步中定位到的问题sql的sql_id，index(【table_name】 【index_name】) 中替换成表名、索引名，如表名有别名必须用别名。

```
use 【dbname】;     --dbname替换成对应的数据库名，切记一定要进入到数据库中，否则outline无法绑定成功
create outline 【outline_name】 on '【sql_id】' using hint /*+ index(【table_name】 【index_name】) */
```

3. 验证outline是否创建成功，以及语句的执行是否走对了执行计划：
- 查看outline是否创建成功：

```
select * from __all_outline where tenant_id=【tenant_id】G
```

- 从与执行计划相关的性能视图中找到sql_id对应的plan_id：

```
select * from gv$plan_cache_plan_stat where sql_id='【sql_id】'G
```

- 将上一步获得的plan_id带入以下查询，确认绑定生效，走了正确的执行计划：

```
select plan_id, operator, name, rows, cost from gv$plan_cache_plan_explain where tenant_id=【tenant_id】 and ip='xxx.xxx.xxx.xxx' and port=2882 and plan_id=【plan_id】;
```