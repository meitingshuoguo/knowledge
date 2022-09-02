为GitHub添加一个SSH key的步骤

1. 本机执行 `ssh-keygen -t ed25519 -C "name1"` 
  
    name1是公钥内容最后的署名。在上述命令执行后还有一个地方可以输入name2，是生成的key的名字。

2. 将公钥的内容放入GitHub的SSH列表中
3. 在本机的`.ssh`文件夹下的config文件中新增
    ```bash
      # github
      Host github.com
      HostName github.com
      PreferredAuthentications publickey
      IdentityFile ~/.ssh/[私钥名称]
      
      #为git@协议配置socks5代理
      ProxyCommand nc -v -x 127.0.0.1:7890 %h %p
    ```
4. 执行`ssh -T git@github.com`如果返回成功字样，则说明配置成功