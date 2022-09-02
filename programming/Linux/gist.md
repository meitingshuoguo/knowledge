# 端口

[Vultr的端口号问题](https://www.vediotalk.com/archives/520)

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

