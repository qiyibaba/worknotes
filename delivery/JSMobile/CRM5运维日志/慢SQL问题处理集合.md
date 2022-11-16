```plsql
select '',
b.be_id,--地市
b.service_number,--号码
b.status,--用户状态
b.subs_id,--用户id
a.offering_id,--融合套餐id
to_char(a.eff_date,'yyyymmddhh24miss'),--生效时间
to_char(a.exp_date,'yyyymmddhh24miss'),--失效时间
decode(b.offering_id,'2000015753','2000015753','2000015754','2000015754', '2000015755','2000015755',
'2000015756','2000015756','2000015757','2000015757','2000015758','2000015758', '2000015759','2000015759',''),--用户主商品
to_char(d.eff_date,'yyyymmddhh24miss') ,--主商品生效时间
to_char(d.exp_date,'yyyymmddhh24miss'),--主商品失效时间
d.status --主套餐状态
from SUBS.inf_offering_inst a ,subs.inf_offer_inst_relation n,subs.inf_subscriber b ,SUBS.inf_offering_inst d where
a.offering_id in(SELECT c.item_code from sysmgr.sys_datadict_item c where c.dict_code = 'GRP_YWT_COMP_APPEND_OFFER' AND c.description like chr(37)||'2400001002'||chr(37))
and a.offering_inst_id = n.src_offering_inst_id and n.dest_offering_id ='2400001002'
and d.primary_flag ='Y'
and a.subs_id = d.subs_id
and b.subs_id = a.subs_id
and a.be_id=b.be_id
and a.be_id=23
and to_char(a.modify_time, 'yyyy/mm/dd') >=
to_char(to_char(TRUNC(add_months(trunc(sysdate), -3), 'MM'),
'yyyy/mm/dd'))
and to_char(a.modify_time, 'yyyy/mm/dd') <
to_char(TRUNC(add_months(trunc(sysdate), 0), 'MM'), 'yyyy/mm/dd')
```

