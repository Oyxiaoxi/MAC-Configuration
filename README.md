# 记一次重大失误 - MAC 重装系统

## System Preferences
在任何的操作系统中，首先你需要做一件事就是**更新系统**，点击窗口左上角的 ** > 关于本机 > 软件更新**。

## Xcode 
在用命令行安装所有软件或服务之前，一定要先去 **Apple Store** , 安装 **XCode**。

## Homebrew
包管理工具可以让你安装和更新程序变得更方便，目前在 OS X 系统中最受欢迎的包管理工具是 **Homebrew**。 [Homebrew](https://brew.sh/)

> ### install
> * 在安装 **Homebrew** 之前，需要将 **Xcode Command Line Tools** 安装完成，这样你就可以使用基于 Xcode Command Line Tools 编译的 **Homebrew**。

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
>   > homebrew 安装较慢，也有可能会出现，需要耐心
