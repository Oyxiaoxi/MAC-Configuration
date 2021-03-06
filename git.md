### 1. Git 常用命令

| Command                                                      | Note                                 |
| :----------------------------------------------------------- | :----------------------------------- |
| git remote add origin git@github.com:<YourName>@qq.com/dofiler.git | 配置远程git版本库                    |
| git pull origin master                                       | 下载代码及快速合并                   |
| git push origin master                                       | 上传代码及快速合并                   |
| git fetch origin                                             | 从远程库获取代码                     |
|                                                              |                                      |
| git branch                                                   | 显示所有分支                         |
| git checkout master                                          | 切换到master分支                     |
| git checkout -b dev                                          | 创建并切换到dev分支                  |
| git commit -m "first version"                                | 提交                                 |
|                                                              |                                      |
| git status                                                   | 查看状态                             |
| git log                                                      | 查看提交历史                         |
|                                                              |                                      |
| git config --global core.editor vim                          | 设置默认编辑器为vim（git默认用nano） |
| git config core.ignorecase false                             | 设置大小写敏感                       |
| git config --global user.name "YOUR NAME"                    | 设置用户名                           |
| git config --global user.email "YOUR EMAIL ADDRESS"          | 设置邮箱                             |



### 2. Git 别名 alias

| Command                                                      | Note                 |
| :----------------------------------------------------------- | :------------------- |
| git config --global alias.br="branch"                        | 创建/查看本地分支    |
| git config --global alias.co="checkout"                      | 切换分支             |
| git config --global alias.cb="checkout -b"                   | 创建并切换到新分支   |
| git config --global alias.cm="commit -m"                     | 提交                 |
| git config --global alias.st="status"                        | 查看状态             |
| git config --global alias.pullm="pull origin master"         | 拉取分支             |
| git config --global alias.pushm="push origin master"         | 提交分支             |
| git config --global alias.log="git log --oneline --graph --decorate --color=always" | 单行、分颜色显示记录 |



### 3. Git 创建版本库

| Command         | Note             |
| :-------------- | :--------------- |
| git clone <url> | 克隆远程版本库   |
| git init        | 初始化本地版本库 |



### 4. Git 修改和提交 

| Command                        | Note                 |
| :----------------------------- | :------------------- |
| git status                     | 查看状态             |
| git diff                       | 查看变更内容         |
| git add .                      | 跟踪所有改动过的文件 |
| git add <file>                 | 跟踪指定的文件       |
| git mv <old> <new>             | 文件改名             |
| git rm <file>                  | 删除文件             |
| git rm --cached <file>         | 停止跟踪文件但不删除 |
| git commit -m “commit message” | 提交所有更新过的文件 |
| git commit --amend             | 修改最后一次提交     |



### 5. Git 查看历史

| Command           | Note                             |
| :---------------- | :------------------------------- |
| git log           | 查看提交历史                     |
| git log -p <file> | 查看指定文件的提交历史           |
| git blame <file>  | 以列表方式查看指定文件的提交历史 |



### 6. Git 撤销

| Command                    | Note                                   |
| :------------------------- | :------------------------------------- |
| git reset --hard HEAD      | 撤消工作目录中所有未提交文件的修改内容 |
| git reset --hard <version> | 撤销到某个特定版本                     |
| git checkout HEAD <file>   | 撤消指定的未提交文件的修改内容         |
| git checkout -- <file>     | 同上一个命令                           |
| git revert <commit>        | 撤消指定的提交分支与标签               |



### 7. Git 分支与标签

| Command                         | Note                           |
| :------------------------------ | :----------------------------- |
| git branch                      | 显示所有本地分支               |
| git checkout <branch/tag>       | 切换到指定分支或标签           |
| git branch <new-branch>         | 创建新分支                     |
| git branch -d <branch>          | 删除本地分支                   |
| git tag                         | 列出所有本地标签               |
| git tag <tagname>               | 基于最新提交创建标签           |
| git tag -a "v1.0" -m "一些说明" | -a指定标签名称，-m指定标签说明 |
| git tag -d <tagname>            | 删除标签                       |
|                                 |                                |
| git checkout dev                | 合并特定的commit到dev分支上    |
| git cherry-pick 62ecb3          |                                |



### 8. Git 合并与衍合

| Command                | Note                                           |
| :--------------------- | :--------------------------------------------- |
| git merge <branch>     | 合并指定分支到当前分支                         |
| git merge --abort      | 取消当前合并，重建合并前状态                   |
| git merge dev -Xtheirs | 以合并dev分支到当前分支，有冲突则以dev分支为准 |
| git rebase <branch>    | 衍合指定分支到当前分支                         |



### 9 . Git 远程操作

| Command                              | Note                   |
| :----------------------------------- | :--------------------- |
| git remote -v                        | 查看远程版本库信息     |
| git remote show <remote>             | 查看指定远程版本库信息 |
| git remote add <remote> <url>        | 添加远程版本库         |
| git remote remove <remote>           | 删除指定的远程版本库   |
| git fetch <remote>                   | 从远程库获取代码       |
| git pull <remote> <branch>           | 下载代码及快速合并     |
| git push <remote> <branch>           | 上传代码及快速合并     |
| git push <remote> :<branch/tag-name> | 删除远程分支或标签     |
| git push --tags                      | 上传所有标签           |



### 10. Git 打包

| Command                                              | Note                                               |
| :--------------------------------------------------- | :------------------------------------------------- |
| git archive --format=zip --output ../file.zip master | 将master分支打包成file.zip文件，保存在上一级目录   |
| git archive --format=zip --output ../v1.2.zip v1.2   | 打包v1.2标签的文件，保存在上一级目录v1.2.zip文件中 |
| git archive --format=zip v1.2 > ../v1.2.zip          | 作用同上一条命令                                   |



### 11. 远程与本地合并

| Command                          | Note             |
| :------------------------------- | :--------------- |
| git init                         | 初始化本地代码仓 |
| git add .                        | 添加本地代码     |
| git commit -m "add local source" | 提交本地代码     |
| git pull origin master           | 下载远程代码     |
| git merge master                 | 合并master分支   |
| git push -u origin master        | 上传代码         |



### 12. 规范

| Type     | Note                                                         |
| -------- | ------------------------------------------------------------ |
| init     | 初始提交                                                     |
| feat     | 新功能或功能变更相关                                         |
| fix      | 修复 Bug 相关                                                |
| ui       | 更新 UI                                                      |
| docs     | 改动文档，注释相关                                           |
| style    | 修改了代码格式化相关，如删除空格、改变缩进、单双引号切换、增删分号等，并不会影响代码逻辑 |
| refactor | 重构代码，代码结构的调整相关                                 |
| release  | 代码发布                                                     |
| deploy   | 部署                                                         |
| perf     | 性能改动，性能、页面等优化相关                               |
| test     | 增加或更改测试用例，单元测试                                 |
| build    | 影响编译的更改相关                                           |
| ci       | 持续集成方面的更改                                           |
| chore    | 更改配置文件                                                 |
| revert   | 回滚版本相关                                                 |
| add      | 添加依赖                                                     |
| mins     | 版本回退                                                     |
| del      | 删除代码/文件                                                |

