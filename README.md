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

```bash
brew cask instll < package >
```
|package|summary|
|:-----:|:-----:|
|alfred|搜索神器|
|iterm2|命令行工具|
|qq|鹅厂通讯工具|
|qqmusic|鹅厂音乐|
|wechat|鹅厂微信|
|suspicious-package|快速查看包安装内容|
|betterzip|压缩包快速查看工具|
|qlcolorcode|代码预览工具|
|visual-studio-code|微软轻量代码编辑器|
|cheatsheet|长按commod快捷键预览|
|qlimagesize|快速查看图片大小|
|quicklook-csv|excel快速查看|
|webpquicklook|文件快速预览|
|firefox|火狐浏览器|
|qlprettypatch|文件路径查看|
|quicklook-json|json快速预览|
|google-chrome|chrome|
|qlstephen|各类文件快速预览|
|sourcetree|git图形界面管理工具|
|archey|mac概览|

### Development

#### mysql
因为使用的数据库结构版本较低，故只能使用 <= 8.0 以上的数据库版本

```zsh
brew install mysql@5.7

# MySql 加入系统变量
export PATH="/usr/local/opt/mysql@5.7/bin:$PATH"

# 配置生效
source ~/.zshrc

# mysql.sock
cd /var && sudo mkdir mysql
sudo ln -s /tmp/mysql.sock /var/mysql/mysql.sock

# mysql root 密码设置
mysql_secure_installation 

# start & stop & restart 
mysql.server start 
mysql.server stop
mysql.server restart

# 锁定包
brew pin mysql@5.7
```

#### Composer
php 包管理工具
```zsh
brew install composer

# 加入系统变量
export PATH="/usr/bin/:$PATH:$HOME/.composer/vendor/bin/:$PATH"  # Composer
```

#### Flutter
Google’s mobile app SDK
```zsh
cd ~ && git clone -b beta https://github.com/flutter/flutter.git

# Flutter 环境变量
export PATH=$HOME/flutter/bin:$PATH

# Flutter CN  国内镜像
export PUB_HOSTED_URL=https://pub.flutter-io.cn
export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn

# 配置生效
source ~/.zshrc 

# 查看依赖项
flutter doctor

# 根据依赖项目安装 
brew install --HEAD libimobiledevice
brew install ideviceinstaller ios-deploy cocoapods
pod setup
```

#### Laravel
[百度网盘](https://pan.baidu.com/s/1slWENqH#list/path=%2F)

install VirtualBox, Vagrant

```bash
# Homestead Box 
[Homestead 虚拟机盒子](http://download.fsdhub.com/lc-homestead-6.1.0-2018061700.zip)

# 解压 lc-homestead ，切换其解压目录下
vagrant box add metadata.json  # Homestead Vagrant 导入

# 下载 Homestead
cd ~ && git clone https://git.coding.net/summerblue/homestead.git Homestead
cd ~/Homestead && git checkout v7.8.0

# 初始化
bash init.sh
```

##### 配置文件 
code ~/Homestead/Homestead.yaml

1. 虚拟机设置
ip: "192.168.10.10"
memory: 2048
cpus: 1
provider: virtualbox

2. SSH 秘钥登录配置
> authorize 选项是指派登录虚拟机授权连接的公钥文件，此文件填写的是主机上的公钥文件地址，虚拟机初始化时，此文件里的内容会被复制存储到虚拟机的 /home/vagrant/.ssh/authorized_keys文件中，从而实现 SSH 免密码登录。

authorize: ~/.ssh/id_rsa.pub

##### 此处我们将公钥和私钥一起同步到虚拟机中：
keys:
    - ~/.ssh/id_rsa
    - ~/.ssh/id_rsa.pub

##### 检查主机上是否已经生成过 SSH Key
ls -al ~/.ssh

##### 3. 共享文件夹配置
folders:
    - map: ~/Code
      to: /home/vagrant/Code
```bash
cd ~
mkdir Code
# Homestead 会把该文件夹下的项目自动映射到虚拟机的 /home/vagrant/Code 文件夹上。
```

##### 4. 站点配置
sites:
    - map: homestead.test
      to: /home/vagrant/Code/Laravel/public

```bash
code /etc/hosts
# hosts 最后一行加入
192.168.10.10  homestead.test
# 保存之后 
source /etc/hosts
```

##### 5.数据库配置
databases:
    - homestead
    - 每次增加数据库在这里

##### 6.自定义变量
variables:
    - key: APP_ENV
      value: local

##### Vagrant
|命令行|说明|
|:-----:|:-----:|
|vagrant init|初始化 vagrant|
|vagrant up|启动 vagrant|
|vagrant halt|关闭 vagrant|
|vagrant ssh|通过 SSH 登录 vagrant（需要先启动 vagrant）|
|vagrant provision|重新应用更改 vagrant 配置|
|vagrant destroy|删除 vagrant|
|vagrant reload |重新加载|

```bash
cd ~/Homestead && vagrant up
vagrant ssh
vagrant halt

# 重新加载
cd ~/Homestead && vagrant provision && vagrant reload 
```