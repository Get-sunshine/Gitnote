## Git分支

Git的分支模型，是git的‘必杀技特性’。

### 分支简介

先回顾一下Git是如何保存数据的。

Git保存的是一系列不同时刻的文件快照，而不是文件的变化或者差异。

提交操作时，Git会保存一个提交对象（commit object）。该提交对象保存了作者的姓名、邮箱、提交时输入的信息以及指向它的父对象的指针。首个提交对象没有父对象，普通提交对象有父对象，由多个分支合并产生的提交对象有多个父对象。

使用一个例子来说明：

假设有一个工作目录，包含三个将要被暂存和提交的文件。暂存操作为每个文件计算校验和（SHA-1 哈希算法），然后把当前版本的文件快照保存到Git仓库中（Git使用blob对象保存快照），最终将校验和加入到暂存区等待提交：

```
$ git add README test.rb LICENSE
$ git commit -m 'The initial commit of my project'
```

git commit提交时，Git先计算每个子目录的校验和，然后在Git仓库中这些校验和保存为树对象。之后，Git创建一个提交对象，该对象包含前面所述信息，还包含指向这个树对象的指针。然后，Git就可以在需要时重现此次的快照。

现在，Git仓库中有五个对象：三个blob对象（保存文件快照）、一个树对象（记录目录结构和blob对象索引）以及一个提交对象（包含指向前述树对象的指针和所有提交信息）

![1629302413401](E:\前端\GitNote\Gitnote\git\图片笔记\1629302413401.png)

提交后，再次修改，再次提交。这次的提交对象会包含指向上次提交对象（父对象）的指针。

![1629603517804](E:\前端\GitNote\Gitnote\git\图片笔记\1629603517804.png)

Git分支本质：指向提交对象的可变指针。 Git默认分支：master。多次提交后，会有指向最后那个提交对象的master分支，它会在每次的提交操作中自动向前移动。

master分支与其他分支完全相同，不是一个特殊的分支。 该分支是git init命令创建的，大多数人懒得更改而已。

![1629603876532](E:\前端\GitNote\Gitnote\git\图片笔记\1629603876532.png)

#### 创建分支：git branch 

Git创建分支，只是创建了可以移动的新的指针。

```
git branch testing
```

该命令创建了testing分支，会在当前所在提交对象上创建新指针。

![1629621887706](E:\前端\GitNote\Gitnote\git\图片笔记\1629621887706.png)

如何确定当前分支是哪个？ Git具有特殊的HEAD指针，指向当前所在的本地分支（即HEAD是当前本地分支的别名？） git branch 仅创建一个分支，不会自动切换分支。

![1629623280871](E:\前端\GitNote\Gitnote\git\图片笔记\1629623280871.png)

简单的使用git log 查看各个分支当前所指的对象。使用参数--decorate

```
$ git log --oneline --decorate
f30ab (HEAD, master, testing) add feature #32 - ability to add new
34ac2 fixed bug #1328 - stack overflow under certain conditions
98ca9 initial commit of my project
```

结果显示，当前的master分支和testing分支均指向同一个提交对象。

#### 切换分支：git checkout

```
$ git checkout testing
```

切换到 testing分支。HEAD也会指向testing分支。

![1629627812088](E:\前端\GitNote\Gitnote\git\图片笔记\1629627812088.png)

这样的实现方式会带来什么好处呢？再次提交一次代码

```
$ vim test.rb
$ git commit -a -m 'made a change'
```

![1629628235361](E:\前端\GitNote\Gitnote\git\图片笔记\1629628235361.png)

testing分支向前移动了，但是master分支没有移动，依旧指向运行git checkout时指向的对象。切换回master分支。

```
$ git checkout master
```

![1629628796243](E:\前端\GitNote\Gitnote\git\图片笔记\1629628796243.png)

该命令做了两件事，一：使HEAD指回master分支；二：将工作目录恢复成master分支指向的快照。

