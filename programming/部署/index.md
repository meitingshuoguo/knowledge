# 安装 Jenkins

> ### WAR 文件
>
> Jenkins 的 Web 应用程序 ARchive（WAR）文件版本可以安装在任何支持 Java 的操作系统或平台上。
>
> **要下载并运行 Jenkins 的 WAR 文件版本，请执行以下操作:**
>
> 1. 将[最新的稳定 Jenkins WAR 包](http://mirrors.jenkins.io/war-stable/latest/jenkins.war) 下载到您计算机上的相应目录。
> 2. 在下载的目录内打开一个终端/命令提示符窗口到。
> 3. 运行命令 java -jar jenkins.war
> 4. 浏览 http://localhost:8080 并等到*Unlock Jenkins*页面出现。
> 5. 继续使用[Post-installation setup wizard](https://www.jenkins.io/zh/doc/book/installing/#setup-wizard)后面步骤设置向导。
>
> 将[最新的稳定 Jenkins WAR 包](http://mirrors.jenkins.io/war-stable/latest/jenkins.war)下载到您计算机上的相应目录。
>
> **Notes:**
>
> - 不像在 Docker 中下载和运行有 Blue Ocean 的 Jenkins，这个过程不会自动安装 Blue Ocean 功能， 这将分别需要在 jenkins 上通过 [**Manage Jenkins**](https://www.jenkins.io/zh/doc/book/managing) > [**Manage Plugins**](https://www.jenkins.io/zh/doc/book/managing/plugins/)安装。 在[Getting started with Blue Ocean](https://www.jenkins.io/zh/doc/book/blueocean/getting-started/)有关于安装 Blue Ocean 的详细信息 。.
>
> - 您可以通过`--httpPort`在运行`java -jar jenkins.war`命令时指定选项来更改端口。例如，要通过端口 9090 访问 Jenkins，请使用以下命令运行 Jenkins： `java -jar jenkins.war --httpPort=9090`
>
>   来自[Jenkins 官网](https://www.jenkins.io/zh/doc/book/installing/#war%E6%96%87%E4%BB%B6)

上传本地文件到服务器 scp /path/filename username@servername:/path ;

例如`scp /Users/mac/Desktop/test.txt root@123.207.170.40:/root/`

> Jenkins initial setup is required. An admin user has been created and a password generated.
>
> Please use the following password to proceed to installation:
>
> afd4009aff1e436ca48c0fe4b9a01ff2 莱西
>
> 551cd6aba55b42129e375973aecb2e0d 宿州
>
> This may also be found at: /root/.jenkins/secrets/initialAdminPassword

# 安装钉钉机器人插件

1. 安装插件
2. 在钉钉群助手中新建一个机器人，获取 webhook 和加签 ID
3. 在 Jenkins 系统配置中设置钉钉机器人的名称，webhook 地址和加签 ID。保存。

# 安装 gitee 插件

1. 安装插件
2. [添加 gitee 链接配置](https://gitee.com/help/articles/4193#article-header5)

# 注意事项

- 以 nohup 启动 Jenkins 和前端项目
- 在 Jenkins 的项目配置中设置 git clone 时的目录
- 前端项目默认是以 8000 端口运行的，所以需要设置一下 nginx 的代理端口

```bash
scp /Users/hefkang/Desktop/jenkins.war root@39.100.45.66:/home/jenkins/
```

# 详细步骤

1. 下载.war 包，上传到服务器指定目录，然后`java -jar jenkins.war --httpPort=9090` 安装 Jenkins，记得保存下输出的密码
2. 服务器要放开该端口，然后 ip+端口去访问 Jenkins，等待，输入密码，安装推荐插件
3. 检查服务器有没有安装 git。
4. 安装 gitee 和 dingtalk 插件
5. 配置上述两个插件

   - gitee 令牌：07c3d71f30fa3533ec217c651a218308

   - 钉钉
     - webhook：https://oapi.dingtalk.com/robot/send?access_token=19e2d60dc1e238cdc29334a5e8c4ec1ce68a105ddbf03e3b3ab3a91022d5bb69
     - 加密：SEC90890edbd8247ad3ff2795a721003714c7e2e2c27584ef0164321f435657c426

6. 新建一个项目，配置该项目

7. 连接服务器，找到 home 目录，新建 jenkins 目录

8. 将本地下载的 jenkins.war 复制到上面的目录

   ` scp /Users/hefkang/Desktop/jenkins.war root@39.100.45.66:/home/jenkins/`

9. 查看 git 是否安装

   ```bash
   whereis git
   // 如果没有输出路径，就是没安装
   cat /proc/version  查看内核版本信息

   yum info git
   yum install git //版本可能比较老
   ```

10. 进入 jenkins 目录，安装。需要先确定好端口号。（要开端口号，防火墙也要开这个端口）

    ```bash
    //第一次是安装，必须运行下面的命令
    java -jar jenkins.war --httpPort=9090

    // 安装成功后如果服务停了，则用下面这个命令
    nohup java -jar jenkins.war --httpPort=9090 &
    ```

    获取到密码后用服务器 IP+端口号去访问。

11. 登录后安装推荐的插件，安装 gitee 和 dingtalk 插件。

    访问路径后加 restart。重启 Jenkins

12. 配置上述两个插件

    链接名：Gitee

    ​ Gitee 域名 URL：https://gitee.com

    ​ 证书令牌：x x x (获取地址 https://gitee.com/profile/personal_access_tokens)

    - 钉钉
      - webhook：https://oapi.dingtalk.com/robot/send?access_token=19e2d60dc1e238cdc29334a5e8c4ec1ce68a105ddbf03e3b3ab3a91022d5bb69
      - 加密：SEC90890edbd8247ad3ff2795a721003714c7e2e2c27584ef0164321f435657c426

13. 新建一个自由项目

    - 钉钉配置：勾选钉钉构建结果机器人。高级中：自定义内容添加：- 具体分支:东西湖分支

    - 源码管理：选 git。Repository URL 里添加项目 gitee 地址。Credentials 里添加用户名密码的认证。

      ​ 指定分支为：\*/具体分支名。Additional Behaviours 中选择检出到子目录，填写/home/web/frontend/

    - 构建触发器：选 gitee webhook 触发。允许触发构建的分支里选择根据分支名过滤，包括中填写：具体分支名。

      ​ 然后去 gitee 的项目管理里配置 webhook。

    - 构建：选择执行 shell，填写 npm i

    - 服务器执行 ` nohup npm run start &` ` exit;`

​