```
MySQL [oceanbase]> select * from gv$sql_audit where sql_id='A9DE33516565B74DC1877E139009E526' limit 1\G;
*************************** 1. row ***************************
                 SVR_IP: 10.32.208.34
               SVR_PORT: 2882
             REQUEST_ID: 3782803630
            SQL_EXEC_ID: 7146333983
               TRACE_ID: YB420A20D022-0005EBC012FFEEBF-0-0
                    SID: 3224047347
              CLIENT_IP: 10.32.208.45
            CLIENT_PORT: 55106
              TENANT_ID: 1002
            TENANT_NAME: crm5
    EFFECTIVE_TENANT_ID: 1002
                USER_ID: 1101710651032554
              USER_NAME: SUBS
             USER_GROUP: 0
         USER_CLIENT_IP: 10.33.198.206
                  DB_ID: 1101710651032604
                DB_NAME: SUBS
                 SQL_ID: A9DE33516565B74DC1877E139009E526
              QUERY_SQL: select '',
b.be_id,--地市
b.service_number,--号码
b.status,--用户状态
b.subs_id,--用户id
a.offering_id,--融合套餐id
to_char(a.eff_date,'yyyymmddhh24miss'),--生效时间
to_char(a.exp_date,'yyyymmddhh24miss'),--失效时间
decode(b.offering_id,'2000015753','2000015753','2000015754','2000015754', '2000015755','2000015755',
'2000015756','2000015756','2000015757','2000015757','2000015758','2000015758', '2000015759','2000015759',''),--用户主商品
to_char(d.eff_date,'yyyymmddhh24miss') ,--主商品生效时间
to_char(d.exp_date,'yyyymmddhh24miss'),--主商品失效时间
d.status --主套餐状态
from SUBS.inf_offering_inst a ,subs.inf_offer_inst_relation n,subs.inf_subscriber b ,SUBS.inf_offering_inst d where
a.offering_id in(SELECT c.item_code from sysmgr.sys_datadict_item c where c.dict_code = 'GRP_YWT_COMP_APPEND_OFFER' AND c.description like chr(37)||'2400001002'||chr(37))
and a.offering_inst_id = n.src_offering_inst_id and n.dest_offering_id ='2400001002'
and d.primary_flag ='Y'
and a.subs_id = d.subs_id
and b.subs_id = a.subs_id
and a.be_id=b.be_id
and a.be_id=23
and to_char(a.modify_time, 'yyyy/mm/dd') >=
to_char(to_char(TRUNC(add_months(trunc(sysdate), -3), 'MM'),
'yyyy/mm/dd'))
and to_char(a.modify_time, 'yyyy/mm/dd') <
to_char(TRUNC(add_months(trunc(sysdate), 0), 'MM'), 'yyyy/mm/dd')
                PLAN_ID: 1877492
          AFFECTED_ROWS: 0
            RETURN_ROWS: 4139
          PARTITION_CNT: 67
               RET_CODE: 0
                  QC_ID: 0
                 DFO_ID: 0
                 SQC_ID: 0
              WORKER_ID: 0
                  EVENT: system internal wait
                 P1TEXT:
                     P1: 0
                 P2TEXT:
                     P2: 0
                 P3TEXT:
                     P3: 0
                  LEVEL: 0
          WAIT_CLASS_ID: 100
            WAIT_CLASS#: 0
             WAIT_CLASS: OTHER
                  STATE: MAX_WAIT TIME ZERO
        WAIT_TIME_MICRO: 0
  TOTAL_WAIT_TIME_MICRO: 0
            TOTAL_WAITS: 0
              RPC_COUNT: 4
              PLAN_TYPE: 3
           IS_INNER_SQL: 1
        IS_EXECUTOR_RPC: 0
            IS_HIT_PLAN: 0
           REQUEST_TIME: 1667807546295908
           ELAPSED_TIME: 42782730
               NET_TIME: 0
          NET_WAIT_TIME: 0
             QUEUE_TIME: 0
            DECODE_TIME: 0
          GET_PLAN_TIME: 0
           EXECUTE_TIME: 42782730
  APPLICATION_WAIT_TIME: 0
  CONCURRENCY_WAIT_TIME: 0
      USER_IO_WAIT_TIME: 0
          SCHEDULE_TIME: 0
          ROW_CACHE_HIT: 0
 BLOOM_FILTER_CACHE_HIT: 0
        BLOCK_CACHE_HIT: 15
  BLOCK_INDEX_CACHE_HIT: 25
             DISK_READS: 2
              RETRY_CNT: 0
             TABLE_SCAN: 1
      CONSISTENCY_LEVEL: 3
MEMSTORE_READ_ROW_COUNT: 4
 SSSTORE_READ_ROW_COUNT: 4
    REQUEST_MEMORY_USED: 2096128
  EXPECTED_WORKER_COUNT: 0
      USED_WORKER_COUNT: 0
             SCHED_INFO: NULL
     FUSE_ROW_CACHE_HIT: 0
             PS_STMT_ID: 2
       TRANSACTION_HASH: 0
           REQUEST_TYPE: 1
  IS_BATCHED_MULTI_STMT: 0
          OB_TRACE_INFO: NULL
              PLAN_HASH: 14927115034292358369
     LOCK_FOR_READ_TIME: 0
  WAIT_TRX_MIGRATE_TIME: 0
           PARAMS_VALUE:
1 row in set (17.78 sec)
```

