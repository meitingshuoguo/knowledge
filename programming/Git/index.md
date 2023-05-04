# 配置

## [多个 SSH-Key](https://gitee.com/help/labels/19)

**背景**

当有多个 git 账号时，比如：

- 一个 gitee，用于公司内部的工作开发；
- 一个 github，用于自己进行一些开发活动；

如果只有一个账号，那就只配置对应的就好了。

**解决方法**

1. 生成一个公司用的 SSH-Key

   `ssh-keygen -t ed25519 -C 'xxxxx@company.com' -f ~/.ssh/gitee_id`

2. 生成一个 github 用的 SSH-Key

   `ssh-keygen -t ed25519 -C 'xxxxx@qq.com' -f ~/.ssh/github_id`

3. 在 ~/.ssh 目录下新建一个 config 文件，添加如下内容（其中 Host 和 HostName 填写 git 服务器的域名，IdentityFile 指定私钥的路径）

   ```
    # gitee
    Host gitee.com
    HostName gitee.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/gitee_id

    # github
    Host github.com
    HostName github.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/github_id
   ```

4. 用 ssh 命令分别测试

   ```bash
   ssh -T git@gitee.com
   ssh -T git@github.com
   ```

   这里以 github 为例，成功的话会返回以下内容

   ```bash
   Connection to github.com port 22 [tcp/ssh] succeeded!
   Hi xxx! You've successfully authenticated, but GitHub does not provide shell access.
   ```

## 个人信息

首次使用 git 时应该检查其配置文件。

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

## 记住当前用户的登录密码

如果你配了 ssh，但是每次拉取或者推送时还需要输入系统当前用户的登录密码时，可以给` ~/.ssh/config` 文件加入一行配置 ` UseKeychain yes`

## git配置中的CRLF、LF、CR

基本

- CRLF: Carriage-Return Line-Feed的缩写，意思是回车换行，即\r\n;
- LF: Line-Feed的缩写,意思是换行，即\n;
- CR: Carriage-Return的缩写，回车，即\r;

进阶

当我们敲击回车键(Enter)时，操作系统会插入不可见的字符表示换行，不同的操作系统插入不同

- Windows: 插入\r\n,回车换行；
- Linux\Unix: 插入\n,换行；
- MacOS: 插入\r，回车；

Git

1. AutoCRLF

- 提交时转换为LF，检出时转换为CRLF
  `git config --global core.autocrlf true`
- 提交时转换为LF，检出时不转换
  `git config --global core.autocrlf input`
- 提交检出均不转换
  `git config --global core.autocrlf false`

2.SafeCRLF

- 拒绝提交包含混合换行符的文件
  `git config --global core.safecrlf true`
- 允许提交包含混合换行符的文件
  `git config --global core.safecrlf false`
- 提交包含混合换行符的文件时给出警告
  `git config --global core.safecrlf warn`

# git tag

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



# 文件名大小写问题

 在 Windows 和 macOS 系统中默认是忽略文件名大小写的。（可以测试一下，新建两个大小写不一样的文件，看看它们能否同时存在）

我在 macOS 下用系统默认的 git 拉下来的仓库中 git 的配置和本地 init 一个新的 git 仓库的配置中，core.ignorecase 的值默认是 true。但是 oh-my-zsh 中却显示 false 是默认值。（不晓得为什么）

所以在你有时候改动了一些文件名的大小写后默认情况下 git 是不会识别到的。有三种解决办法:

- （不推荐）设置 core.ignorecase 为 false，使其不忽略这种改动。(这种办法有问题。当设置为 false 后，重命名某个（已跟踪）文件后，git 状态显示的竟然是当前文件未跟踪。所以之前那个文件去哪儿了？[这个配置默认值就是 false，但是在 init 或者 clone 的时候会自动探测当前系统并将值设置为 true（如果合适的话](https://git-scm.com/docs/git-config)）)
- （推荐）使用命令 git mv [currentName] [newName] 进行改动。此时也不会忽略。（git 状态会明确显示这是一个重命名操作）
- （不推荐）将要重命名的文件删除后提交，再新增新文件提交。
- **可以新建一个分支，然后把之前分支的东西手动更新过来**

假如出现问题。

1. 首先新建一个目录，将该项目拉下来，如果提示有文件路径冲突，这时候一般是远程仓库已经跟踪了多个文件名大小写不一样的文件。但 clone 到本地时，因为本地系统文件大小写不敏感，所以无法让这些同名不同大小写的文件共存。

   1. 此时在原来的目录下执行 ` git rm --cached [fileName]` 如果执行结果为 ` rm [fileName]` 表示执行成功。
   2. `git commit git push ` 将其改动提交到远程仓库。此时可以再按照最开始那一步验证一下远程仓库此时是否已经删除了刚才提交的文件
   3. 如果是，此时远程仓库已经没有同名不同大小写的问题
   4. 此时如果本地当前分支更新后还是有问题，需要将有问题的文件所在的文件夹在本地删除，因为可能本地还缓存了被删除的文件。删除后重新更新该文件夹与远程分支同步。该分支一般就正常了。
   5. 如果此时还存在其他分支，并且分支之间切换出现可能与文件名大小写有关的问题，最好是删除所有本地分支后重新新建分支。一般会解决问题。
   6. 因为此时只有刚才提交了删除文件的分支才是正常的。在其他未进行该操作的分支可能还有同名但不同大小写的文件存在。（因为系统无法同时显示这些文件，所以也只能猜了，在 Linux 下可能会比较直观的看到）。所以从正常的分支新建的分支才会正常。

   另一种解决方案

   ```bash
   git rm --cached <filename>
   
   mv <old_filename> <new_filename>
   
   git add <new_filename>
   
   git commit -m 'rename <new_filename>' <new_filename>
   
   ```



# git stash

刚才说过，git stash 可以形成 list 集合。通过 git stash list 可以看到 list 下的 suoy

使用 git stash apply @{x} ，可以将编号 x 的缓存释放出来，但是该缓存还存在于 list 中

而 git stash apply，会将当前分支的最后一次缓存的内容释放出来，但是刚才的记录还存在 list 中

而 git stash pop，也会将当前分支的最后一次缓存的内容释放出来，但是刚才的记录不存在 list 中
————————————————



# 关联远程仓库并将本地仓库推送到 GitHub 上

```bash
git init
git add README.md
git add .
git commit -m "message"

git remote add origin git@github.com:meitingshuoguo/toy-react.git
git push -u origin master

# 由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。
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

# 修改提交的描述
git commit --amend

# 修改提交的内容
git add <filename>
git commit --amend
# 假如只是修改笔误或者忘记暂存某一个文件，不需要修改提交信息，则可以省略编辑器环节
git commit --amend --no-edit
```

## 删除commit

1. 删除最后一次提交

   ```bash
   git revert HEAD
   git push origin master
   ```

2. 删除历史某次提交

   ```bash
   git rebase -i "commit id"
   git push origin master -f
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





# git reflog 和 git log 的区别

- git reflog

  可以查看所有分支的所有操作记录（包括已经被删除的 commit 记录和 reset 的操作）

- git log

  则不能察看已经删除了的 commit 记录



# .gitignore

1. 只能忽略尚未用 git 管理的文件的修改

   `.gitignore` 和` .git/info/exclude`

   前者会被提交。后者只在本地。对已经纳入 git 管理的文件是不起作用的

2. 如果要忽略已被 git 管理的文件有两种方式

   1. 只忽略本地的文件

       1. `git update-index --skip-worktree []`
       
          想在本地修改由 Git 管理的文件（或自动更新），但又不想让 Git 管理该更改时，请使用该命令。
       
          因为该命令是为了防止 Git 管理本地更改，所以在大多数情况下，我们将使用该命令。
       
          确认 `git ls-files -v | grep ^S`
       
          撤销 `git update-index --no-skip-worktree []`
       
          2. `git update-index --assume-unchanged []`
       
                简单来说，当忽略不需要在本地更改(或者不应该更改)的文件时，使用它。
       
                确认 `git ls-files -v | grep ^h`
       
                撤销 `git update-index --no-assume-unchanged path/to/file`

   2. 需要更新到远程仓库的

       ```bash
       git rm -r --cached .
       git add .
       git commit -m "update .gitignore"
       ```

       


一些特殊场景

1. 模板文件忽略

    对于充当模版的文件，在文件名上加以区分然后用 Git 记住。比如说实际的配置文件应该叫 database.conf，在写好模版之后可以更名为 database.conf.example。Git 记录 database.conf.example 但是忽略 database.conf。

2. 每一个人克隆下来之后，复制一份 database.conf.example 为 database.conf 然后修改后者以符合本地的要求。由于后者是在.gitignore 里的，所以不会被记录，也完全不需要 update-index。这样一来即避免了副作用，又不会产生冲突问题，



# 找回删除的git stash的记录

```bash
// 找到对应的stash的id	
git log --graph --decorate --pretty=oneline --abbrev-commit --all $(git fsck --no-reflogs | grep commit | cut -d' ' -f3)
// 进行处理（下面只是其中一种方式）
git stash apply 95ccbd927ad4cd413ee2a28014c81454f4ede82c
```



# git rebase

**what:** 

提取我们在A分支上的改动，然后应用在B分支的代码上

**how:**

在本地分支中使用 rebase 来合并主分支的改动，是为了让你的本地提交记录清晰可读。（当然， rebase 不只用来合并 master 的改动，还可以在协同开发时 rebase 队友的改动。）

在主分支中使用 merge 来把 feature 分支的改动合并进来，是为了保留分支信息。

**why:**

如果提交存在于你的仓库之外，而别人可能基于这些提交进行开发，那么不要执行变基。

如果你遵循这条金科玉律，就不会出差错。 否则，人民群众会仇恨你，你的朋友和家人也会嘲笑你，唾弃你。

变基操作的实质是丢弃一些现有的提交，然后相应地新建一些内容一样但实际上不同的提交。 如果你已经将提交推送至某个仓库，而其他人也已经从该仓库拉取提交并进行了后续工作，此时，如果你用 `git rebase` 命令重新整理了提交并再次推送，你的同伴因此将不得不再次将他们手头的工作与你的提交进行整合，如果接下来你还要拉取并整合他们修改过的提交，事情就会变得一团糟。

[参考1](https://blog.csdn.net/weixin_42310154/article/details/119004977)

[参考2](在开发过程中使用 git rebase 还是 git merge，优缺点分别是什么？ - 一个小号的回答 - 知乎 https://www.zhihu.com/question/36509119/answer/1990894567)

[参考3](https://zhuanlan.zhihu.com/p/271677627)

# git branch

```bash
# 显示所有分支
git branch -a
# 删除本地分支（-D 强制删除）
git branch -d|-D  local_branch_name
# 删除远程分支
git push remote_name -d remote_branch_name
# 修改分支名称
git branch -m oldName newName
```


