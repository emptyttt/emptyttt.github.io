# Git

## 一些配置

在工作目录下右键打开**git bash**命令行

linux常用指令：

|   指令    |     功能     |
| :-------: | :----------: |
| ls/ll/pwd | 查看当前目录 |
|    cat    | 查看文件内容 |
|   touch   |   创建文件   |
|    vi     |   vi编辑器   |

```bash
git config --global user.name Anna

git config --global user.email 1825155344@qq.com
```

**为常用指令配置别名**

有些常用的指令参数非常多，每次都要输入好多参数，我们可以使用别名。

1. 打开用户目录，创建 .bashrc 文件

部分windows系统不允许用户创建点号开头的文件，可以打开gitBash,执行

```
 touch ~/.bashrc
```

2. bashrc 文件中输入如下内容：

```bash
#用于输出git提交日志
alias git-log='git log --pretty=oneline --all --graph --abbrev-commit'
#用于输出当前目录所有文件及基本信息
alias ll='ls -al'
```

3. 更新

```bash
source ~/.bashrc
```

## 获取本地仓库

```
git init
```

![image-20250329223439904](C:\Users\Anna\AppData\Roaming\Typora\typora-user-images\image-20250329223439904.png)

|                 命令                 |               功能               |
| :----------------------------------: | :------------------------------: |
|              git status              | 查看修改的状态（暂存区、工作区） |
| git add 文件名/git add .（添加所有） |        添加工作区到暂存区        |
|           git commit -m ""           |       添加暂存区到本地仓库       |
|               git log                |           查看提交日志           |
|      git reset --hard commitID       |             版本切换             |
|              git reflog              |       查看已经删除的奖记录       |

命令形式：git log [option]

options：

- --all 显示所有分支

- --pretty=oneline 将提交信息显示为一行

- --abbrev-commit 使得输出的commitId更简短

- --graph 以图的形式显示

```bash
#用于输出git提交日志
alias git-log='git log --pretty=oneline --all --graph --abbrev-commit'
```

```
touch .gitignore
git status
```

.gitignorede的文件内容，会忽略一些后缀文件

```
# no .a files
*.a
# but do track lib.a, even though you're ignoring .a files above
!lib.a
# only ignore the TODO file in the current directory, not subdir/TODO
/TODO
# ignore all files in the build/ directory
build/
# ignore doc/notes.txt, but not doc/server/arch.txt
doc/*.txt
# ignore all .pdf files in the doc/ directory
doc/**/*.pdf
###############
```



## 分支

|          命令          |                 功能                 |
| :--------------------: | :----------------------------------: |
|       git branch       |             查看本地分支             |
|   git branch 分支名    |             创建本地分支             |
|  git checkout 分支名   |               切换分支               |
| git checkout -b 分支名 | 切换到一个不存在的分支（创建并切换） |
|       git merge        |               合并分支               |
|  git branch -d 分支名  |      删除分支时，需要做各种检查      |
|  git branch -D 分支名  |        删除分支，不做任何检查        |

解决冲突：

![image-20250330113300838](C:\Users\Anna\AppData\Roaming\Typora\typora-user-images\image-20250330113300838.png)

几乎所有的版本控制系统都以某种形式支持分支。 使用分支意味着你可以把你的工作从开发主线上分离

开来进行重大的Bug修改、开发新的功能，以免影响开发主线。

在开发中，一般有如下分支使用原则与流程：

- master （生产） 分支

线上分支，主分支，中小规模项目作为线上运行的应用对应的分支；

- develop（开发）分支

是从master创建的分支，一般作为开发部门的主要开发分支，如果没有其他并行开发不同期上线要求，都可以在此版本进行开发，阶段开发完成后，需要是合并到master分支,准备上线。

- feature/xxxx分支

从develop创建的分支，一般是同期并行开发，但不同期上线时创建的分支，分支上的研发任务完成后合并到develop分支。

- hotfix/xxxx分支

从master派生的分支，一般作为线上bug修复使用，修复完成后需要合并到master、test、develop分支。还有一些其他分支，在此不再详述，例如test分支（用于代码测试）、pre分支（预上线分支）等等。

![image-20250330113711264](C:\Users\Anna\AppData\Roaming\Typora\typora-user-images\image-20250330113711264.png)
