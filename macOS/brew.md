| formula (e) | 安装包的描述文件，formulae 为复数                                                              |
| ----------- | ---------------------------------------------------------------------------------------------- |
| cellar      | 安装好后所在的目录                                                                             |
| keg         | 具体某个包所在的目录，keg 是 cellar 的子目录                                                   |
| bottle      | 预先编译好的包，不需要现场下载编译源码，速度会快很多；官方库中的包大多都是通过 bottle 方式安装 |
| tap         | 下载源，可以类比于 Linux 下的包管理器 repository                                               |
| cask        | 安装 macOS native 应用的扩展，你也可以理解为有图形化界面的应用。                               |
| bundle      | 描述 Homebrew 依赖的扩展                                                                       |

其中，最关键的是 tap 、cask，我们在后续会经常用到。

1 xcode-select —install

2 执行脚本安装 brew

3 brew doctor 检测有无问题，根据提示进行操作，直到返回“your system is ready to brew”

```bash
brew install []
brew uninstall []
brew search []
```

## 常用 tap

在使用 homebrew 时，我们一般会添加几个常用的 tap，来确保我们有足够的软件来安装。

### 1. Caskroom

Caskroom 是 Homebrew 下一个非常出名的 tap ，有了 caskroom，我们就可以安装一些有图形化界面的软件了，比如 VSCode、Typora 等软件。

使用起来也非常简单，最新版 Homebrew 中，你可以直接使用 `brew cask install [软件名]` 来安装特定的软件，homebrew 会自动安装 Caskroom。

### 2. homebrew-cask-fonts

程序员难免要安装一些代码字体，这样才能更好的写代码，Homebrew 也提供了方便我们安装字体的 tap。

在使用时，你需要先添加对应的 tap ，然后执行安装即可了，比如我们要安装 source code pro ，只需要执行如下命令。

```bash
brew tap homebrew/cask-fonts
brew cask install font-source-code-pro
```

## 常用 tap

在使用 homebrew 时，我们一般会添加几个常用的 tap，来确保我们有足够的软件来安装。

### 1. Caskroom

Caskroom 是 Homebrew 下一个非常出名的 tap ，有了 caskroom，我们就可以安装一些有图形化界面的软件了，比如 VSCode、Typora 等软件。

使用起来也非常简单，最新版 Homebrew 中，你可以直接使用 `brew cask install [软件名]` 来安装特定的软件，homebrew 会自动安装 Caskroom。

### 2. homebrew-cask-fonts

程序员难免要安装一些代码字体，这样才能更好的写代码，Homebrew 也提供了方便我们安装字体的 tap。

在使用时，你需要先添加对应的 tap ，然后执行安装即可了，比如我们要安装 source code pro ，只需要执行如下命令。

```bash
brew tap homebrew/cask-fonts
brew cask install font-source-code-pro
```

## 辅助软件

除了命令行，还有两款软件可以帮助我们更好的使用 Homebrew ，他们分别是 Cakebrew 和 launchrocket。

### Cakebrew

Cakebrew 是 Homebrew 的 GUI 管理器，在 Cakebrew 中，你可以看到当前所有已经安装的软件，并可以在 Caskbrew 中对其他软件执行升级等操作。

你只需要执行 `brew cask install cakebrew` 就可以完成 Cakebrew 的安装。

安装完成后，在 LaunchPad 中打开即可。

### launchrocket

launchrocket 可以用于管理 Homebrew 安装的服务，在使用时，你需要先添加对应的 tap，然后再安装软件。

```
brew tap jimbojsb/launchrocket
brew cask install launchrocket
```

安装完成后，在 LaunchPad 中打开即可。
