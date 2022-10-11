# 命令行执行卡住分析

```
1、首先就是使用strace去追踪到底在哪里卡住了
strace df -h
2、如果没有strace命令则进行安装即可
yum install strace 或者apt install strace
```

