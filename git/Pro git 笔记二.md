# Git基础

## 查看提交历史: git log

使用Pro Git的例子：

```
git clone https://github.com/schacon/simplegit-progit
```

运行git log：

```
$ git log
commit ca82a6dff817ec66f44342007202690a93763949 (HEAD -> master, origin/master, origin/HEAD)
Author: Scott Chacon <schacon@gmail.com>
Date:   Mon Mar 17 21:52:11 2008 -0700

    changed the verison number

commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
Author: Scott Chacon <schacon@gmail.com>
Date:   Sat Mar 15 16:40:33 2008 -0700

    removed unnecessary test code

commit a11bef06a3f659402fe7563abf99ad00de2209e6
Author: Scott Chacon <schacon@gmail.com>
Date:   Sat Mar 15 10:31:28 2008 -0700

    first commit

```

不加任何参数时，git log按提交时间列出所有的更新，最近的更新在最上面。

-p 显示每次提交的内容差异 ，-2显示最近两次提交。

```
git log -p -2
$ git log -p -2
commit ca82a6dff817ec66f44342007202690a93763949 (HEAD -> master, origin/master, origin/HEAD)
Author: Scott Chacon <schacon@gmail.com>
Date:   Mon Mar 17 21:52:11 2008 -0700

    changed the verison number

diff --git a/Rakefile b/Rakefile
index a874b73..8f94139 100644
--- a/Rakefile
+++ b/Rakefile
@@ -5,7 +5,7 @@ require 'rake/gempackagetask'
 spec = Gem::Specification.new do |s|
     s.platform  =   Gem::Platform::RUBY
     s.name      =   "simplegit"
-    s.version   =   "0.1.0"
+    s.version   =   "0.1.1"
     s.author    =   "Scott Chacon"
     s.email     =   "schacon@gmail.com"
     s.summary   =   "A simple gem for using Git in Ruby code."

commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
Author: Scott Chacon <schacon@gmail.com>
Date:   Sat Mar 15 16:40:33 2008 -0700

    removed unnecessary test code

diff --git a/lib/simplegit.rb b/lib/simplegit.rb
index a0a60ae..47c6340 100644
--- a/lib/simplegit.rb
+++ b/lib/simplegit.rb
@@ -18,8 +18,3 @@ class SimpleGit
     end

 end
-
-if $0 == __FILE__
-  git = SimpleGit.new
-  puts git.show
-end
\ No newline at end of file

```

使用--stat查看每次提交的简略统计信息。

```
git log --stat
$ git log --stat
commit ca82a6dff817ec66f44342007202690a93763949 (HEAD -> master, origin/master, origin/HEAD)
Author: Scott Chacon <schacon@gmail.com>
Date:   Mon Mar 17 21:52:11 2008 -0700

    changed the verison number

 Rakefile | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)

commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
Author: Scott Chacon <schacon@gmail.com>
Date:   Sat Mar 15 16:40:33 2008 -0700

    removed unnecessary test code

 lib/simplegit.rb | 5 -----
 1 file changed, 5 deletions(-)

commit a11bef06a3f659402fe7563abf99ad00de2209e6
Author: Scott Chacon <schacon@gmail.com>
Date:   Sat Mar 15 10:31:28 2008 -0700

    first commit

 README           |  6 ++++++
 Rakefile         | 23 +++++++++++++++++++++++
 lib/simplegit.rb | 25 +++++++++++++++++++++++++
 3 files changed, 54 insertions(+)

```

### 关于git log的命令的多余内容这儿有一点省略，可以去Pro git书籍查看。

### 撤销操作：git commit --amend

如若commit之后，有几个文件没有提交或者提交信息写错了，可以使用git commit --amend重新提交。

```
git commit --amend
```
该命令会将暂存区中的文件提交。如果提交后立即使用此命令，快照会保持不变，更改的只是提交信息而已。
### 取消暂存文件：git reset
使用git reset HEAD  <file> 来取消暂存文件

加上--hard选项会使git reset 变成一个危险的命令，但是git reset并不是。后面详解。

### 撤销对文件的修改：git checkout	

例如不想保存对CONTRIBUTING.md文件的修改。

```
$ git checkout -- CONTRIBUTING.md
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    renamed:    README.md -> README
```

