---
title: Mysql本地数据上传至服务器
date: 2023-11-14 10:32:39
tags:
  - Mysql
---

## 1 任务：

将本地 Mysql 中的数据迁移至线上服务器
线上 Mysql 服务器创建具有相关权限的用户
本地程序更换线上 Mysql 地址

## 2 实现：

**安装 Mysql 并配置远程访问**

1. 第一步下载对应系统版本的 mysql，并解压
   https://dev.mysql.com/downloads/mysql/
   ![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202311141033139.png)
   【其中 data 和 my.ini 是手动创建】

```
## my.ini文件

[mysqld]
# 设置3306端口
port=3306
# 设置mysql的安装目录
basedir=E://software_qw//mysql-5.7.43-winx64
# 设置mysql数据库的数据的存放目录
datadir=E://software_qw//mysql-5.7.43-winx64//data
# 允许最大连接数
max_connections=200
# 允许连接失败的次数。
max_connect_errors=10
wait_timeout=31536000
interactive_timeout=31536000
# 服务端使用的字符集默认为UTF8
character-set-server=utf8
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
# 默认使用“mysql_native_password”插件认证
#mysql_native_password
default_authentication_plugin=mysql_native_password
[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8
[client]
# 设置mysql客户端连接服务端时默认使用的端口
port=3306
default-character-set=utf8
```

data 是空文件夹

2. 用管理员运行 cmd 进入解压目录的 bin 目录

运行命令 `mysqld --initialize --console`
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202311141035726.png)
【红框的位置是 root 用户的密码】

运行命令 安装服务 `mysqld --install`

3. 第三步 启动 mysql 服务

`net start mysql`

4. 第四步 登录 mysql

`mysql -u root -p  上面红框中的密码（或者直接回车，等到出现 password: 再输入）`

5. 第五步 修改 root 密码

`ALTER USER "root"@"localhost" IDENTIFIED  BY "root1";`
这里将密码设置成了 root1

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202311141036382.png)

6. 第六步 创建一个新用户用于远程访问

```
use mysql;
select user,host,plugin from user;
```

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202311141037773.png)

创建新用户：`CREATE USER 'new_user'@'%' IDENTIFIED BY 'passwd';`

例如：`CREATE USER 'icrt'@'%' IDENTIFIED BY 'icrt123!@#';`
用户：icrt 密码：icrt123!@#

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202311141038672.png)
【这里 host 是 % 代表可以任意 ip 访问 plugin 一定是 mysql_native_password 不然客户端连接不了】

7. 为用户赋予权限
   给用户赋权限 操作数据库的权限，这里赋的是全部的权限：`GRANT ALL ON *.* TO 'new_user'@'%';`

例如：`GRANT ALL ON *.* TO 'icrt'@'%';`

`GRANT SELECT ON test.* TO 'icrt'@'%'; // 给用户icrt赋予test数据库的只读权限`

grant 普通数据用户，查询、插入、更新、删除 数据库中所有表数据的权利

```
grant select on testdb.* to common_user@'%';
grant insert on testdb.* to common_user@'%';
grant update on testdb.* to common_user@'%';
grant delete on testdb.* to common_user@'%';
```

8. 🌟 刷新权限【重要，否则不生效】 flush privileges;

可以通过 Navicat / Dbeaver 远程连接
Url: `jdbc:mysql://服务器IP:3306/`

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202311141039233.png)
本地程序更换 datasource 的 url 为上述 ur l 即可

**本地库表上传至服务器 Mysql 数据库**

方法一： 使用 Navicat 的数据传输

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202311141040913.png)

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202311141040652.png)
可能的报错，ERROR 1153 传输的文件内容太大
解决方法：

在终端中使用 MySQL 的 root 用户登录 MySQL;（本地和服务器的 mysql 都要配置以下的内容）：

```
set global max_allowed_packet=1000000000;
set global net_buffer_length=1000000;
FLUSH PRIVILEGES;
```

方法二： 本地导出 SQL 文件，复制到服务器导入
