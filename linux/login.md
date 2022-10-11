# 开通 root远程登录访问

1. 编辑配置文件vim /etc/ssh/sshd_config修改PermitRootLogin后面的值为yes

2. 重启sshd服务

   ```shell
   systemctl restart sshd.service
   ```

   