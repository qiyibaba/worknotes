# 配置主机信息

1. **在每台服务器上执行以下命令，为服务器配置主机名**

   ```
   hostnamectl set-hostname <hostname>
   ```

2. **修改 /etc/hosts 配置文件，指定当前服务器的 IP 地址和主机名**

   ```
   vi /etc/hosts
   ```

3. **执行以下命令，确保可以正确获取当前主机的 IP 地址**

   ```
   hostname -i
   ```

