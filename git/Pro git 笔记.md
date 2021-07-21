# Pro git

## 起步

### 初次运行git前的配置

安装Git之后，应该定制Git环境。Git自带git config 工具用来定制Git环境。

- /etc/gitconfig 文件：包含系统上每一个用户及他们仓库的通用配置。git config 带有--system时，会从此文件读取配置变量。
- ~/.gitconfig 或~/.config/git/config文件：只针对当前用户。git config 带有--global时，会从此文件读取配置。
- 使用当前仓库中的Git目录中的config文件(.git/config)。代表针对该仓库。

上述三个级别，从下往上依次覆盖。

Windows系统中，Git会查找$HOME目录（一般为：C:\Users\$USER）下的.gitconfig文件。Git也会寻找/etc/gitconfig文件，但限于MSys的根目录下，即安装Git时所选的目标位置。

#### 配置用户信息

安装Git之后，首先应该配置自己的用户名称和邮件地址。这些信息会写入到每个Git提交中去，不可更改。

```git
git config --global user.name 'name'
git config --global user.email 'zsy@example.com'
```

强调：使用--global选项后，所有仓库均使用相同的配置。针对特定项目时，在特定项目目录下不带--global来配置。

#### 文本编辑器

配置默认文本编辑器。未配置时，使用操作系统默认的文本编辑器。

```
git config --global core.editor vim
```

#### 检查配置信息

使用git config --list 列出Git找到的所有配置。

可能会有重复的变量名，因为Git会从不同文件中读取同一个配置。此时，Git会使用找到的每个变量的最后一个配置。

查看具体某项配置。

```
git config <key>

git config user.name
```

#### 获取帮助

三种方式找到Git命令的使用手册：

```
git help <verb>
git <verb> --help
man git-<verb>
```

例如：获得git config命令的手册

```
git help config
```

## Git基础

### 获取Git仓库

两种方式：1.在现有项目或目录下导入所有文件到Git中。2.从服务器克隆一个现有的Git仓库。

### 在现有目录中初始化仓库

进入该目录，使用git init 初始化仓库。

初始化后会创建一个.git的子目录，其中含有初始化Git仓库中所有的必须文件。但此时，仅是做了初始化操作，并未跟踪项目里的文件。

使用git add将文件提交到暂存区，执行对文件的跟踪。使用git commit将暂存区的文件提交到本地仓库。

```
git add *.c
git add LICENSE
git commit -m 'initial project version'
```

### 克隆现有仓库

命令：git clone [url]

例如：

```
git clone https://github.com/libgit2/libgit2
```

可以自定义拉取下来的仓库的名字：

```
git clone https://github.com/libgit2/libgit2 mylibgit
```

本地仓库名字就改为：mylibgit

Git支持多种数据传输协议，如HTTPS协议、SSH协议、git://协议。

### 记录每次更新到仓库

工作目录下的每个文件都有两种状态：已跟踪或者未跟踪。已跟踪：即纳入版本控制的文件。未跟踪：即除了已跟踪外的所有文件。初次克隆之后，工作目录中的所有文件都属于已跟踪文件，并处于未修改状态。

文件编辑之后变成已修改，Git标记其为已修改。将已修改文件放入暂存区，再提交所有暂存的修改，如此反复。

使用Git时的生命周期：

![1626881598672](E:\前端\GitNote\Gitnote\git\图片笔记\1626881598672.png)

### 检查当前文件状态 ：git status

