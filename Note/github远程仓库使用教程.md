# 一.创建并链接远程仓库

### 1.初始化本地仓库(<>中包的信息都需要替换成自己的)

```bash
git init
```

会增加一个.git的隐藏文件夹,存放了git仓库信息

### 2.配置用户名和密码

```bash
git config --global <user.name>
git config --global <user.email>
```

#### 设置时将引号中的信息替换

### 3.生成SSH Key密钥

```bash
ssh-keygen -t rsa -C <user.name>
```

连按三次回车,直到出现SHA-256图像为止

密钥存放在C:\user\user.name\\.ssh文件夹下

```bash
cat C:/Users/user.name/.ssh/id_rsa.pub
```

#### 注意user.name的替换

复制从ssh-rsa开始到邮箱结尾的所有内容

### 4.配置github密钥

github - settings - SSH and GPG keys - New SSH key

title是备注随便起名

key:将复制的密钥填入,然后点击Add SSH key

### 5.创建github仓库

略

### 6.复制远程仓库的ssh地址

略

### 7.本地仓库链接远程仓库

```bash
git remote add <仓库别名(本地)> <仓库的ssh地址>
git remote				#查看仓库别名
git remote -v			#查看本地所有仓库
```

### 8.测试链接是否成功

```bash
ssh -T git@github.com
```

出现hi用户名什么的说明链接成功(后面的but GitHub does not provide shell access.意思是github不允许shell交互,不是错误)

### 9.提交本地仓库中的代码到远程仓库

```bash
git add .			   	 #提交本地所有文件
git commit -m '备注'		#添加提交备注
git push -u <仓库别名> <分支名>		#首次提交需要加上后面的参数
```

### 10.注意

SSH Key每台设备有一份就足够了,可以操控多个设备

# 二.一些常用命令

### 注意:下以origin来表示最先创建的本地库别名,master为主分支,dev为第二分支

### 1.分支

```bash
git branch		#获取本地所有分支
git branch -a   #查看所有分支,包括远程分支
git branch -vv  #查看本地分支和它们各自上游分支间的跟踪关系
git branch dev	#创建新分支
git checkout dev	#切换分支
git remote remove origin	#删除指定名字的远程仓库
```

```bash
修改分支名称
如果不指定原分支名称则为当前所在分支
git branch -m master master1
强制修改分支名称
git branch -M master master1

删除指定的本地分支
git branch -d master

强制删除指定的本地分支
git branch -D master
```

```bash
github上的版本发布

本地版本号
git tag -a v1.0.0

把这个版本发布到线上
git push --tags
```

```bash
远程仓库名修改，本地需要先删除，重新添加

查看本地所有仓库
git remote -v

删除本地仓库
git remote rm origin 

重新添加
git remote add origin <远程仓库链接>
```

### 2.git pull

git pull 命令用于从远程获取代码并合并本地的版本。
git pull 其实就是 git fetch(拉取) 和 git merge(合并) 的简写。 命令格式如下：

```bash
git pull <远程主机名> <远程分支名>:<本地分支名>

示例操作:
# 基本的更新
$ git pull
$ git pull origin

# 将远程主机 origin 的 master 分支拉取过来，与本地的 dev 分支合并。
git pull origin master:dev

#如果远程分支是与当前分支合并，则冒号后面的部分可以省略。
git pull origin master
```

# 三.分支管理

### 1.新建分支

① 从已有的分支创建新的分支(如从master分支), 创建一个dev分支，并切换到dev分支

```bash
git checkout -b dev
```

② 创建完可以查看一下,分支已经切换到dev （会列出所有分支，当前分支的面会有一个*号）

```bash
git branch
```

③ 提交dev分支到远程仓库

```bash
git push origin dev
```

### 2.拉取分支

① 把远程分支拉到本地

```bash
git fetch origin dev
```

② 在本地创建分支dev并切换到该分支

```bash
git checkout -b dev
```

③ 把某个分支上的内容都拉取到本地

```bash
git pull origin dev
```

### 3.提交分支

① 首先切换到dev分支上,进行提交推送

```bash
git checkout dev
```

② 推送到dev分支上

```bash
git add .
git commit -m ‘dev'
git push -u origin dev
```

### 4.合并分支

