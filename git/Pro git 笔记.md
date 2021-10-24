---
typora-copy-images-to: ./图片笔记
---

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

![1626881598672](.\图片笔记\1626881598672.png)

### 检查当前文件状态 ：git status

```
git status
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean
```

代表在当前分支（master分支，默认分支）下，所有已跟踪文件在上次提交后都未被更改过，当前目录下没有出现任何处于未跟踪状态的新文件。

新创建一个readme文件，再次使用git status

```
git status
On branch master
Your branch is up to date with 'origin/master'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        readme2.txt

nothing added to commit but untracked files present (use "git add" to track)
```

结果显示readme2.txt并未被跟踪。

### 跟踪新文件:git add

跟踪readme2.txt

```
git add readme2.txt
git status
On branch master
Your branch is up to date with 'origin/master'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   readme2.txt
```

显示readme2.txt被跟踪，即放入暂存区，下次提交将会带上readme2.txt文件。

git add 接受目录或文件做为参数。目录：递归寻找目录下的所有文件； 文件：跟踪该文件。

### 暂存已修改文件：git add

修改一个被跟踪的文件（暂存区的文件），再使用git status查看。

```
git status
On branch master
Your branch is up to date with 'origin/master'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   readme2.txt

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   readme2.txt
```

上述出现了两个readme2.txt文件，一个是已跟踪，一个是未跟踪。分别代表上一次提交到暂存区的readme2.txt和这次未提交到暂存区的readme2.txt。如果现在git commit 提交的则是最后一次使用git add后的文件。

即，修改了暂存区的文件，则可以再次git add，将修改后的文件再次存入暂存区。

```
git add readme2.txt
git status
On branch master
Your branch is up to date with 'origin/master'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   readme2.txt
```

git add是一个多功能命令。将文件放到暂存区；合并时把有冲突的文件标记为已解决状态等。 可以将其理解为：添加内容到暂存区，以便下一次提交。

### 状态简览：git status -s  or git status --short

这是一种更为紧凑的格式输出。

```
$ git status -s
 M README
MM Rakefile
A  lib/git.rb
M  lib/simplegit.rb
?? LICENSE.txt
```

新添加的未跟踪文件前有？？标记，新添加到暂存区的文件前有A标记，修改过的文件前有M标记。右边的M表示该文件被修改了但是还未放入暂存区，左边的M表示该文件修改了并放入暂存区。

### 忽略文件 .gitignore

创建一个.gitignore文件，在其中列出不被Git管理的文件。

```
*.[oa]
*~
```

第一行：忽略以.a或.o结尾的文件。第二行：忽略所有以~结尾的文件。

.gitignore文件的格式规范：

- 所有空行或者以#开头的行都会被忽略。
- 可以使用标准的glob模式匹配。
- 可以以（/）开头防止递归。
- 可以以（/）结尾指定目录。
- 要忽略指定模式以外的文件或目录，可以在模式前加上！取反。

glob模式指的是shell使用的简化正则。*匹配零个或多个任意字符；[abc]匹配任何一个在方括号中的字符；？匹配任意一个字符；[0-9]匹配所有0到9的数字。**匹配任意中间目录，例如：

```
a/**/z 可以匹配a/z a/b/z a/b/c/z等
```

### 查看已暂存和未暂存的修改 git diff（git different）

git status 命令的结果略显模糊。可以使用git diff查看文件中具体修改了哪些地方。 git diff 通过文件补丁的格式显示具体哪些行发生了改变。

修改README文件后暂存，编辑CONTRIBUTING.md文件后不暂存。

```
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    modified:   README

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   CONTRIBUTING.md
```

查看尚未暂存的文件更新了哪些部分，直接使用git diff。

```
$ git diff
diff --git a/CONTRIBUTING.md b/CONTRIBUTING.md
index 8ebb991..643e24f 100644
--- a/CONTRIBUTING.md
+++ b/CONTRIBUTING.md
@@ -65,7 +65,8 @@ branch directly, things can get messy.
 Please include a nice description of your changes when you submit your PR;
 if we have to read the whole diff to figure out why you're contributing
 in the first place, you're less likely to get feedback and have your change
-merged in.
+merged in. Also, split your changes into comprehensive chunks if your patch is
+longer than a dozen lines.

 If you are starting to work on a particular area, feel free to submit a PR
 that highlights your work in progress (and note in the PR title that it's
```

查看已暂存的将要添加到下次提交里的内容，使用git diff --cached命令。（Git 1.6.1 及更高版本可以使用git diff --staged命令）。

```
$ git diff --staged
diff --git a/README b/README
new file mode 100644
index 0000000..03902a1
--- /dev/null
+++ b/README
@@ -0,0 +1 @@
+My Project
```

git diff 本身只显示尚未暂存的改动。如果一次性暂存了所有改动，运行git diff后就什么也没有。

### 提交更新： git commit 

暂存区域准备妥当后，使用git commit 提交所有的更新。

```
git commit

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
# On branch master
# Changes to be committed:
#	new file:   README
#	modified:   CONTRIBUTING.md
#
~
~
~
".git/COMMIT_EDITMSG" 9L, 283C
```

这种方式会启动文本编辑器以便输入本次提交的说明。默认的提交消息包含最后一次运行git status 的输出，放在注释行里。开头也会有一个空行，可以输入提交说明。

使用git commit -m '' 将提交信息和命令放在同一行。

### 跳过使用暂存区域：git commit -a

git commit -a 将所有已跟踪过的文件暂存起来一并提交，从而跳过git add步骤：

### 移除文件：git rm

从已跟踪的文件清单中移除某个文件，使用git rm命令。该命令同时会删除工作目录中指定的文件。

如果是简单的删除文件，运行git status 依旧会在未暂存的清单中看见此文件。

```
$ rm PROJECTS.md
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        deleted:    PROJECTS.md

no changes added to commit (use "git add" and/or "git commit -a")
```

使用git rm 记录此次移除文件的操作

```
$ git rm PROJECTS.md
rm 'PROJECTS.md'
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    deleted:    PROJECTS.md
```

下次commit时，此文件就不被Git所管理了。但是删除之前修改过并且已经将该文件放入暂存区的话，则必须强制删除，即加上-f选项。

如果仅仅想把文件从Git仓库中删除，该文件依旧存在于磁盘上，阻止Git的跟踪。使用git rm --cached 命令。

```
git rm --cached READE
```

git rm 命令后可以列出文件或者目录名，也可以使用glob模式。

```
git rm log/\*.log
```
此命令删除log/目录下所有扩展名为.log的文件。
```
git rm \*~
```
此命令删除所有以~结尾的文件。

### 移动文件：git mv

在git中对文件改名：

```
git mv file_from file_to
```

此刻使用git status能够看到关于重命名的操作。

```
$ git mv README.md README
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    renamed:    README.md -> README
```

运行git mv即相当于运行了下面三条命令：

```
mv readme.md readme
git rm readme.md
git add readme
```

