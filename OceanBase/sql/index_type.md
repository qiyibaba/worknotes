# 表字典信息中index_type取值

```mysql
-- index type:
create table tt (a varchar(10) primary key,b varchar(10),c varchar(10),d varchar(10),e varchar(10),f varchar(10),g varchar(10),h varchar(10));
create index tt_idx_1 on tt (b) local;
create unique index tt_idx_2 on tt  (c) local;
create index tt_idx_3 on tt (d) global;
create unique index tt_idx_4 on tt (e) global;
CREATE INDEX tt_idx_5 ON tt(f) GLOBAL PARTITION BY hash(f) partitions 10 ;
CREATE unique INDEX tt_idx_6 ON tt(g) GLOBAL PARTITION BY hash(g) partitions 10 ;

select table_name,table_type,table_id,index_type,part_level from gv$table where table_name like '%1100611139474012%';
+---------------------------------+-----------+------------------+------------+------------+
| table_name                      | TableType | table_id         | IndexType  | part_level |
+---------------------------------+-----------+------------------+------------+------------+
| __idx_1100611139474012_TT_IDX_1 |         5 | 1100611139474013 |          1 |          0 | 
| __idx_1100611139474012_TT_IDX_2 |         5 | 1100611139474014 |          2 |          0 |
| __idx_1100611139474012_TT_IDX_3 |         5 | 1100611139474015 |          7 |          0 |
| __idx_1100611139474012_TT_IDX_4 |         5 | 1100611139474016 |          8 |          0 |
| __idx_1100611139474012_TT_IDX_5 |         5 | 1100611139474017 |          3 |          1 |
| __idx_1100611139474012_TT_IDX_6 |         5 | 1100611139474018 |          4 |          1 |
+---------------------------------+-----------+------------------+------------+------------+
6 rows in set (0.14 sec)
-- 1   -- 局部普通索引
-- 2   -- 局部唯一索引
-- 7   -- 不带分区的全局索引
-- 8   -- 不带分区的全局唯一索引
-- 3   -- 带分区的全局索引
-- 4   -- 带分区的全局唯一索引
```

