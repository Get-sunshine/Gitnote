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

