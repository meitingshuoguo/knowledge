# [Git配置多个SSH-Key](https://gitee.com/help/labels/19)



**背景**

当有多个git账号时，比如：

  - 一个gitee，用于公司内部的工作开发；
  -  一个github，用于自己进行一些开发活动；

如果只有一个账号，那就只配置对应的就好了。



**解决方法**

1. 生成一个公司用的SSH-Key

   `ssh-keygen -t rsa -C 'xxxxx@company.com' -f ~/.ssh/gitee_id_rsa`

2. 生成一个github用的SSH-Key

   `ssh-keygen -t rsa -C 'xxxxx@qq.com' -f ~/.ssh/github_id_rsa`

3. 在 ~/.ssh     目录下新建一个config文件，添加如下内容（其中Host和HostName填写git服务器的域名，IdentityFile指定私钥的路径）

   ```
    # gitee
    Host gitee.com
    HostName gitee.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/gitee_id_rsa
    # github
    Host github.com
    HostName github.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/github_id_rsa
   ```

4. 用ssh命令分别测试

   ```bash
   ssh -T git@gitee.com
   ssh -T git@github.com
   ```

   这里以github为例，成功的话会返回以下内容

   ```bash
   Connection to github.com port 22 [tcp/ssh] succeeded!
   Hi xxx! You've successfully authenticated, but GitHub does not provide shell access.
   ```



# 配置个人信息

首次使用git时应该检查其配置文件。

Git 自带一个 `git config` 的工具来帮助设置控制 Git 外观和行为的配置变量。 这些变量存储在三个不同的位置：

1. `/etc/gitconfig` 文件: 包含系统上每一个用户及他们仓库的通用配置。 如果在执行 `git config` 时带上 `--system` 选项，那么它就会读写该文件中的配置变量。 （由于它是系统配置文件，因此你需要管理员或超级用户权限来修改它。）
2. `~/.gitconfig` 或 `~/.config/git/config` 文件：只针对当前用户。 你可以传递 `--global` 选项让 Git 读写此文件，这会对你系统上 **所有** 的仓库生效。
3. 当前使用仓库的 Git 目录中的 `config` 文件（即 `.git/config`）：针对该仓库。 你可以传递 `--local` 选项让 Git 强制读写此文件，虽然默认情况下用的就是它。 （当然，你需要进入某个 Git 仓库中才能让该选项生效。）

每一个级别会覆盖上一级别的配置，所以 `.git/config` 的配置变量会覆盖 `/etc/gitconfig` 中的配置变量。

相关命令：

```bash
//查看所有配置
git config --list
//查看配置所在文件
git config --list --show-origin
//查看某个配置
git config [configName]
//查看某个配置的原始值
git config --show-origin rerere.autoUpdate
//添加配置
git config -[-local|-global|-system] [configName] [configValue]
//删除配置
git config -[-local|-global|-system] --unset [configName]
```

