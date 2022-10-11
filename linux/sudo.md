# 配置admin用户sudo免密登录

1. 登陆或切换到root用户下

2. 添加sudo文件的写权限，命令是:chmod u+w /etc/sudoers

3. 编辑sudoers文件：vi /etc/sudoers

   ```
   找到这行 root ALL=(ALL) ALL,在他下面添加如下命令
   admin       ALL=(ALL)         
   NOPASSWD: ALL
   ```

4. 撤销sudoers文件写权限,命令:chmod u-w /etc/sudoers