① 首先切换到master分支上

```bash
git checkout master
```

② 如果是多人开发的话 需要把远程master上的代码pull下来

```bash
git pull origin master
```

③ 把dev分支的代码合并到master上

```bash
git merge dev
```

④ push到远程master上

```bash
git push origin master
```

### 5.删除分支

① 删除本地分支

```bash
git branch -d dev
```

② 删除远程分支

```bash
git push <库别名> -d <远程分支>
```

### 6.分支改名

### 假设分支名称为old,想要修改为new

① 本地分支重命名(还没有推送到远程)

```bash
git branch -m old new
```

② 远程分支重命名 (已经推送远程-假设本地分支和远程对应分支名称相同)

```bash
# a. 重命名远程分支对应的本地分支

git branch -m old new

# b. 删除远程分支

git push --delete origin old

# c.上传新命名的本地分支

git push origin new

# d.把修改后的本地分支与远程分支关联

git branch --set-upstream-to origin/new

# c和d可以用一句来解决

git push -u origin new

# 若已经关联了远程分支,需要先解除关联
git branch --unset-upstream
```

# 四.标签与版本管理

### 1.标签相关命令

```bash
git tag <标签名>			#打标签
git tag					  #查看所有标签
git tag -a <标签名> -m "标签信息"		#打指定标签信息
git checkout <标签名>		#切换到指定标签
git show <标签名>			#查看标签文字说明
```

### 2.推送与删除

```bash
git push origin <标签名>		#推送标签
git push origin --tags		  #一次性推送全部未到达远程的标签名

git tag -d <标签名>			#本地删除
git push origin :refs/tags/<标签名>	#远程删除某个
:refs/tags/ 的语法表示删除远程仓库中的所有标签。
```

在 Git 中，`refs` 是引用（references）的简称。它们是指向特定提交对象的指针。常见的 `refs` 包括分支（branches）、标签（tags）和远程引用（remote references）。

### 3.版本回退

```bash
#查看提交的序号，也可以直接在服务器看文件
git log   

#记得先提交保存
git reset --hard xxx   

#此时如果用 “git push” 会报错，因为我们本地库HEAD指向的版本比远程库的要旧：
git push -f    #强推

git reset [--soft | --mixed | --hard] [HEAD]
--mixed 为默认，可以不用带该参数，用于重置暂存区的文件与上一次的提交(commit)保持一致，工作区文件内容保持不变。
--soft 参数用于回退到某个版本
--hard 参数撤销工作区中所有未提交的修改内容，将暂存区与工作区都回到上一次版本，并删除之前的所有信息提交
```

```bash
git reset命令用于重置当前分支的HEAD到指定的状态，并且可以选择性地重置工作目录和暂存区的内容。它有三个主要选项：--soft、--mixed 和 --hard。

用法示例：

1. --soft：

   - 仅重置HEAD指针到指定的提交，不改变暂存区和工作目录。

   - 例如，将HEAD指针重置到上一个提交：

     git reset --soft HEAD~1

2. --mixed（默认选项）：

   - 重置HEAD指针到指定的提交，并重置暂存区的内容，但不改变工作目录。

   - 例如，将HEAD指针和暂存区重置到上一个提交：

     git reset --mixed HEAD~1

3. --hard

   - 重置HEAD指针到指定的提交，并重置暂存区和工作目录的内容。

   - 例如，将HEAD指针、暂存区和工作目录重置到上一个提交：

     git reset --hard HEAD~1

参数说明：

- HEAD：当前分支的最新提交。
- HEAD~1：当前分支的上一个提交。
- [commit]：可以是任意提交的SHA-1哈希值或引用。

示例说明：

假设当前分支有三个提交，分别是 A、B 和 C，当前HEAD指向 C。

(1). 使用 git reset --soft HEAD~1 后，HEAD指针将指向 B，但 C 的更改仍保留在暂存区。
(2). 使用 git reset --mixed HEAD~1 后，HEAD指针将指向 B，C 的更改从暂存区移除，但仍保留在工作目录。
(3). 使用 git reset --hard HEAD~1 后，HEAD指针将指向 B，C 的更改从暂存区和工作目录中移除。

通过这些选项，你可以灵活地控制重置的范围和影响。
```

