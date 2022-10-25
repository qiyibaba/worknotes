# OB_DNS负载均衡使用

```
负载均衡配置
域名配置
#进入obdns的docker
docker exec -it ob_dns bash
#添加域名
cd /var/named/ob_dns_monitor
ob_dns_ops.py add dev.oceanbase.com -t http -r <IP_LIST> -l 2883:2883

客户端机器配置/etc/resolv.conf
#在/etc/resolv.conf中添加内容,3节点OCP添加三行地址信息
nameserver <OCP地址>

例：开发环境配置指导
OCP：10.165.12.123,10.165.12.124,10.165.12.125

OBPROXY：10.165.12.120,10.165.12.121,10.165.12.122

域名配置
#进入obdns的docker
docker exec -it ob_dns bash
#添加域名
cd /var/named/ob_dns_monitor
ob_dns_ops.py add dev.oceanbase.com -t http -r 10.165.12.120,10.165.12.121,10.165.12.122 -l 2883:2883
#验证添加成功
ob_dns_ops.py list http

客户端机器配置/etc/resolv.conf
#在/etc/resolv.conf中添加内容,3节点OCP添加三行地址信息
nameserver 10.165.12.123
nameserver 10.165.12.124
nameserver 10.165.12.125

验证登录
mysql -uroot@kf_mysql_tent#kaifa_cluster -hdev.oceanbase.com -P2883 -p
```
