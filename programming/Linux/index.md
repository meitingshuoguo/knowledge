# 拥有一个 VPS 之后

1. 先用密码登录`ssh @ip`

2. 修改主机名称 `hostnamectl set-hostname yourhostname`

3. 安装 nginx

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

4. 使用 ssh key 登录

   1. 本地生成一对 key

      `ssh-keygen -t ed25519 -C "name1" -f myVps`

   2. 把公钥上传到服务器（可能要先在服务器上新建.ssh 文件夹）

      `scp ~/.ssh/myVps.pub root@xxx:/root/.ssh/`

   3. 在服务器上执行以下命令

      ```bash
      mv .ssh/myVps.pub .ssh/authorized_keys
      chmod 700 .ssh
      chmod 600 .ssh/authorized_keys

      service sshd restart

      //PasswordAuthentication no  关闭密码登录
      ```

   4. 测试 key 登录

      在本地的软件中（比如 mac 下的 Rolay TSX）中配置 ssh key 的登录方式。

      下面这个命令可以来测试是不是配置正确了

      `ssh root@xxx.xxx.xxx -p [port] -i ~/.ssh/myVps`

   5. 用别名登录

      在本机的`.ssh`文件夹下的 config 文件中新增

   ```bash
     # your vps
     Host [vpsName]
     User root
     HostName [IP Address]
     PreferredAuthentications publickey
     IdentityFile ~/.ssh/[私钥名称]

   ```

5. 开启 bbr

   ```bash
   wget --no-check-certificate -O /opt/bbr.sh https://github.com/teddysun/across/raw/master/bbr.sh
   chmod 755 /opt/bbr.sh
   /opt/bbr.sh

   //验证
   lsmod | grep bbr
   ```
