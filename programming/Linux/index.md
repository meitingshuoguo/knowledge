拥有一个 CentOS 7 之后

## 登录（ssh 口令登录）

```bash
# root是服务器用户名 ip是服务器地址
ssh root@ip
```

## 更新

```bash
# 升级所有包同时也升级软件和系统内核
yum -y update

# 只升级所有包，不升级软件和系统内核
yum -y upgrade
```

## 安装 vim

```bash
# 安装
yum install vim -y

# 设置别名，替代内置的vi
echo "alias vi=vim" >> /etc/profile

# 更新别名配置
source /etc/profile

# 添加vim配置
vi ~/.vimrc
# 输入
syntax on
colorscheme pablo
```

## 安装[oh-my-zsh](https://github.com/ohmyzsh/ohmyzsh)

1. [安装 zsh](https://github.com/ohmyzsh/ohmyzsh/wiki/Installing-ZSH)

    ```bash
    sudo yum -y install zsh
    ```
2. [安装 git](https://computingforgeeks.com/install-git-2-on-centos-7/)

    ```bash
    sudo yum -y install https://packages.endpointdev.com/rhel/7/os/x86_64/endpoint-repo.x86_64.rpm
    
    sudo yum install git
    ```
3. 安装 oh-my-zsh
<br>
    `sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"`
4. 插件安装
    1. 需要安装的

        ```bash
        # zsh-autosuggestions
        git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
        # zsh-syntax-highlighting
        git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
        ```
    2. 启用插件

        ```bash
        # 编辑zsh的配置文件
        vi ~/.zshrc
        plugins=(git z zsh-autosuggestions zsh-syntax-highlighting)
        
        # 更新配置文件
        source .zshrc
        ```

## 使用 ssh 密钥登录

### 本地生成密钥对

`ssh-keygen -t ed25519`

把公钥内容添加到服务器下的 `.ssh/authorized_keys`中

`ssh-copy-id -i ~/.ssh/key.pub root@ip`

### 测试 key 登录

**确保 22 端口是开启的**

**注意你的密钥文件名生成方式**

1. 如果你是一路回车生成的密钥文件
<br>
    `ssh root@xxx.xxx.xxx`
2. 如果你是生成的自定义的密钥文件，要注意 `IdentityFile`这个配置。在 config 里配别名登录的时候必须指定这个，因为默认情况下，ssh 只会找系统默认的几个文件名（`/etc/ssh/ssh_config`）

    ```
    
    ```

```bash
	#ssh root@192.168.182.55 -p 22
	ssh root@xxx.xxx.xxx -r 'key'
```

## 用别名登录

在本机的 `.ssh`文件夹下的 config 文件中新增

```bash
# your vps
Host [vpsName]
User [root]
HostName [IP Address]
IdentityFile ~/.ssh/[私钥名称]
```

安装一个查看端口的软件

```bash
# -y：安装过程中都选yes
yum install net-tools -y
```

安装的时候可能会报错 `Couldn't resolve host 'mirrorlist.centos.org`

```bash
vim /etc/resolv.conf
#加入以下内容
nameserver 8.8.8.8
nameserver 202.106.0.20

#重启网络服务
/etc/init.d/network restart
```

修改主机名称（不是必须的）

`hostnamectl set-hostname yourhostname`

## 端口

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