```
MySQL [oceanbase]> select * from gv$sql_audit where sql_id='13E2FBDF804BF18A9753B9EAAB4A34DA' limit 1\G;
*************************** 1. row ***************************
                 SVR_IP: 10.32.208.34
               SVR_PORT: 2882
             REQUEST_ID: 3697415669
            SQL_EXEC_ID: 6985995876
               TRACE_ID: YB420A20D022-0005EBC015CA3B4A-0-0
                    SID: 3224102069
              CLIENT_IP: 10.32.208.45
            CLIENT_PORT: 21012
              TENANT_ID: 1002
            TENANT_NAME: crm5
    EFFECTIVE_TENANT_ID: 1002
                USER_ID: 1101710651032556
              USER_NAME: BESAUDIT
             USER_GROUP: 0
         USER_CLIENT_IP: 172.20.79.1
                  DB_ID: 1101710651032606
                DB_NAME: BESAUDIT
                 SQL_ID: 13E2FBDF804BF18A9753B9EAAB4A34DA
              QUERY_SQL: select a.settleoid,a.attachdate,a.createoperid, (select opername from fa_operator_cz p where p.operid=a.createoperid and rownum=1) createopername, a.createdate,a.status,a.statusdate, a.auditoperid, (select dictname from fa_dict_item_cz where groupid='fastatus' and dictid=a.status and rownum=1) statusname, (select opername from fa_operator_cz p where p.operid=a.auditoperid and rownum=1) auditopername, a.region,a.parentsettleoid, (select orgname from fa_organization_cz c where c.ORGID=b.orgid and rownum=1) orgname, b.orgid,b.shouldfee,b.factfee,b.adjustfee, a.ctry_orgid, (select distname orgname from icdpub.T_UCP_DISTRICT c where c.distid=a.ctry_orgid and rownum=1) ctry_orgname, a.remark  from fa_st_list_CZ a,fa_st_incomelist_cz b,fa_organization_cfg_CZ d where a.region = ? and b.region = ? and b.attachdate between ? and ? and b.settleoid = a.settleoid and a.attachdate between ? and ? and a.nodeid = 'OrgIn' and a.settlelisttype = 'fasltIn'  and d.orgid=a.createorgid  and d.region=? and d.toauditoperid=?  and a.status = ? and a.ctry_orgid=? order by attachdate,orgid
                PLAN_ID: 1756569
          AFFECTED_ROWS: 0
            RETURN_ROWS: 58
          PARTITION_CNT: 8
               RET_CODE: 0
                  QC_ID: 0
                 DFO_ID: 0
                 SQC_ID: 0
              WORKER_ID: 0
                  EVENT: system internal wait
                 P1TEXT:
                     P1: 0
                 P2TEXT:
                     P2: 0
                 P3TEXT:
                     P3: 0
                  LEVEL: 0
          WAIT_CLASS_ID: 100
            WAIT_CLASS#: 0
             WAIT_CLASS: OTHER
                  STATE: MAX_WAIT TIME ZERO
        WAIT_TIME_MICRO: 0
  TOTAL_WAIT_TIME_MICRO: 0
            TOTAL_WAITS: 0
              RPC_COUNT: 0
              PLAN_TYPE: 1
           IS_INNER_SQL: 1
        IS_EXECUTOR_RPC: 0
            IS_HIT_PLAN: 1
           REQUEST_TIME: 1667803231945532
           ELAPSED_TIME: 22833017
               NET_TIME: 0
          NET_WAIT_TIME: 0
             QUEUE_TIME: 0
            DECODE_TIME: 0
          GET_PLAN_TIME: 0
           EXECUTE_TIME: 22833017
  APPLICATION_WAIT_TIME: 0
  CONCURRENCY_WAIT_TIME: 0
      USER_IO_WAIT_TIME: 0
          SCHEDULE_TIME: 0
          ROW_CACHE_HIT: 4268
 BLOOM_FILTER_CACHE_HIT: 0
        BLOCK_CACHE_HIT: 95010
  BLOCK_INDEX_CACHE_HIT: 19
             DISK_READS: 0
              RETRY_CNT: 0
             TABLE_SCAN: 1
      CONSISTENCY_LEVEL: 3
MEMSTORE_READ_ROW_COUNT: 8016
 SSSTORE_READ_ROW_COUNT: 15330148
    REQUEST_MEMORY_USED: 2096128
  EXPECTED_WORKER_COUNT: 0
      USED_WORKER_COUNT: 0
             SCHED_INFO: NULL
     FUSE_ROW_CACHE_HIT: 0
             PS_STMT_ID: 31
       TRANSACTION_HASH: 0
           REQUEST_TYPE: 1
  IS_BATCHED_MULTI_STMT: 0
          OB_TRACE_INFO: NULL
              PLAN_HASH: 8547570077202138121
     LOCK_FOR_READ_TIME: 0
  WAIT_TRX_MIGRATE_TIME: 0
           PARAMS_VALUE:
```

select a.settleoid,a.attachdate,a.createoperid, (select opername from fa_operator_cz p where p.operid=a.createoperid and rownum=1) createopername, a.createdate,a.status,a.statusdate, a.auditoperid, (select dictname from fa_dict_item_cz where groupid='fastatus' and dictid=a.status and rownum=1) statusname, (select opername from fa_operator_cz p where p.operid=a.auditoperid and rownum=1) auditopername, a.region,a.parentsettleoid, (select orgname from fa_organization_cz c where c.ORGID=b.orgid and rownum=1) orgname, b.orgid,b.shouldfee,b.factfee,b.adjustfee, a.ctry_orgid, (select distname orgname from icdpub.T_UCP_DISTRICT c where c.distid=a.ctry_orgid and rownum=1) ctry_orgname, a.remark from fa_st_list_CZ a,fa_st_incomelist_cz b,fa_organization_cfg_CZ d where a.region = ? and b.region = ? and b.attachdate between ? and ? and b.settleoid = a.settleoid and a.attachdate between ? and ? and a.nodeid = 'OrgIn' and a.settlelisttype = 'fasltIn' and d.orgid=a.createorgid and d.region=? and d.toauditoperid=? and a.status = ? and a.ctry_orgid=? order by attachdate,orgid
 PLAN_ID
