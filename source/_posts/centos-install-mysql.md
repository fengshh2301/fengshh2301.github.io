---
title: Centos7安装MySQL
date: 2018-08-26 15:29:54
tags: [centos,安装,mysql]
category: [贴士]
---

在 CentOS 上安装 MySQL 有四个步骤。<!-- more -->

## 添加 MySQL YUM 源

根据自己的操作系统选择合适的[安装源](https://link.jianshu.com?t=http://dev.mysql.com/downloads/repo/yum/)，直接获取[下载的地址](https://link.jianshu.com?t=https://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm)，下载后通过 `rpm -Uvh` 安装。

```bash
[jason]$ wget 'https://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm'
[jason]$ sudo rpm -Uvh mysql57-community-release-el7-11.noarch.rpm
[jason]$ yum repolist all | grep mysql
mysql-cluster-7.5-community/x86_64 MySQL Cluster 7.5 Community   disabled
mysql-cluster-7.5-community-source MySQL Cluster 7.5 Community - disabled
mysql-cluster-7.6-community/x86_64 MySQL Cluster 7.6 Community   disabled
mysql-cluster-7.6-community-source MySQL Cluster 7.6 Community - disabled
mysql-connectors-community/x86_64  MySQL Connectors Community    enabled:     63
mysql-connectors-community-source  MySQL Connectors Community -  disabled
mysql-tools-community/x86_64       MySQL Tools Community         enabled:     69
mysql-tools-community-source       MySQL Tools Community - Sourc disabled
mysql-tools-preview/x86_64         MySQL Tools Preview           disabled
mysql-tools-preview-source         MySQL Tools Preview - Source  disabled
mysql55-community/x86_64           MySQL 5.5 Community Server    disabled
mysql55-community-source           MySQL 5.5 Community Server -  disabled
mysql56-community/x86_64           MySQL 5.6 Community Server    disabled
mysql56-community-source           MySQL 5.6 Community Server -  disabled
mysql57-community/x86_64           MySQL 5.7 Community Server    enabled:    287
mysql57-community-source           MySQL 5.7 Community Server -  disabled
mysql80-community/x86_64           MySQL 8.0 Community Server    disabled
mysql80-community-source           MySQL 8.0 Community Server -  disabled
```

先解释下为什么下载的是 5.7 版本的，现在最新的是 5.7 版本的，当然官网默认都是最新版本的，但是下载的页面也有说明

> The MySQL Yum repository includes the latest versions of:
> MySQL 8.0 (Development)
> MySQL 5.7 (GA)
> MySQL 5.6 (GA)
> MySQL 5.5 (GA - Red Hat Enterprise Linux and Oracle Linux Only)
> MySQL Cluster 7.5 (GA)
> MySQL Cluster 7.6 (Development)
> MySQL Workbench
> MySQL Fabric
> MySQL Router (GA)
> MySQL Utilities
> MySQL Connector / ODBC
> MySQL Connector / Python
> MySQL Shell (GA)

也就是说这个安装源包含了上面列举的这些版本，当然包括 5.6 版本的。

## 选择安装版本

如果想安装最新版本的，直接使用 yum 命令即可

```bash
[jason]$sudo yum install mysql-community-server
```

如果想要安装 5.6 版本的，有2个方法。命令行支持 `yum-config-manager` 命令的话，可以使用如下命令：

```bash
[jason]$ sudo dnf config-manager --disable mysql57-community
[jason]$ sudo dnf config-manager --enable mysql56-community
[jason]$ yum repolist | grep mysql
mysql-connectors-community/x86_64 MySQL Connectors Community                  36
mysql-tools-community/x86_64      MySQL Tools Community                       47
mysql56-community/x86_64          MySQL 5.6 Community Server                 327
```

或者直接修改 `/etc/yum.repos.d/mysql-community.repo` 这个文件

```bash
# Enable to use MySQL 5.6
[mysql56-community]
name=MySQL 5.6 Community Server
baseurl=http://repo.mysql.com/yum/mysql-5.6-community/el/7/$basearch/
enabled=1 #表示当前版本是安装
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-mysql

[mysql57-community]
name=MySQL 5.7 Community Server
baseurl=http://repo.mysql.com/yum/mysql-5.7-community/el/7/$basearch/
enabled=0 #默认这个是 1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-mysql
```

通过设置 `enabled` 来决定安装哪个版本。

设置好之后使用 `yum` 安装即可。

## 启动 MySQL 服务

启动命令很简单

```bash
[jason]$sudo service mysqld start 
[jason]$sudo systemctl start mysqld #CentOS 7
[jason]$sudo systemctl status mysqld
● mysqld.service - MySQL Community Server
   Loaded: loaded (/usr/lib/systemd/system/mysqld.service; enabled; vendor preset: disabled)
   Active: active (running) since Sat 2017-05-27 12:56:26 CST; 15s ago
  Process: 2482 ExecStartPost=/usr/bin/mysql-systemd-start post (code=exited, status=0/SUCCESS)
  Process: 2421 ExecStartPre=/usr/bin/mysql-systemd-start pre (code=exited, status=0/SUCCESS)
 Main PID: 2481 (mysqld_safe)
   CGroup: /system.slice/mysqld.service
           ├─2481 /bin/sh /usr/bin/mysqld_safe --basedir=/usr
           └─2647 /usr/sbin/mysqld --basedir=/usr --datadir=/var/lib/mysql --plugin-dir=/usr/...
```

说明已经正在运行中了。

对于 MySQL 5.7 版本，启动的时候如果数据为空的，则会出现如下提示

> The server is initialized.
> An SSL certificate and key files are generated in the data directory.
> The validate_password plugin is installed and enabled.
> A superuser account 'root'@'localhost' is created. A password for the superuser is set and stored in the error log [file.To](https://link.jianshu.com?t=http://file.To) reveal it, use the following command:
> `sudo grep 'temporary password' /var/log/mysqld.log`

简单的说就是服务安装好了，SSL 认证的文件会在 data 目录中生存，密码不要设置的太简单了，初始密码通过下面的命令查看，赶紧去改密码吧。
安装提示，查看密码，登录数据库，然后修改密码：

```bash
[jason]$ mysql -uroot -p  #输入查看到的密码
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass4!';
```

## MySQL 5.6 的安全设置

由于 5.7 版本在安装的时候就设置好了，不需要额外设置，但是 5.6 版本建议从安全角度完善下，运行官方脚本即可

```bash
[jason]$ mysql_secure_installation
```

会提示设置5个关键位置

1. 设置 root 密码
2. 禁止 root 账号远程登录
3. 禁止匿名账号（anonymous）登录
4. 删除测试库
5. 是否确认修改

## 安装第三方组件

查看 yum 源中有哪些默认的组件：

```bash
[jason]$ yum --disablerepo=\* --enablerepo='mysql*-community*' list available
```

需要安装直接通过 `yum` 命令安装即可。

## 修改编码

在 `/etc/my.cnf` 中设置默认的编码

```bash
[client]
default-character-set = utf8

[mysqld]
default-storage-engine = INNODB
character-set-server = utf8
collation-server = utf8_general_ci #不区分大小写
collation-server =  utf8_bin #区分大小写
collation-server = utf8_unicode_ci #比 utf8_general_ci 更准确
```

## 创建数据库和用户

创建数据库

```sql
CREATE DATABASE <datebasename> CHARACTER SET utf8;
CREATE USER 'username'@'host' IDENTIFIED BY 'password';
GRANT privileges ON databasename.tablename TO 'username'@'host';
SHOW GRANTS FOR 'username'@'host';
REVOKE privilege ON databasename.tablename FROM 'username'@'host';
DROP USER 'username'@'host';
```

其中

- username：你将创建的用户名
- host：指定该用户在哪个主机上可以登陆，如果是本地用户可用 localhost，如果想让该用户可以从任意远程主机登陆，可以使用通配符 %
- password：该用户的登陆密码，密码可以为空，如果为空则该用户可以不需要密码登陆服务器
- privileges：用户的操作权限，如 SELECT，INSERT，UPDATE 等，如果要授予所的权限则使用ALL
- databasename：数据库名
- tablename：表名，如果要授予该用户对所有数据库和表的相应操作权限则可用 * 表示，如 *.*

## Django 中使用 MySQL

Django 默认使用的是 sqlite3 数据库，可以修改如下

```bash
DATABASES = {
    'default': {
        # 'ENGINE': 'django.db.backends.sqlite3',
        # 'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
        'ENGINE': 'django.db.backends.mysql',
         'OPTIONS': {
            'read_default_file': '/path/to/my.cnf',
            'init_command': "SET sql_mode='STRICT_TRANS_TABLES'" ,
            'isolation_level':'read committed',
            'init_command': 'SET default_storage_engine=INNODB',
        },
        'NAME': 'mydatabase',
        'USER': 'mydatabaseuser',
        'PASSWORD': 'mypassword',
        'HOST': '127.0.0.1',
        'PORT': '3306',
    }
}

# my.cnf
[client]
database = 'mydatabase'
user = 'mydatabaseuser'
password = 'mypassword'
default-character-set = utf8
```

重点关注下 `OPTIONS` 选项，它会优先与下面的配置项。在 [Django 官方文档](https://link.jianshu.com?t=https://docs.djangoproject.com/en/1.11/ref/databases/#collation-settings)说明中，主要强调了 `init_command`, `isolation_level` 这两个选项的设置。

### init_command

为了防止数据的丢失，mysql 5.7 和新装的 5.6 版本都新增加了一个模式 `STRICT_TRANS_TABLES`，在这个模式下，如果插入数据中断就会报错，而不是仅仅之前的警告。之前的模式默认为 `NO_ENGINE_SUBSTITUTION`. 所以也建议设置为 `STRICT_TRANS_TABLES` 或者 `STRICT_ALL_TABLES`. 在选项中设置或者在数据库中设置都可以的。

### isolation_level

这是在 Django 1.11 版本中新增加的一个选择，当运行并发负载时，来自不同会话的数据库事务（例如，处理不同请求的单独线程）可能会相互交互，这些交互受每个会话的事务隔离级别的影响。默认有 5 个隔离级别，默认为 `REPEATABLE-READ`

```sql
mysql> SELECT @@global.tx_isolation;
+-----------------------+
| @@global.tx_isolation |
+-----------------------+
| REPEATABLE-READ       |
+-----------------------+
1 row in set (0.00 sec)


-  read uncommitted 
# SELECT的时候允许脏读，即SELECT会读取其他事务修改而还没有提交的数据。
-  read committed
# SELECT的时候无法重复读，即同一个事务中两次执行同样的查询语句，若在第一次与第二次查询之间时间段，其他事务又刚好修改了其查询的数据且提交了，则两次读到的数据不一致。
-  repeatable read
# SELECT的时候可以重复读，即同一个事务中两次执行同样的查询语句，得到的数据始终都是一致的。实现的原理是，在一个事务对数据行执行读取或写入操作时锁定了这些数据行。但是这种方式又引发了幻想读的问题。因为只能锁定读取或写入的行，不能阻止另一个事务插入数据，后期执行同样的查询会产生更多的结果。数据库默认的级别。
-  serializable
# 与可重复读的唯一区别是，默认把普通的SELECT语句改成SELECT …. LOCK IN SHARE MODE。即为查询语句涉及到的数据加上共享琐，阻塞其他事务修改真实数据。serializable模式中，事务被强制为依次执行。
-  None
# 无隔离级别

```

### MySQL 修改时间戳为服务器时间

mysql 中默认的时间戳是 UTC 时间，需要改为服务器时间的话官网提供了 3 种方式

```bash
[jason]$ mysql_tzinfo_to_sql tz_dir
[jason]$ mysql_tzinfo_to_sql tz_file tz_name
[jason]$ mysql_tzinfo_to_sql --leap tz_file
```

tz_dir 代表服务器时间数据库，CentOS 7 中默认的目录为 `/usr/share/zoneinfo` ，tz_name 为具体的时区。如果设置的时区需要闰秒，则使用 `--leap`，具体的用法如下：

```bash
[jason]$ mysql_tzinfo_to_sql /usr/share/zoneinfo | mysql -u root -p mysql
[jason]$ mysql_tzinfo_to_sql tz_file tz_name | mysql -u root mysql
[jason]$ mysql_tzinfo_to_sql --leap tz_file | mysql -u root mysql
```