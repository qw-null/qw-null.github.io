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

## 3.Git高层命令
```
git init               初始化仓库
git add ./             将修改添加到暂存区
git commit -m  注释    将暂存区提交到版本库
```
 
##### 3.1git操作最基本流程
创建工作目录 $\rightarrow$  对工作目录进行修改
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



