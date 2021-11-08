# 端口

```bash
//安装netstat工具
yum install net-tools -y

//查看服务器所有被占用的端口
netstat -ant

//验证某个端口是否被占用
netstat -tunlp|grep 15692

//查看所有监听端口号
netstat -lntp
```

