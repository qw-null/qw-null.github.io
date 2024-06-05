---
title: Git教程
date: 2021-11-22 14:34:30
tags:
- Git
---

[尚硅谷Git教程全套完整版](https://www.bilibili.com/video/BV15J411973T?share_source=copy_web)

## 1.版本控制
使用版本控制可以将某个文件回溯到之前的状态，设置将某个项目回退到过去某个时间点的状态。
#### 集中化的版本控制系统 (SVN）
  
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20211122145302.png)
优点：代码存放在单一的服务器上，便于项目管理
缺点：
1. 中央服务器的单点故障
   如果服务器宕机一小时，那么在这一小时内，谁都无法提交更新，也就无法协同工作。
   ><span style="color:blue;">(并不是说服务器故障了就没有办法写代码了,只是在服务器故障的情况下,编写的代码是没有办法得到保障的.试想 svn 中央服务器挂机一天.你还拼命写了一天代码,其中 12 点之前的代码都是高质量可靠的,而且有很多闪光点.而 12 点之后的代码由于你想尝试一个比较大胆的想法,将代码改的面目全非了.这样下来你 12 点之前做的工作也都白费了 有记录的版本只能是 svn 服务器挂掉时保存的版本!)</span>

2. 中央服务器磁盘发生故障
   >要是中央服务器的磁盘发生故障，碰巧没做备份，或者备份不够及时，就会有丢失数据的风险。最坏的情况是彻底丢失整个项目的所有历史更改记录，而客户端偶然提取出来的保存在本地的某些快照数据就成了恢复数据的希望。但这样的话依然是个问题，你不能保证所有的数据都已经有人事先完整提取出来过。只要整个项目的历史记录被保存在单一位置，就有丢失所有历史更新记录的风险。
  

  ![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20211122151436.png)

SVN每次存的都是版本之间的差异代码，需要硬盘空间相对小一些，但是回滚的速度会很慢
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20211220100343.png)

#### 分布式的版本控制系统 （Git）
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20211122152040.png)
*客户端并不只是提取最新版本的文件快照，而是把代码仓库完整地镜像下来*

Git每次存储的都是项目的完整快照，需要的硬盘空间会相对大一些（Git团队对代码做了极致的压缩，最终需要的实际空间比SVN多不了多少），Git的回滚速度极快
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20211122154932.png)

优点：完全的分布式
缺点：学习成本高

## 2.Git底层概念
#### 2.1 基础Linux命令
```bash
clear # 清除屏幕
echo 'test content' #往控制台输出信息 echo 'test content' > test.txt
ll #将当前目录下的 子文件&子目录平铺在控制台
find 目录名 #将对应目录下的子孙文件&子孙目录平铺在控制台
find 目录名 -type f # 将对应目录下的文件平铺在控制台
rm 文件名 #删除文件
mv 源文件 重命名文件 #重命名
cat 文件的url #查看对应文件的内容
vim 文件的url（在英文模式下）
  按i进入插入模式 进行文件的编辑
  按esc键 + 按:键 进入命令的执行
    q! #强制退出（不保存）
    wq #保存退出
    set nu #设置行号
```
#### 2.2 初始化新仓库与.git目录
首次使用
```bash
# git版本信息
git --version
# git配置
git config --global user.name "我的用户名"
git config --global user.email "我的邮箱"
# 查看当前配置
git config --list
# 修改时使用相同的命令进行覆盖
# git config --global user.name "我的用户名"
# git config --global user.email "我的邮箱"
```
初始化新仓库
```bash
git init
1. 初始化后，在当前目录下会出现一个名为 .git 的目录，所有 Git 需要的数据和资源都存放在这个目录中。
2. 要对现有的某个项目开始用 Git 管理，只需到此项目所在的目录，执行：git init
```
.git目录
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20211220142540.png)


#### 2.3 Git区域与对象
##### 2.3.1 区域
1. 工作区（Working Directory）
本地代码，日常书写代码目录，肉眼可见区域（沙箱环境：git不会对其进行管理）
2. 暂存区（Stage 或 Index）
数据暂时存放的区域，可在工作区和版本库之间进行数据的友好交流。
3. 版本库（commit History）
git commit 后给你生成版本的地方,注意push是提交到远程仓库而不是版本库,请勿混淆

