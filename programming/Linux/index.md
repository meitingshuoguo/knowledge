# 拥有一个 VPS 之后

1. 密码登录

   ```bash
   # root是服务器用户名 ip是服务器地址
   ssh root@ip
   ```

2. 更新

   ```bash
   # 升级所有包同时也升级软件和系统内核
   yum -y update

   # 只升级所有包，不升级软件和系统内核
   yum -y upgrade
   ```

3. oh-my-zsh

   ```bash
   sudo yum -y install zsh
   ```

4. 安装一个查看端口的软件

   ```bash
   # -y：安装过程中都选yes
   yum install net-tools -y
   ```

   安装的时候可能会报错`Couldn't resolve host 'mirrorlist.centos.org`

   ```bash
   vim /etc/resolv.conf
   #加入以下内容
   nameserver 8.8.8.8
   nameserver 202.106.0.20

   #重启网络服务
   /etc/init.d/network restart
   ```

5. 修改主机名称（不是必须的）

   `hostnamectl set-hostname yourhostname`

6. 使用 ssh key 登录

   1. 本地生成一对 key

      `ssh-keygen -t ed25519 -C "密钥文件内容最后的署名" -f "密钥文件名"`

   2. 把公钥内容添加到服务器下的`.ssh/authorized_keys`中（可能要先在服务器上新建.ssh 文件夹）

   3. 在服务器上执行以下命令

      ```bash
      #文件权限设置
      chmod 700 .ssh
      chmod 600 .ssh/authorized_keys

      #重启ssh服务
      service sshd restart

      #关闭密码登录
      cd /etc/ssh/sshd_config
      PasswordAuthentication no
      ```

   4. 测试 key 登录

      在本地的软件中（比如 mac 下的 Rolay TSX）中配置 ssh key 的登录方式。

      下面这个命令可以来测试是不是配置正确了

      **确保 22 端口是开启的**

      ```bash
      #ssh root@192.168.182.55 -p 22 -i '.\server_55'
      ssh root@xxx.xxx.xxx -p [port] -i ~/.ssh/myVps
      ```

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

# 端口

[Vultr 的端口号问题](https://www.vediotalk.com/archives/520)

```bash
# 列出所有端口
netstat -ntlp

# 查询80端口是否开启,返回yes/no
firewall-cmd --query-port=80/tcp --zone=public

# 添加80端口，执行后需要重启防火墙
firewall-cmd --zone=public --add-port=80/tcp --permanent

# 重启防火墙
systemctl restart firewalld

# 防火墙状态
systemctl status firewalld

# 打开防火墙
systemctl start firewalld

# 关闭防火墙
systemctl stop firewalld

```

```bash
# 安装netstat工具
yum install net-tools -y

# 查看服务器所有被占用的端口
netstat -ant

# 验证某个端口是否被占用
netstat -tunlp|grep 15692

# 查看所有监听端口号
netstat -lntp
```

```bash
# 查看端口是否被占用
lsof -i:8300

# 执行并退出，有时候需要再执行 exit;
nohup npm run start &

#查看进程或pid被占用的情况
ps -ef|grep [进程名｜pid]

# 在你买了新 mac 使用迁移助理前一定一定要记得删掉所有 node_modules
find . -name "node_modules" -type d -prune -exec rm -rf '{}' +

# 没想到迁移完成后 node_modules 还在不停的耗费 spotlight 资源……禁用 node_modules 的索引：
find . -type d -name "node_modules" -exec touch "{}/.metadata_never_index" \

```

5. 安装 nginx

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

6. 开启 bbr

   ```bash
   wget --no-check-certificate -O /opt/bbr.sh https://github.com/teddysun/across/raw/master/bbr.sh
   chmod 755 /opt/bbr.sh
   /opt/bbr.sh

   //验证
   lsmod | grep bbr
   ```
