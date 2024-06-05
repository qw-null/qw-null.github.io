---
title: CentOS7安装Mysql5.7（ARM架构）
date: 2023-11-29 10:39:13
tags:
  - 服务器
---

**第一步：下载 arm 版本离线 mysql 5.7 安装包**
[arm 版本离线 mysql 5.7 安装包](https://obs.cn-north-4.myhuaweicloud.com/obs-mirror-ftp4/database/mysql-5.7.27-aarch64.tar.gz)

**第二步：查询并卸载 CentOS 自带的数据库 Mariadb**
找到数据库 mariadb，如果有会给出一个结果，结果是 mariadb 名称
`rpm -qa | grep mariadb`
如果存在就卸载
`rpm -e --nodeps [查询到的mariadb名称]`

**第三步：创建用户和用户组**
先检查 mysql 用户和用户组有没有被使用
`cat /etc/group | grep mysql`
`cat /etc/passwd | grep mysql`
添加 mysql 用户组 `groupadd mysql`
添加 mysql 用户并加入用户组 `useradd -g mysql mysql`
修改 mysql 用户的登陆密码(这里根据需要设置，可以略过)
`passwd mysql`
`12345678`

**第四步：上传文件至服务器的/usr/local 后解压、改名、授权**
`cd /usr/local`
上传文件
解压安装包 mysql-5.7.27-aarch64.tar.gz
`tar -xvf mysql-5.7.27-aarch64.tar.gz`

将解压后的目录改名为 mysql
`mv mysql-5.7.27-aarch64 mysql`

目录授权操作

```bash
按照下面的操作执行

cd /usr/local/
chown -R mysql mysql/
chgrp -R mysql mysql/
cd mysql/
mkdir data
chown -R mysql:mysql data
```

**第五步：安装 mysql 数据库【目录：/usr/local/mysql/bin】**
`mysqld --initialize --user=mysql --basedir=/usr/local/mysql/ --datadir=/usr/local/mysql/data/`

安装成功输出的日志如下:
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202311300927353.png)
(红线部分即为 root 密码)

---

报错：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202311300928948.png)
解决方案：
原因是因为没有修改环境变量

`vi /etc/profile`
在文件最后一行添加：`export PATH=$PATH:/usr/local/mysql/bin`
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202311300929299.png)

退出后使用命令
`source /etc/profile`

---

报错：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202311300929212.png)
解决方案：
yum install libatomic

---

报错：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202311300930521.png)
解决方案：
原因：因为 `CentOS7` 当前版本默认的 `GCC` 的版本太老，里面的动态链接库没有 `GLIBCXX_3.4.20` 和 `GLIBCXX_3.4.21`。

1.执行命令检查动态库：`strings /usr/lib64/libstdc++.so.6 | grep GLIBC`
输出：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202311300931321.png)
可以看出最高版本是 3.4.19

2.查看 libstdc++.so.6 的位置：`find / -name libstdc++.so.6\*`
输出：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202311300931360.png)
最高版本文件是 `libstdc++.so.6.0.19`

3.下载 GCC 源码，选择合适的版本，本文以 gcc-13.2.0 为例
gcc 各版本下载地址： https://ftp.gnu.org/gnu/gcc/
安装编译环境：
`yum groupinstall "Development Tools"`
`yum install glibc-static libstdc++-static`

解压上传的 gcc-13.2.0：`tar -Jxvf gcc-13.2.0.tar.xz` 【在上传目录中运行】

进入源码目录进行编译：

```bash
cd gcc-13.2.0
./contrib/download_prerequisites
mkdir build
cd build
```

生成 make 文件并且编译（ps:此处编译时间比较久)
`../configure --enable-checking=release --enable-languages=c,c++ --disable-multilib`
`make`
编译完成后安装：`make install`
安装完成后查看版本是否更新：`strings /usr/lib64/libstdc++.so.6 | grep GLIBC`
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202311300933026.png)
发现并没有更新到最新的动态库

查找编译 gcc 时生成的最新动态库：`find / -name "libstdc++.so\*"`
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202311300934751.png)
可以看到生成的最新版本文件在：`/usr/local/mysql/extra/libstdc++.so.6.0.24`
下面拷贝文件到 lib 目录，并重新建立软链接：

```bash
cp /usr/local/mysql/extra/libstdc++.so.6.0.24 /usr/lib64/
cd /usr/lib64
rm libstdc++.so.6
ln -sf /usr/lib64/libstdc++.so.6.0.24 /usr/lib64/libstdc++.so.6
```

最后再确认 GLIBCxx 的版本: `strings /usr/lib64/libstdc++.so.6 |grep GLIBC`

**第六步：安装成功后设置文件和目录权限：**
此时 root 用户 还是在 mysql 目录下执行

```bash
cp ./support-files/mysql.server /etc/init.d/mysqld
chown 777 my.cnf
chmod +x /etc/init.d/mysqld
```

**第七步：修改配置文件**
将 `/etc/init.d/mysqld` 里面的 所有的 `mysql-5.7.27-aarch64` 改为 `mysql`
将 `/usr/local/mysql/my.cnf` 里面所有的 `“socket =”` 后面改为 `/tmp/mysql.sock`

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202311300936543.png)

**第八步：创建日志文件**
创建日志目录
`mkdir /usr/local/mysql/logs`
创建错误日志文件
`echo “” > /usr/local/mysql/logs/mysql-error.log`
授权
`chown -R mysql:mysql /usr/local/mysql/logs/mysql-error.log`

**第九步：启动脚本**
`/etc/init.d/mysqld restart`

**第十步：登录并修改 root 密码**
`mysql -uroot -p`
输入密码（上面操作 bin/mysqld --initialize xxx 生成）

**第十一步：设置开机自启动** 1.先将`/usr/local/mysql/support-files/` 文件夹下的 `mysql.server` 文件复制到 `/etc/rc.d/init.d/` 目录下 `mysqld`
命令: `cp /usr/local/mysql/support-files/mysql.server /etc/rc.d/init.d/mysqld`

赋予可执行权限：`chmod +x /etc/init.d/mysqld`

添加为服务: `chkconfig --add mysqld`

查看服务列表: `chkconfig --list`
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202311300941574.png)
看到 3、4、5 状态为开或者为 on 则表示成功。如果是 关或者 off 则执行一下：`chkconfig --level 345 mysqld on`

重启计算机:`reboot`
