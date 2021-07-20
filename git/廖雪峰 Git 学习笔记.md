# 1 基本概念？

版本库？ 版本库即仓库（repository）。可以理解为一个目录，目录里所有的文件都可以被git管理，能跟踪所有文件的修改、删除。

如何创建一个版本库？新建一个空目录-->使用git init初始化为一个Git仓库（目录中会生成一个.git隐藏文件）。

新建test.txt文件，添加内容。

```
git add test.txt  // 添加到暂存区
git commit -m 'message' // 添加到本地仓库
```

修改test.txt文件

```
git status // 查看当前仓库的状态
git diff <filename> // 查看文件具体更改情况 git diff即git difference 的缩写
```

