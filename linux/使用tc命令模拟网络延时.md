```sh
[root@h07b09255.sqa.eu95 /home/ningyue.lt]
#ping -c 4 11.166.78.70
PING 11.166.78.70 (11.166.78.70) 56(84) bytes of data.
64 bytes from 11.166.78.70: icmp_seq=1 ttl=61 time=0.150 ms
64 bytes from 11.166.78.70: icmp_seq=2 ttl=61 time=0.140 ms
64 bytes from 11.166.78.70: icmp_seq=3 ttl=61 time=0.155 ms
64 bytes from 11.166.78.70: icmp_seq=4 ttl=61 time=0.141 ms

--- 11.166.78.70 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3001ms
rtt min/avg/max/mdev = 0.140/0.146/0.155/0.013 ms

[root@h07b09255.sqa.eu95 /home/ningyue.lt]
#tc qdisc show
qdisc mq 0: dev eth2 root
qdisc mq 0: dev eth3 root

[root@h07b09255.sqa.eu95 /home/ningyue.lt]
#tc qdisc add dev bond0 root netem delay 30ms

[root@h07b09255.sqa.eu95 /home/ningyue.lt]
#tc qdisc show
qdisc mq 0: dev eth2 root
qdisc mq 0: dev eth3 root
qdisc netem 8001: dev bond0 root refcnt 17 limit 1000 delay 30.0ms

[root@h07b09255.sqa.eu95 /home/ningyue.lt]
#ping -c 4 11.166.78.70
PING 11.166.78.70 (11.166.78.70) 56(84) bytes of data.
64 bytes from 11.166.78.70: icmp_seq=1 ttl=61 time=30.1 ms
64 bytes from 11.166.78.70: icmp_seq=2 ttl=61 time=30.1 ms
64 bytes from 11.166.78.70: icmp_seq=3 ttl=61 time=30.1 ms
64 bytes from 11.166.78.70: icmp_seq=4 ttl=61 time=30.1 ms

--- 11.166.78.70 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3002ms
rtt min/avg/max/mdev = 30.139/30.143/30.147/0.173 ms

[root@h07b09255.sqa.eu95 /home/ningyue.lt]
#tc qdisc change dev bond0 root netem delay 40ms

[root@h07b09255.sqa.eu95 /home/ningyue.lt]
#ping -c 4 11.166.78.70
PING 11.166.78.70 (11.166.78.70) 56(84) bytes of data.
64 bytes from 11.166.78.70: icmp_seq=1 ttl=61 time=40.1 ms
64 bytes from 11.166.78.70: icmp_seq=2 ttl=61 time=40.1 ms
64 bytes from 11.166.78.70: icmp_seq=3 ttl=61 time=40.1 ms
64 bytes from 11.166.78.70: icmp_seq=4 ttl=61 time=40.1 ms

--- 11.166.78.70 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3002ms
rtt min/avg/max/mdev = 40.116/40.122/40.129/0.245 ms

[root@h07b09255.sqa.eu95 /home/ningyue.lt]
#tc qdisc del dev bond0 root

[root@h07b09255.sqa.eu95 /home/ningyue.lt]
#ping -c 4 11.166.78.70
PING 11.166.78.70 (11.166.78.70) 56(84) bytes of data.
64 bytes from 11.166.78.70: icmp_seq=1 ttl=61 time=0.094 ms
64 bytes from 11.166.78.70: icmp_seq=2 ttl=61 time=0.115 ms
64 bytes from 11.166.78.70: icmp_seq=3 ttl=61 time=0.136 ms
64 bytes from 11.166.78.70: icmp_seq=4 ttl=61 time=0.136 ms

--- 11.166.78.70 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 2999ms
rtt min/avg/max/mdev = 0.094/0.120/0.136/0.019 ms
```
