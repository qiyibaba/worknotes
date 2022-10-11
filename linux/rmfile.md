# 删除10天前的文件

```
find ./ -type f -mtime +10 -exec rm {} ;
odp的sql-digest日志查找执行超过2s的sql
awk -F ',' '{if ($19 > 2000000) print $1,$19}' sql-digest.log
替换字符串
tr "|" " "
```