注意：切换分支时，工作目录里的文件会被改变。切换到较旧的分支时，工作目录会恢复到该分支最后一次提交的样子。如果不能恢复，则禁止切换分支。

再次修改，再次提交：

```
$ vim test.rb
$ git commit -a -m 'made other changes'
```

此刻，项目的提交历史产生分叉。因为创建了test分支后，切换到test分支进行了工作，又切换回master分支进行了另外的工作。可以在不同的分支之间来回切换和工作，并在时机成熟时将它们合并起来。

![1629906648405](E:\前端\GitNote\Gitnote\git\图片笔记\1629906648405.png)

可以使用git log命令简单的查看分支的历史。运行 git log --oneline --decorate --graph --all，会输出提交历史，各个分支的指向以及项目的分支分叉情况。

```
$ git log --oneline --decorate --graph --all
* c2b9e (HEAD, master) made other changes
| * 87ab2 (testing) made a change
|/
* f30ab add feature #32 - ability to add new formats to the
* 34ac2 fixed bug #1328 - stack overflow under certain conditions
* 98ca9 initial commit of my project
```

### 分支的新建与合并

#### 新建分支

首先，假设在项目上工作，并有一些提交。

![1629907115680](E:\前端\GitNote\Gitnote\git\图片笔记\1629907115680.png)

为了解决某个问题，新建一个分支，新建的同时切换到该分支上工作。

```
$ git checkout -b iss53
Switched to a new branch 'iss53'
```

该命令等同于：

```
$ git branch iss53
$ git chcekout iss53
```

此时：

![1629992156804](E:\前端\GitNote\Gitnote\git\图片笔记\1629992156804.png)

继续在该分支上工作，并做提交。

```
$ vim index.html
$ git commit -a -m 'added a new footer [issue 53]'
```

![1629992233279](E:\前端\GitNote\Gitnote\git\图片笔记\1629992233279.png)

假设在解决这个问题的同时，突发一个紧急问题。首先，切换到master分支。

```
$ git checkout master
```

针对该紧急问题建立一个分支，在该分支上工作，直到问题解决：

```
$ git checkout -b hotfix
Switched to a new branch 'hotfix'
$ vim index.html
$ git commit -a -m 'fixed the broken email address'
[hotfix 1fb7853] fixed the broken email address
 1 file changed, 2 insertions(+)
```

![1629993191563](E:\前端\GitNote\Gitnote\git\图片笔记\1629993191563.png)
然后将该分支合并到master分支，部署到线上。
```
$ git checkout master
$ git merge hotfix
Updating f42c576..3a0874c
Fast-forward
 index.html | 2 ++
 1 file changed, 2 insertions(+)
```
注意fast-forward这个词。由于master分支是该分支的直接上游，因此git只是简单移动指针。

![1629993774821](E:\前端\GitNote\Gitnote\git\图片笔记\1629993774821.png)
该紧急问题被解决后,需要回到之前的工作中去.首先,删除hotfix分支，master分支已经合并了，不需要该分支了。
```
$ git branch -d hotfix
Deleted branch hotfix (3a0874c).
```

#### 分支的合并：git merge

假设iss53上的分支已经修复，需要将iss53分支合并到master分支。首先，切换到master分支，再运行git merge命令。

```
$ git checkout master
Switched to branch 'master'
$ git merge iss53
Merge made by the 'recursive' strategy.
index.html |    1 +
1 file changed, 1 insertion(+)
```

这次与合并hotfix有些不同，这次是一个简单的三方合并。即两个分支没有共同的祖先，分别从两个分支的末端快照与一个共同祖先进行三方合并。

![1630509427644](E:\前端\GitNote\Gitnote\git\图片笔记\1630509427644.png)

此次三方合并的结果做了一个新快照并且自动创建一个新的提交指向它。其被称为一次合并提交。特别之处：有不止一个父提交。

![1630509837726](E:\前端\GitNote\Gitnote\git\图片笔记\1630509837726.png)

#### 遇到冲突时的分支合并

