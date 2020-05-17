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



1. 基于http协议

   - 创建空目录，名称和线上仓库名称相同。

   - 使用clone指令克隆线上仓库到本地。 git clone 线上仓库地址。
   - 在仓库上做对应的操作（提交暂存区、提交至本地仓库、提交线上仓库、拉取线上仓库）。
     - 提交到线上仓库：git push 



