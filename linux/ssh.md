# 互信配置

```shell
#方法1：
ssh-keygen -t rsa #（一直y）
cat ~/.ssh/id_*.pub
echo "xxxxxx" > .ssh/.ssh/authorized_keys

#方法2：
#生成id_rsa.pub：
ssh-keygen -t rsa  #一路回车
#手工ssh互信配置：
/usr/bin/ssh-copy-id -i /home/admin/.ssh/id_rsa.pub ningyue.lt@11.166.79.27

#没有ssh-copy-id直接拷贝文件：
cat ~/.ssh/id_*.pub|ssh oracle@172.30.215.16 'cat>>.ssh/authorized_keys'
```