###### 工作流程
   > 每个项目都有一个git目录（.git），它是用来保存元数据和对象数据库的地方，每次克隆镜像仓库时，实际上就是拷贝这个目录里的数据。
   >①、在工作目录中修改某些文件
   从项目中取出某个版本的所有文件和目录,用以开始后续工作的叫做工作目录,这些文件实际上都是从Git目录中的压缩对象数据库中提取出来的,接下去就可以在工作目录中对这些文件进行编辑
   >②、保存到暂存区域,对暂存区做快照
   暂存区域只不过是个简单的文件,一般都放在Git目录中,有时候人们会把这个区域的文件叫做索引文件,不过标准说法还是叫暂存区域
   >③、提交更新
   将保存区在暂存区域的文件快照永久转储到本地数据库(Git目录)中

   可以从文件所处的位置来判断状态

##### 2.3.2对象（Git底层概念）
1. Git对象

   key : value 组成的键值对,key是value对应的hash，键值对在git内部都是一个blob类型
   git对象存储文件内容
   git对象代表文件的一次次版本
      ```bash
      》了解即可《
      ① 直接写入git对象方法与读取(存入".git/objects")
      #将打印内容写入对象(git数据库)并且返回其相应哈希值
      echo "写入的对象内容" | git hash-object -w --stdin 

      #读取内容并不能直接cat读取,因为git存入时已经加密,需要如下代码 -p:内容  -t:类型
      git cat-file -p 存入对象的哈希值(此值可以由上一步得到) 

      #将文件写入git对象,即我们常见的版本控制中出现的
      git hash-object -w ./test.txt

      #查看Git存储的数据  返回其文件夹内的所有哈希文件
      find .git/objects -type f 
      ```

2. 树对象
   树对象代表项目的一次次快照
   ```bash
   》了解即可《
   构建树对象
   我们可以通过 update-index , write-tree , read-tree 等命令来构建树对象并且塞到暂存区

   ① 利用 update-index 命令 创建暂存区
   利用 update-index 命令 为test.txt文件的首个版本创建一个暂存区,并通过write-tree命令生成树对象

   #1生成一个树对象
   git update-index --add --cacheinfo 100664(文件状态码:普通文件) 哈希值 对应文件名
   #生成快照(树对象)
   git write-tree
   #2 将第一个树对象加入第二个树对象,使其成为新的树对象
   git read-tree -prefix=bak 哈希值(树对象的)  
   git write-tree
   ② 查看暂存区当前样子
   git ls-files -s
   ```
3. 提交对象 -- 链式
   我们可以通过调用commit-tree命令创建一个提交对象,为此需要指定一个树对象的SHA-1值,为此需要指定一个树对象的SHA-1值 , 以及该提交的父提交对象(如果有的话,第一次将暂存区做快照就没有父对象)


![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220105231626.png)

#### 2.4小结
1. 底层命令
   ```hash
   git对象：
      git hash-object -w fileUrl //生成一个key（hash值）:value（压缩后的文件内容）键值对存到.git/objects
   tree对象
      git update-index --add --cacheinfo 100644 hash test.txt //往暂存区添加一条记录（让git对象 对应上 文件名）存到.git/index
      git write-tree //生成树对象存到.git/objects
   commit对象
      echo 'first commit' | git commit-tree treehash //生成一个提交对象存到.git/objects
   对以上对象的查询
      git cat-file -p hash  //拿对应对象的内容
      git cat-file -t hash  //拿对应对象的类型
   ```
2. 查看暂存区（需要记忆）
   ```git  ls-files  -s```



## 3.Git高层命令
```
git init                     初始化仓库
git add ./                   将修改添加到暂存区
git commit -a                Git 就会自动把所有已经跟踪过的文件暂存起来一并提交，从而跳过 git add 步骤
git commit -m  注释          将暂存区提交到版本库
git status                   查看文件的状态
git diff                     查看哪些修改还没有暂存
git diff --staged            查看哪些修改以及被暂存，还没有提交
git log                      查看操作日志
git rm 文件名                删除工作目录中对应的文件，再将修改添加到暂存区
git mv 原文件名  新文件名     将工作目录中文件进行重命名，再将修改添加到暂存区
---
git branch                   显示分支列表
git branch 分支名            创建分支
git checkout 分支名          切换分支
git branch -d 分支名         删除分支（**不能在本分支上删除该分支，需切出去）
git branch -v                查看每一个分支的最后一次提交
git branch -D 分支名         强制删除分支
git branch name commitHash   新建一个分支并且使分支指向对应的提交对象，commitHash通过下面的命令查找
git log --oneline --decorate --graph --all     查看项目的分叉历史
```
 
