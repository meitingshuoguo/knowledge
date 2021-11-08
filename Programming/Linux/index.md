1. 买个vps

2. 先用密码登录`ssh @ip`

3. 修改主机名称`hostnamectl set-hostname yourhostname`

4. 安装nginx

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

5. 使用ssh key 登录

   1. 本地生成一对key

      `ssh-keygen -t rsa`

   2. 把公钥上传到服务器

      `scp ~/.ssh/kamatera_react_rsa.pub root@xxx:/root/.ssh/`

   3. 在服务器上

      ```bash
      mv .ssh/id_rsa.pub .ssh/authorized_keys
      chmod 700 .ssh
      chmod 600 .ssh/authorized_keys
      // vim /etc/ssh/sshd_config 中的  #PubkeyAuthentication yes   //这个默认就是开启的	
      service sshd restart
      
      //PasswordAuthentication no  关闭密码登录
      ```

   4. 使用key登录`ssh root@xxx.xxx.xxx.xxx -p aaa -i ~/.ssh/id_rsa`

开启bbr

```bash
wget --no-check-certificate -O /opt/bbr.sh https://github.com/teddysun/across/raw/master/bbr.sh
chmod 755 /opt/bbr.sh
/opt/bbr.sh

//验证
lsmod | grep bbr
```

