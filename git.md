# git 版本控制学习
基于 [廖雪峰的git教程](https://liaoxuefeng.com/books/git/introduction/index.html) 进行基本的版本控制学习。

## 目录


## 本地创建
### 创建git管理的仓库
```powershell
git init
```
该命令将使本目录为根目录，并受到git的管理。

### 将文件暂存到仓库
```powershell
git add <文件>  # 将<文件>进行暂存
git add *       # 将根目录内所有文件暂存
```
该命令会将文件暂存，但不是正式提交。可以分别、多次的暂存修改文件，而提交则将所有修改一次提交为一个版本。

### 提交修改文件到仓库
```powershell
git commit -m "<提交说明>"
```
该命令将**暂存文件**进行提交得到一个新的版本，附带信息：提交人和<提交说明>。
对于没有暂存的修改文件而言，提交不会把修改文件提交到git的版本中。

#### 规范的提交说明type
截取自 ["Git｜commit 规范" based 球球的前端奶茶屋](https://www.bilibili.com/video/BV1kM4m1X7P5/)
- feat:增加新功能
- fix:修复bug
- docs:修改文档内容
- style:不影响代码内容的改动（去掉空格、改变缩进、增删分号等）
- build:构建工具的或者外部依赖的改动
- refactor:代码重构时使用
- test:添加测试或者修改现有测试
- perf:提高性能的改动

### 查看目前修改情况
```powershell
git status
# (use "git add <file>..." to update what will be committed) 如果没有这句提示证明"modified:"的文件已经被暂存
# (use "git restore <file>..." to discard changes in working directory)
# modified:   <文件>
```
该命令将查看目前修改未提交的文件，包括暂存、未暂存、未被添加和删除的文件。

### 查看修改内容
```powershell
git diff
```
该命令将显示出修改文件所修改的内容。

### 查看提交日志
```powershell
git log
git log --pretty=oneline  # 该命令将只输出版本号和提交说明，以节省显示空间
```
该命令将显示出commit的日志，包含版本号、提交人及提交说明。
显示"HEAD"为当前版本。

### 回退文件版本
```powershell
git reset --hard HEAD^      # 回退到上一个版本
git reset --hard HEAD~<n>   # n表示上n个版本
git reset --hard <版本号>   # 版本号只写前几位就能够找到除非有重复
```
该命令用于回退版本，如果进行了回退，在`git log`中将不能看见未来的版本及提交说明，但是如果有未来版本的版本号，使用版本号回到未来版本是可行的。
"HEAD^ / HEAD^^ "表述为上一个版本和上上个版本，"HEAD~100"为上100个版本。
#### 回退时不同的参数影响
- --hard:表示回退到上个版本已提交的状态
- --soft:表示回退到上个版本未提交的状态
- --mixed:表示回退到上个版本已暂存但未提交的状态

### 版本操作记录日志（用于回到未来版本的办法）
```powershell
git reflog
```
该命令将显示git中版本操作的日志，包括版本号、版本回退操作。

### 撤销 工作区/暂存区 操作
```powershell
git restore <文件>              # 撤销工作区操作
git restore --staged <文件>     # 撤销暂存区操作
```
撤销操作包括：恢复删除和撤销修改。对于`git rm`删除的文件而言，这个操作被认为在暂存区操作，可以恢复，但删除的文件需要再次撤销工作区操作才能恢复。

### 删除命令
```powershell
rm <文件>       # 工作区的删除命令（普通的删除命令和你平常删除文件性质一样）
git rm <文件>   # 暂存区的删除命令（git的删除命令）
```
删除与修改的级别是一样的，如果需要从git中完全删除，应当在删除后进行提交，否则只被认为是暂存区级的删除。

### SSH链接创建
在gitbush中输入下述命令
```gitbush
$ ssh-keygen -t  rsa -C "<邮箱>"
```
按照提示enter或y,直到完成为止，此时在 "C:\Users\nbnfcatIntel\.ssh\id_rsa.pub" 中将保存你创建的SSH密码。在 "Github网站\Settings\SSH and GPG keys" 中，创建新的SSH key并把刚刚的SSH密码复制到里面，即可完成配置。
注：
- 使用本方法连接远程仓库时应当使用仓库的SSH地址而非HTTP格式地址。
- 详细的步骤可见：[人人都能看懂的Git教程！Git如何和 GitHub、GitLab 交互？团队如何用 Git 协作开发？小白也能看懂的Git教程! 【based 玄离199 start 6:33】](https://www.bilibili.com/video/BV1d6XVYqEuy/)

### 连接远程仓库
```powershell
git remote add origin <创建的仓库地址HTTP / SSH>
```
该命令将使本地仓库与远程仓库链接。"origin"为仓库名。

### 推送到远程仓库
```powershell
git push -u origin main     # -u将本地main分支与远程仓库origin的main分支链接，在第一次使用即可
git push origin main        # 使用-u后可用该代码，会将本地main分支最新版本文件推送到远程仓库origin的main分支中。
```
该命令将最新版本代码推送到远程仓库。"origin"为仓库名。

### 获得目前本地与远程的链接关系
```powershell
git remote -v
```
该命令将显示目前远程库所链接的远程仓库地址HTTP。

### 删除本地与远程的链接
```powershell
git remote rm origin
```
该命令将使本地仓库与远程仓库"origin"断联，但是一般不会将远程仓库删除。如果要删除，应该去远程仓库处删库。

### 克隆远程仓库
```powershell
git clone <创建的仓库地址HTTP / SSH>    # 似乎SSH会更快
```
该命令将会克隆远程仓库到本地。