##### 3.1 git操作最基本流程
创建工作目录 ——>  对工作目录进行修改
```bash
git add ./
   + 对应底层命令 ：
   git hash-object -w 文件名（修改了多少个工作目录中的文件，此命令就要被执行多少次）
   git update-index...
git commit -m "注释内容"
   + 对应底层命令 ： 
   git write-tree **
   git commit-tree **
``` 
检查当前文件的状态
命令： ``` git  status ```
作用：确定文件当前处于什么状态
工作目录下面的所有文件都不外乎这两种状态：<span style="color:red;">已跟踪</span> 或 <span style="color:red;">未跟踪</span>
已跟踪的文件是指本来就被纳入版本控制管理的文件，在上次快照中有它们的记录，工作一段时间后，它们的状态可能是<span style="color:red;">已提交</span>，<span style="color:red;">已修改</span>或者<span style="color:red;">已暂存</span>

查看操作日志
命令：```git log```
作用：在提交了若干更新，又或者克隆了某个项目之后，想回顾下提交历史。
其他：
```
# 规整查看信息的格式
git log --pretty=oneline
git log --oneline
```

##### 3.2 git的分支
创建分支
命令：```git branch```
作用：为你创建了一个可以移动的新的指针。 比如，创建一个 testing 分支：```git branch testing```。这会在当前所在的提交对象上创建一个指针

Git 的分支本质上是一个提交对象，HEAD是一个指针，默认指向master分支，切换分支时就是让HEAD指向不同的分支

分支切换会改变你工作目录中的文件。在切换分支时，一定要注意你工作目录里的文件会被改变。 如果是切换到一个较旧的分支，你的工作目录会恢复到该分支最后一次提交时的样子。如果 Git 不能干净利落地完成这个任务，它将禁止切换分支

* git切换分支会改变三个地方：HEAD、暂存区、工作目录
* 最佳实践：每次切换分支前，当前分支一定要处于已提交状态
* 切换分支时，如果当前分支上有未暂存的修改 或者 有未提交的暂存（第一次），分支可以切换成功，但是这种操作可能会污染其他分支

##### 3.3小结
已下命令需要记忆
```
# 安装
git --version

# 初始化配置
git config --global user.name "qw"
git config --global user.email qw@qq.com
git config --list

# 初始化仓库
git init

# C（新增）-- 在工作目录中新增文件
git status
git add ./
git commit -m "msg"

# U（修改）-- 在工作目录中修改文件
git status
git add ./
git commit -m "msg"

# D（删除 & 重命名）--在工作目录中删除文件
git rm 要删除的文件
git mv 老文件  新文件
git status
git commit -m "msg"

# R（查询）
git status // 查看工作目录中的文件状态（已跟踪【已提交，已暂存，已修改】 未跟踪）
fir diff // 查看未暂存的修改
git diff --cache // 查看未提交的暂存
git log --oneline // 查看提交记录

# 分支
git log --oneline --decorate --graph --all // 查看整个项目的分支图
git branch // 查看分支列表
git branch -v // 查看分支指向的最新的提交
git branch name // 在当前提交对象上创建新的分支
git branch name commithash // 在指定的提交对象上创建新的分支
git checkout name //切换分支
git branch -d name //删除空的分支或删除已经被合并的分支
git branch -D name // 强制删除分支
```

## 4.Git存储
有时，当你在项目的一部分上已经工作一段时间后，所有东西都进入了混乱的状态，而这时你想要切换到另一个分支做一点别的事情。 问题是，你不想仅仅因为过会儿回到这一点而为做了一半的工作创建一次提交。 针对这个问题的答案是 ```git stash``` 命令。

```git stash ```命令会将未完成的修改保存到一个栈上，而你
可以在任何时候重新应用这些改动(```git stash apply```)

```bash
git stash list    // 查看存储
git stash apply stash@{2}   // 如果不指定一个储藏，Git 认为指定的是最近的储藏
git stash pop     // 来应用储藏然后立即从栈上扔掉它,常使用
git stash drop    // 加上将要移除的储藏的名字来移除它
```

## 5.Git后悔药
三种情景：
1. 工作区：如何撤销自己在工作目录中的修改
   ```git checkout -- filename```
2. 暂存区：如何撤销自己的暂存
   ```git reset HEAD filename```
3. 版本库：如何撤销自己的提交【注释写错】
   ``` git commit --amend  // 重新提交注释信息```

##### 5.1 reset 三部曲(原理)
&star; 1.移动 HEAD 
reset 做的第一件事是移动 HEAD 的指向。
```git reset --soft HEAD~```
- 只动HEAD（带着分支一起移动）

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220110230239.png)
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220110230304.png)

