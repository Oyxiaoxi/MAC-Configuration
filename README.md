# 记一次重大失误 - MAC 重装系统

## System Preferences
在任何的操作系统中，首先你需要做一件事就是**更新系统**，点击窗口左上角的 ** > 关于本机 > 软件更新**。

## Xcode 
在用命令行安装所有软件或服务之前，一定要先去 **Apple Store** , 安装 **XCode**。

## Homebrew
包管理工具可以让你安装和更新程序变得更方便，目前在 OS X 系统中最受欢迎的包管理工具是 **Homebrew**。 [Homebrew](https://brew.sh/)

> ### install
> * 在安装 **Homebrew** 之前，需要将 **Xcode Command Line Tools** 安装完成，这样你就可以使用基于 Xcode Command Line Tools 编译的 **Homebrew**。
>   > homebrew 安装较慢，也有可能会出现问题，需要耐心

```bash
# install
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
# test
which brew && brew doctor
```

## brew install iterm2
```bash
# install iterm2
brew cask install iterm2

# zsh --version
brew install zsh zsh-completions

# install oh-my-zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

# zsh theme
sudo vim ~/.zshrc

# 修改 ZSH_THEME的值 这里改为cloud主题；
ZSH_THEME="cloud"

# :wq 保存退出后
source ~/.zshrc

# git zsh-autosuggestions
sudo git clone https://github.com/zsh-users/zsh-autosuggestions ~/.zsh/zsh-autosuggestions
source ~/.zsh/zsh-autosuggestions/zsh-autosuggestions.zsh

# downolad Dracula theme for iterm2
cd download && git clone https://github.com/dracula/iterm.git

1、iTerm2 > Preferences > Profiles > Colors Tab
2、Open the Color Presets... drop-down in the bottom right corner
3、Select Import... from the list
4、Select the Dracula.itermcolors file
5、Select the Dracula from Color Presets...

# Homebrew 禁止每次安装应用前更新
export HOMEBREW_NO_AUTO_UPDATE=true
```

### Homebrew Usage
```bash
# 更新所有，清除，通知完成任务
brew update && brew upgrade && brew cleanup ; say mission complete
brew update && brew upgrade brew-cask && brew cleanup ; say mission complete

# 查看哪些包能更新
brew outdated

# 更新指定包
brew upgrade <package>

# 清理所有包
brew cleanup

# 清理旧版本包
brew cleanup -n

# 锁定不想更新的包
brew pin <package> && brew unpin <package>
```

### Homebrew Cask
搜索 如果你想查看 cask 上是否存在你需要的 app，可以到 [Homebrew Cask](caskroom.io) 进行搜索

|package|summary|
|:-----:|:-----:|
|alfred|Alfred is an award-winning app for Mac OS X which boosts your efficiency with hotkeys, keywords, text expansion and more. Search your Mac and the web, and be more productive with custom actions to control your Mac.|