git checkout -- [file] 是一个危险的命令。这个命令会让你对某个文件做的任何修改都会消失（本质上是拷贝了一个文件来覆盖此文件？）。

Git中已提交的东西，几乎总是可以恢复的。但是，任何未提交的东西，丢失后可能就无法找回了。

### 远程仓库的使用 

远程仓库：托管在因特网或其他网络中的项目的版本库。可以有多个远程仓库，有些仓库对你只读，有些你则可以读写。

### 查看远程仓库： git remote 

git remote ：查看已经配置的远程仓库服务器。会列出你指定的每一个远程服务器的简写。如果已经克隆了仓库，至少应该看到origin-，这是Git给你克隆的仓库服务器的默认名字。

git remote -v：显示需要读写远程仓库使用的简写与对应的URL。

```
$ git remote -v
origin	https://github.com/schacon/ticgit (fetch)
origin	https://github.com/schacon/ticgit (push)
```

多个远程仓库时，该命令会列出所有的远程仓库。

```
$ cd grit
$ git remote -v
bakkdoor  https://github.com/bakkdoor/grit (fetch)
bakkdoor  https://github.com/bakkdoor/grit (push)
cho45     https://github.com/cho45/grit (fetch)
cho45     https://github.com/cho45/grit (push)
defunkt   https://github.com/defunkt/grit (fetch)
defunkt   https://github.com/defunkt/grit (push)
koke      git://github.com/koke/grit.git (fetch)
koke      git://github.com/koke/grit.git (push)
origin    git@github.com:mojombo/grit.git (fetch)
origin    git@github.com:mojombo/grit.git (push)
```

### 添加远程仓库：git remote add <shortname>  <url>

```console
$ git remote
origin
$ git remote add pb https://github.com/paulboone/ticgit
$ git remote -v
origin	https://github.com/schacon/ticgit (fetch)
origin	https://github.com/schacon/ticgit (push)
pb	https://github.com/paulboone/ticgit (fetch)
pb	https://github.com/paulboone/ticgit (push)
```

### 从远程仓库中抓取与拉取：git fetch [remote-name]

从远程仓库中获取数据：git fetch [remote-name]

该命令访问远程仓库，拉取所有你还没有的数据。之后，你会拥有该远程仓库中所有分支的引用，可以随时合并或查看。

使用clone克隆仓库时，该仓库会自动添加为远程仓库，并默认origin为简写。因此，git fetch origin会抓取克隆后新推送的所有工作。git fetch将数据拉取到本地仓库，并不会自动合并或修改当前的工作，需要将其手动合并。

如果一个分支设置为跟踪一个远程分支，就可以使用git pull自动抓取，然后远程分支合并到当前分支。这样更简单。默认，git clone命令自动设置本地master分支跟踪克隆的远程仓库的master分支（或不管是什么名字的默认分支）。git pull通常从最初克隆的服务器上抓取数据并自动尝试合并到当前所在的分支。

### 推送到远程仓库：git push [remote-name] [branch-name]

推送master分支到origin服务器。git push origin master。

如果在你推送之前，已经有人推送了。就需要先拉取别人的工作，合并了之后，才能继续推送。

### 查看远程仓库：git remote show [remote-name]

查看一个远程仓库详细一点的信息：git remote show [remote-name]

例如：git remote show origin

```
$ git remote show origin
* remote origin
  Fetch URL: https://github.com/schacon/ticgit
  Push  URL: https://github.com/schacon/ticgit
  HEAD branch: master
  Remote branches:
    master                               tracked
    dev-branch                           tracked
  Local branch configured for 'git pull':
    master merges with remote master
  Local ref configured for 'git push':
    master pushes to master (up to date)
```

### 远程仓库的移除与重命名：git remote rename

git remote rename 修改远程仓库的简写名。例如：git remote rename pb paul 。这同样也会修改远程分支的名字。pb/master-->paul/master

git remote rm 移除一个远程仓库。例如：git remote rm pb

### 打标签

Git可以给某个提交打上标签，以示重要。

### 列出标签：git tag

在Git中列出已有的标签：git tag 

该命令以字母顺序列出标签。