看一眼上图，理解一下发生的事情：它本质上是撤销了上一次 ```git commit ```命令。 当你在运行 ```git commit ```时，Git 会创建一个新的提交，并移动 HEAD 所指向的分支来使其指向该提交。
当你将它 reset 回 HEAD~（HEAD 的父结点）时，其实就是把该分支移动回原来的位置，而不会改变索引和工作目录。 现在你可以更新索引并再次运行 ```git commit``` 来完成 ```git commit --amend ```所要做的事情了。
  
&star;  2.更新暂存区（索引） 
```git reset --mixed HEAD~```
- 动HEAD（带着分支一起移动）
- 动暂存区

&star; 3.更新工作目录 
```git reset --hard HEAD~```
- 动HEAD（带着分支一起移动）
- 动暂存区
- 动工作目录
<span style="color:red;">必须注意，--hard 标记是 reset 命令唯一的危险用法，它也是 Git 会 真正地销毁数据的仅有的几个操作之一。 </span>


git checkout commithash 与 git reset --hard commithash区别：
1. checkout 只动HEAD、--hard动HEAD而且带着分支一起走
2. checkout对工作目录是安全的、--hard是强制覆盖工作目录

路径reset

前面讲述了 reset 基本形式的行为，不过你还可以给它提供一个作用路径。 若指定了一个路径，reset 将会跳过第 1 步，并且将它的作用范围限定为指定的文件或文件集合。
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220110233703.png)
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220110233729.png)


## 6.数据恢复
在你使用 Git 的时候，你可能会意外丢失一次提交。 通常这是因为你强制删除了正在工作的分支，但是最后却发现你还需要这个分支；亦或者硬重置了一个分支，放弃了你想要的提交。 如果这些事情已经发生，该如何找回你的提交呢？
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220111103150.png)
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220111103238.png)
最方便，也是最常用的方法，是使用一个名叫 ```git reflog``` 的工具。
当你正在工作时，Git 会默默地记录每一次你改变 HEAD 时它的值。 每一
次你提交或改变分支，引用日志都会被更新
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220111103454.png)
git reflog 并不能显示足够多的信息。为了使显示的信息更加有用，
我们可以执行 git log -g，这个命令会以标准日志的格式输出引用日志

&star; 恢复

看起来下面的那个就是你丢失的提交，你可以通过创建一个新的分支指向这个提交来恢复它。 例如，你可以创建一个名为 recover-branch 的分支指向这个提交（ab1afef）

```git branch recover-branch ab1afef```

现在有一个名为 recover-branch 的分支是你的 master 分支曾经指向的地方，再一次使得前两次提交可到达了。

## 7.打tag
Git 可以给历史中的某一个提交打上标签，以示重要。 比较有代表性的是人们会使用这个功能来标记发布结点（v1.0 等等）。
```bash

git tag // 列出标签
git show tagname // 查看特定标签
git tag v1.0 // 创建标签1
git tag v1.0 commithash // 创建标签2
git tag -d tagname // 删除标签
git checkout tagname // 检出标签（会造成头部分离，需要再创建一个分支避免）
   + git checkout -b branchname
   
```

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/20220111231847.jpg)
[文件地址](https://github.com/qw-null/BlogImages/blob/master/git%E5%A4%8D%E4%B9%A0.pdf)


## 8.团队协作
1. 项目经理初始化远程仓库
   一定要初始化一个空的仓库，在GitHub上操作
2. 项目经理创建本地仓库
   ```git remote 别名 仓库地址（可选）```
   ```git init```
   将源码复制进来（项目内容）
   修改用户名、邮箱
   ```git add .```
   ```git commit```
3. 项目经理推送本地仓库到远程仓库
   清理windows凭据
   ```git push 别名/项目地址 分支名```（输入项目经理的github用户名和密码；推送完之后会附带生成远程跟踪分支）
4. 项目邀请成员 & 成员接受邀请
   在GitHub上操作
5. 成员克隆远程仓库
   ```git clone 仓库地址```（在本地生成.git文件，默认为远程仓库配了别名 origin）
   只有在克隆的时候，本地分支master和远程分支 别名/master 是有同步关系的
6. 成员贡献代码
   修改源码文件
   ```git add```
   ```git commit```
   ```git push 别名 分支```（输入成员的github用户名和密码；推送完之后会附带生成远程跟踪分支）
7. 项目经理更新修改
   ```git fetch 别名```（将修改同步到远程跟踪分支上）
   ```git merge 远程跟踪分支```
   
##### 8.1正常的数据推送和拉取步骤
1. 确保本地分支已经跟踪了远程跟踪分支
2. 拉取数据：```git pull```
3. 上传数据：```git push```