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
