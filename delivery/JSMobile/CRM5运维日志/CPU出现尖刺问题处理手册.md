1. 业务发生RT尖刺

2. 连接到持久化的sqlaudit
   
   1. 登录服务器10.32.57.17，进入目录”/oma/sqlauditstore2/store“,选择对应时间段的文件，如果已经被打包，执行解压缩”tar zxvf xxxx.tar.gz“
   
   2. 导入数据到性能库：/home/admin/ob-loader-dumper-3.1.0-SNAPSHOT/bin/obloader --csv -h10.33.221.183 -P 2883 -c crmdb5_XN -t sqlauditstore -u root -p 'ROot..123' -D sqlaudit -f '/oma/sqlauditstore2/store/crmdb1-1002.csv'  --table crmdb1_crm5_sqlaudit_110916 --sys-user root --sys-password 'ROot..123' --external-data   --column-delimiter '"'

3. 按RT尖刺的时间范围在持久化的sqlaudit中拉取可疑sql
   
   ```plsql
   select tenant_id, tenant_name, sql_id, query_sql, usec_to_time(request_time), elapsed_time , returnrow, execute_time, queue_time, sid,transaction_hash,plan_type 
   from gv$sql_audit t where 
   tenant_id = 1002 and
   request_time > time_to_usec('2022-11-09 10:07:05')
   /*and request_time < time_to_usec('2022-11-08 09:01:00')*/
   order by execute_time desc
   ```

4. 把捞取的可以sql在测试环境执行，做对比
   
   ```
   性能测试环境：
   obclient -h10.33.221.183 -P2883 -uBESAUDIT@crm5#crmdb5_XN:1666545522 -p'HWhe123%%'   
   obclient -h10.33.221.183 -P2883 -uBESGRPCUST@crm5#crmdb5_XN:1666545522 -p'HWhe123%%'   
   obclient -h10.33.221.183 -P2883 -uBESOPM@crm5#crmdb5_XN:1666545522 -p'HWhe123%%'                
   obclient -h10.33.221.183 -P2883 -uBESREPORT@crm5#crmdb5_XN:1666545522 -p'HWhe123%%'    
   obclient -h10.33.221.183 -P2883 -uBOSSPRO@crm5#crmdb5_XN:1666545522 -p'HWhe123%%'  
   obclient -h10.33.221.183 -P2883 -uCUST@crm5#crmdb5_XN:1666545522 -p'HWhe123%%'   
   obclient -h10.33.221.183 -P2883 -uLOGON@crm5#crmdb5_XN:1666545522 -p'HWhe123%%'   
   obclient -h10.33.221.183 -P2883 -uPROD@crm5#crmdb5_XN:1666545522 -p'HWhe123%%'   
   obclient -h10.33.221.183 -P2883 -uSUBS@crm5#crmdb5_XN:1666545522 -p'HWhe123%%'   
   obclient -h10.33.221.183 -P2883 -uUBESMGR@crm5#crmdb5_XN:1666545522 -p'HWhe123%%' 
   obclient -h10.33.221.183 -P2883 -uBESCHARGE@crm5#crmdb5_XN:1666545522 -p'HWhe123%%'   
   obclient -h10.33.221.183 -P2883 -uBESINVOICE@crm5#crmdb5_XN:1666545522 -p'HWhe123%%' 
   obclient -h10.33.221.183 -P2883 -uBESRECEIPT@crm5#crmdb5_XN:1666545522 -p'HWhe123%%'  
   obclient -h10.33.221.183 -P2883 -uBESSMS@crm5#crmdb5_XN:1666545522 -p'HWhe123%%'      
   obclient -h10.33.221.183 -P2883 -uCOMMON@crm5#crmdb5_XN:1666545522 -p'HWhe123%%'  
   obclient -h10.33.221.183 -P2883 -uICDPUB@crm5#crmdb5_XN:1666545522 -p'HWhe123%%' 
   obclient -h10.33.221.183 -P2883 -uORDERS@crm5#crmdb5_XN:1666545522 -p'HWhe123%%'    
   obclient -h10.33.221.183 -P2883 -uREPLX@crm5#crmdb5_XN:1666545522 -p'HWhe123%%'  
   obclient -h10.33.221.183 -P2883 -uSYSMGR@crm5#crmdb5_XN:1666545522 -p'HWhe123%%'  
   obclient -h10.33.221.183 -P2883 -uUNIFILE@crm5#crmdb5_XN:1666545522 -p'HWhe123%%'  
   obclient -h10.33.221.183 -P2883 -usys@crm5#crmdb5_XN:1666545522 -p'ROot..123'
   ```

5. 输出对比结果
   
   ```
   select tenant_id, tenant_name, sql_id, query_sql, usec_to_time(request_time), elapsed_time , returnrow, execute_time, queue_time, sid,transaction_hash,plan_type 
   from gv$sql_audit t where 
   tenant_id = 1002 and
   request_time > time_to_usec('2022-11-09 10:07:05')
   /*and request_time < time_to_usec('2022-11-08 09:01:00')*/
   order by execute_time desc
   ```

6. 根据对比结果进行优化或者业务调整
   a. 执行计划走偏
   b. 索引低效
   c. 大结果集
   ....
