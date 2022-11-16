![](/Users/qiyibaba/gitbook/image/2022-11-04-20-36-26-image.png)

![](/Users/qiyibaba/gitbook/image/2022-11-04-20-40-12-image.png)

```
MySQL [oceanbase]> select trans_id,count(*) from __all_virtual_trans_stat group by trans_id order by 2 desc limit 10;
+------------------------------------------------------------------------------------------+----------+
| trans_id                                                                                 | count(*) |
+------------------------------------------------------------------------------------------+----------+
| {hash:17380076687319239664, inc:105840315, addr:"10.32.208.34:2882", t:1667547072085149} |      432 |
| {hash:17348196177677393584, inc:105839909, addr:"10.32.208.34:2882", t:1667547071989496} |      432 |
| {hash:2548449697224222815, inc:105841125, addr:"10.32.208.34:2882", t:1667547072259706}  |      432 |
| {hash:5301075230828397704, inc:105840272, addr:"10.32.208.34:2882", t:1667547072073278}  |      432 |
| {hash:10892092977170909703, inc:150933603, addr:"10.32.208.34:2882", t:1667566981719867} |      188 |
| {hash:4627395438577152242, inc:11433893, addr:"10.33.221.28:2882", t:1667545707918564}   |      156 |
| {hash:11847260290691700766, inc:105794716, addr:"10.32.208.34:2882", t:1667547062797971} |       96 |
| {hash:16813454142363297258, inc:101362746, addr:"10.32.208.33:2882", t:1667520210172907} |       87 |
| {hash:11764477542585570064, inc:105573553, addr:"10.32.208.33:2882", t:1667538976563108} |       87 |
| {hash:3542344340110079168, inc:102579844, addr:"10.32.208.33:2882", t:1667524362470256}  |       87 |
+------------------------------------------------------------------------------------------+----------+
10 rows in set (0.15 sec)

MySQL [oceanbase]> select usec_to_time(1667547072085149);
+--------------------------------+
| usec_to_time(1667547072085149) |
+--------------------------------+
| 2022-11-04 15:31:12.085149     |
+--------------------------------+
1 row in set (0.00 sec)

MySQL [oceanbase]> select usec_to_time(1667547071989496);
+--------------------------------+
| usec_to_time(1667547071989496) |
+--------------------------------+
| 2022-11-04 15:31:11.989496     |
+--------------------------------+
1 row in set (0.01 sec)

MySQL [oceanbase]> select usec_to_time(1667547072259706);
+--------------------------------+
| usec_to_time(1667547072259706) |
+--------------------------------+
| 2022-11-04 15:31:12.259706     |
+--------------------------------+
1 row in set (0.00 sec)

MySQL [oceanbase]> select usec_to_time(1667547072073278);
+--------------------------------+
| usec_to_time(1667547072073278) |
+--------------------------------+
| 2022-11-04 15:31:12.073278     |
+--------------------------------+
1 row in set (0.00 sec)
```

[2022-11-04 15:32:28.931857] INFO  [SERVER.OMT] ob_multi_tenant.cpp:819 [171865][0][Y0-0000000000000000-0-0] [lt=10] [dc=0] dump tenant info(tenant={id:1002, compat_mode:1, unit_min_cpu:"4.800000000000000000e+01", unit_max_cpu:"4.800000000000000000e+01", slice:"0.000000000000000000e+00", slice_remain:"0.000000000000000000e+00", token_cnt:192, sug_token_cnt:192, ass_token_cnt:192, lq_tokens:57, used_lq_tokens:14, stopped:false, idle_us:4279017, recv_hp_rpc_cnt:132566255, recv_np_rpc_cnt:89738969, recv_lp_rpc_cnt:0, recv_mysql_ps_close_cnt:110957329, recv_mysql_cnt:351331598, recv_task_cnt:359507, recv_large_req_cnt:94527, tt_large_quries:32929159, pop_normal_cnt:13507640047, actives:192, workers:192, nesting workers:7, lq waiting workers:0, req_queue:total_size=56409 queue[0]=29067 queue[1]=0 queue[2]=4653 queue[3]=15 queue[4]=22674 queue[5]=0 , large queued:0, reserve queued:189, multi_level_queue:total_size=0 queue[0]=0 queue[1]=0 queue[2]=0 queue[3]=0 queue[4]=0 queue[5]=0 queue[6]=0 queue[7]=0 , recv_level_rpc_cnt:cnt[0]=0 cnt[1]=0 cnt[2]=29365 cnt[3]=0 cnt[4]=0 cnt[5]=3186494 cnt[6]=0 cnt[7]=0 , group_map:null, rpc_stat_info: pcode=0x710:cnt=172180 pcode=0x51f:cnt=70046 pcode=0x750:cnt=63205 pcode=0x701:cnt=49733 pcode=0x51c:cnt=38478})

 {tid:
 {tid:
 {tid:
 {tid:
 {tid:
 {tid:
 {tid:
 {tid:

select table_name from gv$table where table_id in ('1101710651102127','1101710651101125','1101710651102410','1101710651110113','1101710651082497','1101710651110063','1101710651110053','1101710651110520');

select a.table_name,a.table_id,count(*) from __all_virtual_partition_item a left join gv$partition b on a.partition_id = b.partition_id and a.table_id = b.table_id where a.table_id in ('1101710651102127','1101710651101125','1101710651102410','1101710651110113','1101710651082497','1101710651110063','1101710651110053','1101710651110520') and b.role = 1 group by table_id;

```
MySQL [oceanbase]> select a.table_name,a.table_id,count(*) from __all_virtual_partition_item a left join gv$partition b on a.partition_id = b.partition_id and a.table_id = b.table_id where a.table_id in ('1101710651102127','1101710651101125','1101710651102410','1101710651110113','1101710651082497','1101710651110063','1101710651110053','1101710651110520') and b.role = 1 group by table_id;
+---------------------------+------------------+----------+
| table_name                | table_id         | count(*) |
+---------------------------+------------------+----------+
| SR_RECEPTION              | 1101710651082497 |        8 |
| INT_SYNCINFO_UPLOADDTL_EX | 1101710651101125 |       16 |
| INT_SYNCINFO_MODIFYTIME   | 1101710651102127 |        1 |
| INT_SYNCINFO_UPLOADLOG_EX | 1101710651102410 |      224 |
| PM_ENTITY_ATTR            | 1101710651110053 |        1 |
| PM_OFFERING               | 1101710651110063 |        1 |
| SYS_DATADICT_ITEM         | 1101710651110113 |        1 |
| INF_OFFERING_INST         | 1101710651110520 |       32 |
+---------------------------+------------------+----------+
8 rows in set (3.70 sec)
```
