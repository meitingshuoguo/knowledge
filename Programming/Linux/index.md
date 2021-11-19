# 拥有一个VPS之后

1. 先用密码登录`ssh @ip`

2. 修改主机名称 `hostnamectl set-hostname yourhostname`

3. 安装nginx

   ```bash
   sudo yum install epel-release
   sudo yum install nginx
   
   sudo firewall-cmd --permanent --zone=public --add-service=http 
   sudo firewall-cmd --permanent --zone=public --add-service=https
   sudo firewall-cmd --reload
   
   //启动/停止/重启/查看状态
   sudo systemctl [start/stop/restart/status] nginx
   
   //【启用/禁用】开机启动
   sudo systemctl [enable/disable] nginx
   
   ```

4. 使用ssh key 登录

   1. 本地生成一对key

      `ssh-keygen -t rsa`

   2. 把公钥上传到服务器（可能要先在服务器上新建.ssh文件夹）

      `scp ~/.ssh/kamatera_react_rsa.pub root@xxx:/root/.ssh/`

   3. 在服务器上执行以下命令

      ```bash
      mv .ssh/id_rsa.pub .ssh/authorized_keys
      chmod 700 .ssh
      chmod 600 .ssh/authorized_keys
      
      service sshd restart
      
      //PasswordAuthentication no  关闭密码登录
      ```

   4. 使用key登录

      在本地的软件中（比如mac下的Rolay TSX）中配置ssh key的登录方式。

      下面这个命令可以来测试是不是配置正确了

      `ssh root@xxx.xxx.xxx -p [port] -i ~/.ssh/id_rsa`

5. 开启bbr

   ```bash
   wget --no-check-certificate -O /opt/bbr.sh https://github.com/teddysun/across/raw/master/bbr.sh
   chmod 755 /opt/bbr.sh
   /opt/bbr.sh
   
   //验证
   lsmod | grep bbr
   ```

   