可以使用特定的模式查找标签。例如：git tag -l 'v1.8.5*'   该命令表示列出1.8.5系列的标签。

### 创建标签：

Git使用两种主要类型的标签：轻量标签、附注标签。

轻量标签：一个特定提交的引用。

附注标签：Git数据库中的一个完整对象。可以被校验，包含打标签者的名字、电子邮件地址、日期时间、标签信息；通常建议创建附注标签。

#### 创建附注标签：git tag -a  

例如:

```
git tag -a v1.0 -m 'my version 1.0'
```

-m 指定一条存储在标签中的信息。没有使用-m选项是，Git会启动编辑器，要求输入信息。

git show 显示标签信息与对应的提交信息。

```
$ git show v1.4
tag v1.4
Tagger: Ben Straub <ben@straub.cc>
Date:   Sat May 3 20:19:12 2014 -0700

my version 1.4

commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Mon Mar 17 21:52:11 2008 -0700

    changed the version number
```

#### 轻量标签：git tag [tagname]

轻量标签：本质上是将提交检验和存储到一个文件中-没有保存任何其他信息。

```
git tag v1.1
```

此时，运行git show，不会看到额外的信息。

```
$ git show v1.1
commit 6906bb31815cd739420fa181c0cedf2ce2b63662 (HEAD -> master, tag: v1.1, tag: v1.0)
Author: Scott Chacon <schacon@gmail.com>
Date:   Mon Mar 17 21:52:11 2008 -0700

    changed the verison number

```

### 后期打标签

可以对过去的提交打标签。仅需要在命令的末尾指定提交的校验和（或部分校验和）。

首先找到提交历史：

```
$ git log --pretty=oneline
15027957951b64cf874c3557a0f3547bd83b3ff6 Merge branch 'experiment'
a6b4c97498bd301d84096da251c98a07c7723e65 beginning write support
0d52aaab4479697da7686c15f77a3d64d9165190 one more thing
6d52a271eda8725415634dd79daabbc4d9b6008e Merge branch 'experiment'
0b7434d86859cc7b8c3d5e1dddfed66ff742fcbc added a commit function
4682c3261057305bdd616e23b64b0857d832627b added a todo file
166ae0c4d3f420721acbb115cc33848dfcc2121a started write support
9fceb02d0ae598e95dc970b74767f19372d61af8 updated rakefile
964f16d36dfccde844893cac5b347e7b3d44abbc commit the todo
8a5cbc430f1a9c3d00faaeffd07798508422908a updated readme
```

然后给指定的历史提交打标签：

```
git tag -a v1.2 9fceb02
```

### 共享标签：git  push origin [tagname]

默认：git push不会将标签传送到远程仓库服务器上。需要显示的推送。

```
$ git push origin v1.5
Counting objects: 14, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (12/12), done.
Writing objects: 100% (14/14), 2.05 KiB | 0 bytes/s, done.
Total 14 (delta 3), reused 0 (delta 0)
To git@github.com:schacon/simplegit.git
 * [new tag]         v1.5 -> v1.5
```

git push origin --tags 一次性推送多个标签。将所有不在服务器上的标签都传送过去。

```
$ git push origin --tags
Counting objects: 1, done.
Writing objects: 100% (1/1), 160 bytes | 0 bytes/s, done.
Total 1 (delta 0), reused 0 (delta 0)
To git@github.com:schacon/simplegit.git
 * [new tag]         v1.4 -> v1.4
 * [new tag]         v1.4-lw -> v1.4-lw
```

### 检出标签？：git checkout -b [branchname] [tagname]

Git中标签不能像分支一样来回移动，因此不能检出标签。（不能检出标签是个啥意思？检出远程仓库的标签到本地工作目录中？）如果需要工作目录与仓库中特定的标签版本完全一致，可以使用 git checkout -b [branchname] [tagname] 在标签上创建一个分支。

```
$ git checkout -b version2 v2.0.0
Switched to a new branch 'version2'
```

### Git别名

git别名，即为git命令设置简写。

```
$ git config --global alias.co checkout
$ git config --global alias.br branch
$ git config --global alias.ci commit
$ git config --global alias.st status
```

设置结果：

```
git checkout --> git co
git branch --> git br
git commit --> git ci
git status --> git st
```

