# 配置时钟源

```
1) Chrony:
检查配置文件：/etc/chrony.conf，操作命令：
检查是否开机自启动: systemctl is-enabled chronyd.service
检查是否开启状态: systemctl status chronyd.service
开启 chronyd: systemctl start chronyd.service
关闭 chronyd:  systemctl stop chronyd.service
查看时间同步源: chronyc sources -v
查看时间同步源状态: chronyc sourcestats -v

2) NTP:
检查配置文件：/etc/ntp.conf，操作命令：
检查是否开机自启动: systemctl is-enabled ntpd.service
检查是否开启状态: systemctl status ntpd.service
开启 ntpd: systemctl start ntpd.service
关闭 ntpd:  systemctl stop ntpd.service

遍历Observer ip间时差，结果输出中rtt第2个ms代表时差，需要小于100ms。可以通过以下shell命令，或者直接通过clockdiff <ip>检查机器之间时差：
for i in ob_ip01 ob_ip02 ob_ip03 ob_ip04 ob_ip05 ob_ip06 ob_ip07 ob_ip08 ob_ip09 ob_ip10 ob_ip11 ob_ip12
do
    clockdiff ${i}
done
注：ob_ip01/ob_ip02/… 需要替换为对应节点IP
```

