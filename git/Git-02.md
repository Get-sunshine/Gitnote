## Git基本操作

### 本地仓库操作

1. 在安装好Git后，首次使用需要进行全局配置。

   配置：

   ​	git config --global user.name "用户名" 

   ​	git config --global user.eamil "邮箱地址"

   查看：

   ​	git config --global user.name

   ​	git config --global user.email

2. 创建仓库

   创建仓库时，使用的目录不一定要求是空目录。目录也最好不要出现中文。

   初始化仓库：让Git来管理这个仓库。

   命令：git init 。在项目目录下生成.git的隐藏目录，不能随便更改。

3. Git管理仓库常用指令

   git status:查看当前状态 

   git add:添加到缓存区

   ```
   语法1：git add 文件名； 添加一个文件
   语法2：git add 文件1 文件2 文件3 ...；添加多个文件
   语法3: git add . ;添加当前目录到缓存区
   ```

   git commit -m "注释内容":提交至版本库 

4. Git的版本回退

   步骤：

   1. 查看版本，确定需要回到的时刻点

      命令：

      ​	git log 显示日志

      ​	git log --pretty=oneline

   2. 回退操作

      命令：

      ​	git reset --hard 版本号（提交编号）

   3. （去到未来）注意：回到过去之后，要想再回到之前最新的版本的时候，则需要使用指令去查看历史操作，以得到最新的commit id.

      查看历史操作命令：

      ​	git reflog 

   

   

### 远程仓库的创建



1. 基于https协议

   - 创建空目录，名称和线上仓库名称相同。

   - 使用clone指令克隆线上仓库到本地。 git clone 线上仓库地址。
   - 在仓库上做对应的操作（提交暂存区、提交至本地仓库、提交线上仓库、拉取线上仓库）。
     - 提交到线上仓库：git push 
     - 拉取线上仓库：git pull
2. 基于ssh协议：只是影响github对于用户的身份鉴权方式，与https协议无太大区别。
   - 生成公私钥对指令：ssh-keygen -t rsa -C "注册邮箱"
     - id_rsa :私钥
     - id_rsa.pub :公钥
   - 步骤：
     - 生成客户端公钥文件
     - 将公钥上传至Github
   - 其它操作与基于https的一样





### Git的分支管理

- 每次提交都有记录，Git把他们串成时间线，形成类似于时间轴的东西，这个时间轴就是一个分支，称之为master分支。也称为主分支。
- 分支相关指令：
  - git branch ：查看分支
  - git branch 分支名：创建分支
  - git checkout 分支名：切换分支
  - git branch -d 分支名：删除分支
    - 在删除分支的时候，一点要先退出被删除的分支。
  - git merge 被合并的分支名：合并分支

### 冲突的产生与解决

-  冲突：本地仓库和线上仓库中的内容有不一致的地方，push的时候会报错.先pull，再解决冲突的内容，最后push。

### Git的图形管理工具

- Github for Desktop 
  - GitHub出品的软件，功能完善，使用方便。界面干净，用起来顺手，顶部有时间线。
- Source tree
  - 老牌的Git GUI管理工具，号称是最好用的Git GUI工具。
- TortoiseGit 
  - 简称tgit，中文名海龟git

### 忽略文件

​	在项目的目录下，有很多万年不变的文件目录，例如css、js、image等，或者还有一些目录即便有改动，也不想让其提交到远程仓库的文档，此时就可以使用“忽略文件”机制来实现需求。

​	忽略文件需要新建一个名为**.gitignore**的文件，该文件用于声明忽略文件或不忽略文件的规则，规则对**当前目录，及其子目录生效。**

​	常见的规则：

1. /mtk/   过滤整个文件夹
2. *.zip  过滤所有.zip文件
3. /mtk/do.c 过滤某个具体文件
4. !index.php 不过滤某个文件

