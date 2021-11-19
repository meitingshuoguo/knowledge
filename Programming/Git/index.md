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



# 打标签

```bash
git tag                                       //查看所有标签
git tag -a v1.4 -m "my version 1.4"           //添加一个附注标签
git tag v1.4                                  //添加一个轻量标签
git tag -a v1.2 9fceb02 xxx										//为某一个或某几个提交添加标签
git push origin v1.5                          //将该标签推送到远端
git push origin --tags                        //将所有本地标签推送到远端
git tag -d v1.4-lw                            //删除本地标签
git push origin --delete <tagname>            //删除远程标签
git checkout -b version2 v2.0.0               //检出标签并新建分支，且切换到该分支
```



## 文件名大小写问题

​	在Windows和macOS系统中默认是忽略文件名大小写的。（可以测试一下，新建两个大小写不一样的文件，看看它们能否同时存在）

我在macOS下用系统默认的git拉下来的仓库中git的配置和本地init一个新的git仓库的配置中，core.ignorecase的值默认是true。但是oh-my-zsh中却显示false是默认值。（不晓得为什么）

所以在你有时候改动了一些文件名的大小写后默认情况下git是不会识别到的。有三种解决办法:

* （不推荐）设置 core.ignorecase 为false，使其不忽略这种改动。(这种办法有问题。当设置为false后，重命名某个（已跟踪）文件后，git状态显示的竟然是当前文件未跟踪。所以之前那个文件去哪儿了？[这个配置默认值就是false，但是在init或者clone的时候会自动探测当前系统并将值设置为true（如果合适的话](https://git-scm.com/docs/git-config)）)
* （推荐）使用命令 git mv [currentName] [newName] 进行改动。此时也不会忽略。（git状态会明确显示这是一个重命名操作）
* （不推荐）将要重命名的文件删除后提交，再新增新文件提交。
* **可以新建一个分支，然后把之前分支的东西手动更新过来**

假如出现问题。

1. 首先新建一个目录，将该项目拉下来，如果提示有文件路径冲突，这时候一般是远程仓库已经跟踪了多个文件名大小写不一样的文件。但clone到本地时，因为本地系统文件大小写不敏感，所以无法让这些同名不同大小写的文件共存。

   1. 此时在原来的目录下执行 ``` git rm --cached [fileName]``` 如果执行结果为   ```	rm [fileName]``` 表示执行成功。
   2. ```git commit   git push ``` 将其改动提交到远程仓库。此时可以再按照最开始那一步验证一下远程仓库此时是否已经删除了刚才提交的文件
   3. 如果是，此时远程仓库已经没有同名不同大小写的问题
   4. 此时如果本地当前分支更新后还是有问题，需要将有问题的文件所在的文件夹在本地删除，因为可能本地还缓存了被删除的文件。删除后重新更新该文件夹与远程分支同步。该分支一般就正常了。
   5. 如果此时还存在其他分支，并且分支之间切换出现可能与文件名大小写有关的问题，最好是删除所有本地分支后重新新建分支。一般会解决问题。
   6. 因为此时只有刚才提交了删除文件的分支才是正常的。在其他未进行该操作的分支可能还有同名但不同大小写的文件存在。（因为系统无法同时显示这些文件，所以也只能猜了，在Linux下可能会比较直观的看到）。所以从正常的分支新建的分支才会正常。

   

   另一种解决方案

   ```bash
   git rm --cached <filename>
   
   mv <old_filename> <new_filename>
   
   git add <new_filename>
   
   git commit -m 'rename <new_filename>' <new_filename>
   
   ```

   



## 记住当前用户的登录密码

如果你配了ssh，但是每次拉取或者推送时还需要输入系统当前用户的登录密码时，可以给``` ~/.ssh/config``` 文件加入一行配置  ``` UseKeychain yes``` 



## git stash

刚才说过，git stash 可以形成list 集合。通过git stash list 可以看到list下的suoy

使用git stash apply @{x} ，可以将编号x的缓存释放出来，但是该缓存还存在于list中

而 git stash apply，会将当前分支的最后一次缓存的内容释放出来，但是刚才的记录还存在list中

而 git stash pop，也会将当前分支的最后一次缓存的内容释放出来，但是刚才的记录不存在list中
————————————————
版权声明：本文为CSDN博主「Jacob-wj」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/wangjia55/java/article/details/8791227



# 关联远程仓库并将本地仓库推送到GitHub上

```bash
git init
git add README.md
git add .
git commit -m "message"

git remote add origin git@github.com:meitingshuoguo/toy-react.git
git push -u origin master
// 由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。
```

# git push

```bash
git push <远程主机名> <本地分支名>:<远程分支名>
git push -f origin localDev:dev   //强制推送本地分支到远程分支（会覆盖远程分支，慎用）
```



# git cherry-pick

代码冲突
如果操作过程中发生代码冲突，Cherry pick 会停下来，让用户决定如何继续操作。

（1）--continue

用户解决代码冲突后，第一步将修改的文件重新加入暂存区（git add .），第二步使用下面的命令，让 Cherry pick 过程继续执行。


$ git cherry-pick --continue
（2）--abort

发生代码冲突后，放弃合并，回到操作前的样子。

（3）--quit

发生代码冲突后，退出 Cherry pick，但是不回到操作前的样子。



# git commit

```bash
git reset --soft HEAD^   // 撤销本次commit（保留更改）
```



# 本地创建分支后推送到远程（即在本地创建一个远程分支）

```bash
 git checkout -b fs_saas_front_lx
 git push origin fs_saas_front_lx  			//默认使用本地分支名做远程分支名
 git push origin branch_dev:branch_dev  //可以指定远程分支名
```

```bash
git checkout -b [本地分支branch_x] origin/[远程分支名name]     //拉取远程分支到本地，并切换到该分支
```

