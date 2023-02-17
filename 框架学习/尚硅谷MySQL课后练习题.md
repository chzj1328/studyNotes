# MySQL下载与安装

## 1.1、MySQL的4大版本

> - MySQL Community Server 社区版本，开源免费，自由下载
> - MySQL Community Server 企业版本，付费，可免费试用30天，不能在线下载
> - MySQL Cluster 集群版 开源免费。用于架设服务器
> - MySQL Cluster CGE  高集群版

- 目前最新版为`8.0.28`

此外，官方还提供了`MySQL Workbench`(GUITOOL)一款专门为MySQL设计的`图形界面管理工具`。

分为社区版和商用版

## 1.2、软件的下载

**1、下载地址**

官网：[https://www.mysql.com/](https://www.mysql.com/)

**2、打开官网 点击DOWNLOADS**

然后，点击`MySQL Community (GPL) Downloads `

![21](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/21.png)

点击`MySQL Community Server`

![1650292971879](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/1650292971879.png)



点击`(mysql-installer-community-8.0.28.0.msi) `Doenload

![4cd40efd17a642b98c3d7385958e8f42](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/4cd40efd17a642b98c3d7385958e8f42.png)

**下载历史版本点击Archives**

![9478346edb434a10a3dbf62271dd5774](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/9478346edb434a10a3dbf62271dd5774.png)

![825727d748394e9691de08a495b00a75](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/825727d748394e9691de08a495b00a75.png)

## 1.3、下载完成后，开始安装

**1、点击下载好的MSI**

![7 (2)](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/7%20(2).png)

**选择自定义安装，点击【next】**

![8](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/8.png)

**2、点击【next】，把8.0.28放到右边的框框内**

![9](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/9.png)

**选中MySQL Server8.0.28-x64可以修改安装路径**

![10](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/10.png)

**3、点击【next】，再点击【Execute】**

![9](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/91.png)

**4、等待下载完成，完成后点击【next】**

![11](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/11.png)

![12](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/12.png)

**点击【next】**

![13](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/13.png)

**配置端口号，点击【next】**

![14](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/14.png)

**选择加密方法第一个Strong，点击【next】**

![15](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/15.png)

**设置root账户的密码，在生产环境时需要设置强密码，学习时可以设置简单的，点击【next】**

![16](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/16.png)

**设置MySQL的服务名称【next】**

![17](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/17.png)

**等全部加载完毕后点击【Finish】**

![18](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/18.png)

**点击【next】** 

![19](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/19.png)

**点击【Finish】**

![20](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/20.png)

# MySQL的卸载

## 1、查看当前mysql安装状况

```shell
rpm -qa | grep mysql

# 或

yum list install | grep mysql
```

![image-20221007230130000](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221007230130000.png)

## 2、查看mysql的服务是否启动

```shell
systemctl status mysqld
```

![image-20221007230223269](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221007230223269.png)

### 2.1、如果启动则关闭mysql服务

```shell
systemctl stop mysqld.service  # service可加可不加
# 停止MySQL的服务
[root@chenstudy ~]\# systemctl stop mysqld
# 再次查看MySQL的服务状态
[root@chenstudy ~]\# systemctl status mysqld
```

![](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221007171744754.png)

## 3、 卸载上述命令查询出的已安装程序

- yum remove mysql-community-libs-8.0.30-1.el7.x86_64
- rpm -qa | grep mysql
- yum remove mysql-community-client-plugins-8.0.30-1.el7.x86_64
- rpm -qa | grep mysql
- yum remove mysql-community-server-8.0.30-1.el7.x86_64
- rpm -qa | grep mysql

```shell
[root@chenstudy ~]# yum remove mysql-community-libs-8.0.30-1.el7.x86_64
[root@chenstudy ~]# rpm -qa | grep mysql
mysql-community-client-plugins-8.0.30-1.el7.x86_64
mysql80-community-release-el7-7.noarch
mysql-community-common-8.0.30-1.el7.x86_64
mysql-community-icu-data-files-8.0.30-1.el7.x86_64
[root@chenstudy ~]# yum remove mysql-community-client-plugins-8.0.30-1.el7.x86_64
[root@chenstudy ~]# rpm -qa | grep mysql
mysql80-community-release-el7-7.noarch
mysql-community-common-8.0.30-1.el7.x86_64
mysql-community-icu-data-files-8.0.30-1.el7.x86_64
[root@chenstudy ~]# yum remove mysql-community-common-8.0.30-1.el7.x86_64
[root@chenstudy ~]# rpm -qa | grep mysql
mysql80-community-release-el7-7.noarch
mysql-community-icu-data-files-8.0.30-1.el7.x86_64
[root@chenstudy ~]# yum remove mysql-community-icu-data-files-8.0.30-1.el7.x86_64
[root@chenstudy ~]# rpm -qa | grep mysql
mysql80-community-release-el7-7.noarch
[root@chenstudy ~]# yum remove mysql80-community-release-el7-7.noarch
```



![图一](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221007172415804.png)

​                                                              **图一：移除mysql-community-libs-8.0.30-1.el7.x86_64**

![图二](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221007172745414.png)

​                                                                           **图二：查看剩余的MySQL安装包**

![图三](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221007172930224.png)

​                                                                  **图三：移除mysql-community-client-plugins**

![image-20221007174136533](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221007174136533.png)

​                                                                              **图四：再次查看MySQL的安装状态**

![](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221007174935299.png)

​                                                                     **图五：移除 mysql-community-icu-data-files**

![图七](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221007175350300.png)

​                                                            **图六：移除mysql80-community-release-el7-7.noarch**

## 4、删除mysql相关文件

- 查找相关文件

```shell
find / -name mysql
```

![image-20221007175553701](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221007175553701.png)

- 删除上述命令查找出来的相关文件

```shell
rm -rf /usr/lib64/mysql
rm -rf /var/lib/mysql
rm -rf /var/lib/mysql/mysql
rm -rf /etc/selinux/targeted/active/modules/100/mysql
```



```shell
[root@chenstudy ~]# find / -name mysql
/usr/local/mysql
/usr/lib64/mysql
/var/lib/mysql
/var/lib/mysql/mysql
/etc/selinux/targeted/active/modules/100/mysql
[root@chenstudy ~]# rm -rf /usr/lib64/mysql
[root@chenstudy ~]# rm -rf /var/lib/mysql
[root@chenstudy ~]# rm -rf /var/lib/mysql/mysql
[root@chenstudy ~]# rm -rf /etc/selinux/targeted/active/modules/100/mysql
```



## 5、删除my.cnf

```shell
rm -rf /etc/my.cnf
```

![image-20221007175806758](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221007175806758.png)

```shell
[root@chenstudy ~]# rm -rf /etc/my.cnf
[root@chenstudy ~]# 
```



# MySQL5.7的安装

## 1、新建文件夹

```shell
mkdir /opt/mysql
# 进入到文件夹中
cd /opt/mysql
```

## 2、上传下载的文件到/opt/mysql

```shell
# 通过xftp7进行文件的上传
```

## 3、解压刚上传的文件

```shel
tar -xvf mysql-5.7.37-1.el7.x86_64.rpm-bundle.tar 
```

## 4、查询mariadb相关安装包

```shell
rpm -qa | grep mari
```

## 5、开始安装MySQL

**依次执行如下命令：**

```shell
rpm -ivh mysql-community-common-5.7.37-1.el7.x86_64.rpm 
rpm -ivh mysql-community-libs-5.7.37-1.el7.x86_64.rpm 
rpm -ivh mysql-community-client-5.7.37-1.el7.x86_64.rpm
rpm -ivh mysql-community-server-5.7.37-1.el7.x86_64.rpm 
```

## 6、启动MySQL

```shell
systemctl start mysqld.service
```

## 7、设置root用户密码

- 查看初始密码

```shell
cat /var/log/mysqld.log
# 或
grep "password" /var/log/mysqld.log
```

## 8、进入到MySQL并设置root密码

```sql
mysql -uroot -p*****

# 设置root密码
set password for 'root'@'localhost' = password('ZJch1328.');
# 刷新权限
flush privileges;
```



# MySQL笔记

## 字符集的修改与底层原理的说明

### 字符集

```sql
# 查看默认的字符集
show variables like '%character%';
# 修改字符集
# 1、进入到my.cnf文件
vim /etc/my.cnf
# 2、在文件最后添加上character_set_server=utf8
character_set_server=utf8
# 重启mysql的服务
systemctl restart mysqld.service

# 修改已有数据库的字符集
alter database dbtest1 character set 'utf8';

# 修改已有表的字符集
alter table emp1 convert to character set 'utf8';
```

### 各级别的字符集

MySQL有4个级别的字符集和比较规则：

- 服务器级别
- 数据库级别
- 表级别
- 列级别

### MySQL目录结构与表在文件系统中的展示

+ 数据库文件存放的路径 
  - /var/lib/mysql
+ 相关命令的目录
  -  /usr/bin
  -  /usr/sbin

- 相关配置文件的路径
  - /usr/share/mysql-8.0
  - /etc/my.cnf

### 第03章_用户与权限管理

#### 1.用户管理

##### 1.1 登录MySQL服务器

启动MySQL服务后，可以通过mysql命令来登录MySQL服务器，命令如下：

```shell
mysql –h hostname|hostIP –P port –u username –p DatabaseName –e "SQL语句"
```

下面详细介绍命令中的参数：

- h参数后面接主机名或者主机IP，hostname为主机，hostIP为主机IP。
- P参数后面接MySQL服务的端口，通过该参数连接到指定的端口。MySQL服务的默认端口是3306，
  不使用该参数时自动连接到3306端口，port为连接的端口号。
- u参数后面接用户名，username为用户名。
- p参数会提示输入密码。
- DatabaseName参数指明登录到哪一个数据库中。如果没有该参数，就会直接登录到MySQL数据库
  中，然后可以使用USE命令来选择数据库。
- e参数后面可以直接加SQL语句。登录MySQL服务器以后即可执行这个SQL语句，然后退出MySQL
  服务器。

##### 1.2 创建用户

CREATE USER语句的基本语法形式如下：

```sql
CREATE USER 用户名 [IDENTIFIED BY '密码'][,用户名 [IDENTIFIED BY '密码']];``
```

- 用户名参数表示新建用户的账户，由用户（User） 和主机名（Host） 构成；
- “[ ]”表示可选，也就是说，可以指定用户登录时需要密码验证，也可以不指定密码验证，这样用户
  可以直接登录。不过，不指定密码的方式不安全，不推荐使用。如果指定密码值，这里需要使用
  IDENTIFIED BY指定明文密码值。
- CREATE USER语句可以同时创建多个用户。

举例：

```sql
CREATE USER zhang IDENTIFIED BY '123123'; # 默认host是%
SELECT host,user FROM user;
# 查看权限
SHOW GRANTS;
```

```sql
CREATE USER 'chen'@'localhost' IDENTIFIED BY '123456';
```

##### 1.3修改用户

修改用户名：

```sql
UPDATE mysql.user SET USER = 'li4' WHERE USER = 'zhang';

FLUSH PRIVILEGES;
```

##### 1.4删除用户

使用DROP USER语句来删除用户时，必须用于DROP USER权限。DROP USER语句的基本语法形式如下：

```sql
DROP USER user[,user]…;
```

举例：

```sql
DROP USER li4 ; # 默认删除host为%的用户
```

```sql
DROP USER 'kangshifu'@'localhost';
```

##### 1.5、设置当前用户密码和修改其他用户的密码

- 使用ALTER语句来修改当前用户的密码

```sql
ALTER user user() IDETIFIED BY 'new password';
```

- 使用ALTER语句来修改指定用户的密码

```sql
ALTER user 'zhang3'@'%' IDENTIFIED BY 'new password';
```

- 使用SET语句来修改当前用户的地址

```sql
SET PASSWORD = 'new password'
```

- 使用SET语句来修改指定用户的密码

```sql
SET PASSWORD FOR 'username'@'hostname' = 'new password';
```

##### 1.6、密码过期策略

- 使用SQL语句更改该变量的值并持久化

```sql
SET PERSIST default_password_lifetime = 180; # 建立全局策略，设置密码每隔180天过期
```

- 配置文件my.cnf中进行维护

```properties
[mysqld]
default_password_lifetime = 180;   # 建立全局策略，设置密码每隔180天过期
```

- 单独设置密码过期时间

```mysql
# 设置用户账号密码每90天过期
CREATE USER 'username'@'hostname' PASSWORD EXPIRE INTERVAL 90 DAY;
ALTER USER 'username'@'hostname' PASSWORD EXPIRE INTERVAL 90 DAY;

# 设置密码永不过期
CREATE USER 'username'@'hostname' PASSWORD EXPIRE INTERVAL NEVER;
ALTER USER 'username'@'hostname' PASSWORD EXPIRE INTERVAL NEVER;

# 延用全局密码过期策略
CREATE USER 'username'@'hostname' PASSWORD EXPIRE INTERVAL DEFAULT;
ALTER USER 'username'@'hostname' PASSWORD EXPIRE INTERVAL DEFAULT;
```

### 2、权限管理

#### 2.1、权限列表

mysql有哪些权限呢？

```mysql
show privileges;
```

（1） `CREATE和DROP权限`，可以创建新的数据库和表，或删除（移掉）已有的数据库和表。如果将MySQL数据库中的DROP权限授予某用户，用户就可以删除MySQL访问权限保存的数据库。 

（2）`SELECT、INSERT、UPDATE和DELETE权限`允许在一个数据库现有的表上实施操作。 

（3） `SELECT权限`只有在它们真正从一个表中检索行时才被用到。

 （4）` INDEX`权限允许创建或删除索引，INDEX适用于已有的表。如果具有某个表的CREATE权限，就可以在CREATE TABLE语句中包括索引定义。 

（5） `ALTER权限`可以使用ALTER TABLE来更改表的结构和重新命名表。 

（6） `CREATE ROUTINE权限`用来创建保存的程序（函数和程序），`ALTER ROUTINE权限`用来更改和删除保存的程序，` EXECUTE权限`用来执行保存的程序。 

（7） `GRANT权限`允许授权给其他用户，可用于数据库、表和保存的程序。 

（8）` FILE权限`使用户可以使用LOAD DATA INFILE和SELECT ... INTO OUTFILE语句读或写服务器上的文件，任何被授予FILE权
限的用户都能读或写MySQL服务器上的任何文件（说明用户可以读任何数据库目录下的文件，因为服务器可以访问这些文件）。

#### 2.2、授予权限的原则

权限控制主要是出于安全因素，因此需要遵循以下几个`经验原则`：
	1、只授予能满足`需要的最小权限`，防止用户干坏事。比如用户只是需要查询，那就只给select权限就可以了，不要给用户赋予update、insert或者delete权限。
	2、创建用户的时候`限制用户的登录主机`，一般是限制成指定IP或者内网IP段。
	3、为每个用户`设置满足密码复杂度的密码`。
	4、`定期清理不需要的用户`，回收权限或者删除用户。

#### 2.3、 授予权限

给用户授权的方式有 2 种，分别是通过把`角色赋予用户给用户授权`和`直接给用户授权`。

授权命令：

```mysql
GRANT 权限1,权限2,…权限n ON 数据库名称.表名称 TO 用户名@用户地址 [IDENTIFIED BY ‘密码口令’];
```

- 该权限如果发现没有该用户，则会直接新建一个用户。

# MySQL高级特性

## 第03章_用户与权限管理

### 1.用户管理

MySQL用户可以分为==普通用户==和==root用户==。root用户是超级管理员，拥有所有权限，包括创建用户、删除用户和修改用户的密码等管理权限；普通用户只拥有被授予的各种权限。

**MySQL提供了许多语句来管理用户账号**，这些语句可以用来管理包括登录和退出MySQL服务器、创建用户、删除用户、密码管理和权限管理等。

#### 1.1 登录MySQL服务器 

启动MySQL服务后，可以通过mysql命令来登录MySQL服务器，命令如下： 

```shell
mysql -h hostname|hostIp -P port -u username -p DatabaseName -e "SQL语句"
```

下面详细介绍命令中的参数： 

- ==-h参数== 后面接主机名或者主机IP，hostname为主机，hostIP为主机IP。 
- ==-P参数== 后面接MySQL服务的端口，通过该参数连接到指定的端口。MySQL服务的默认端口是3306， 不使用该参数时自动连接到3306端口，port为连接的端口号。 
- ==-u参数== 后面接用户名，username为用户名。 
- ==-p参数== 会提示输入密码。 DatabaseName参数 指明登录到哪一个数据库中。如果没有该参数，就会直接登录到MySQL数据库 中，然后可以使用USE命令来选择数据库。
- ==-e参数== 后面可以直接加SQL语句。登录MySQL服务器以后即可执行这个SQL语句，然后退出MySQL 服务器。

#### 1.2创建用户

在MySQL数据库中，官方推荐使用`CREATE USER`语句创建新用户。

使用CREATE USER语句来创建新用户时，==必须拥有CREATE USER权限==。每添加一个用户，CREATE USER语句会在MySQL.user表中添加一条新纪录，但没有任何权限。

CREATE USER语句的基本语法形式如下： 

```shell
CREATE USER 用户名 [IDENTIFIED BY '密码'][,用户名 [IDENTIFIED BY '密码']];
```

- 用户名参数表示新建用户的账户，由 用户（User） 和 主机名（Host） 构成；
- “[ ]”表示可选，也就是说，可以指定用户登录时需要密码验证，也可以不指定密码验证，这样用户 可以直接登录。不过，不指定密码的方式不安全，不推荐使用。如果指定密码值，这里需要使用 IDENTIFIED BY指定明文密码值。 
- CREATE USER语句可以同时创建多个用户。

举例：

```shell
CREATE USER chen IDENTIFIED BY 'Abc123..'; 		# 默认host是%
```

```shell
CREATE USER 'chen'@'localhost' IDENTIFIED BY 'Abc123..';
```

#### 1.3 修改用户

修改用户名：

```shell
UPDATE mysql.user SET USER='li4' WHERE USER='chen';

# 刷新权限
FLUSH PRIVILEGES;
```

#### 1.4删除用户

**方式一：使用DROP方式删除（推荐）**

使用DROP USER语句来删除用户时，必须用于DROP USER权限。DROP USER语句的基本语法形式如下：

```shell
DROP USER user[,user]…;
```

举例： 

```shell
DROP USER li4 ; # 默认删除host为%的用户

DROP USER 'kangshifu'@'localhost';
```

**方式2：使用DELETE方式删除** 

```shell
DELETE FROM mysql.user WHERE Host=’hostname’ AND User=’username’;
```

执行完DELETE命令后要使用FLUSH命令来使用户生效，命令如下：

```shell
FLUSH PRIVILEGES;
```

举例： 

```shell
DELETE FROM mysql.user WHERE Host='localhost' AND User='Emily';
FLUSH PRIVILEGES;
```

>  注意：不推荐通过 DELETE FROM USER u WHERE USER='li4' 进行删除，系统会有残留信息保 留。而drop user命令会删除用户以及对应的权限，执行命令后你会发现mysql.user表和mysql.db表 的相应记录都消失了。

#### 1.5 设置当前用户的密码

适用于root用户修改自己的密码，以及普通用户登陆后修改自己的密码。

root用户拥有很高的权限，必须保证root用户的密码安全，使用`ALTER USER `修改用户密码是MySQL`官方推荐`的方式。

**旧写法如下：**

```shell
# 修改当前用户的密码：（MySQL5.7测试有效）
SET PASSWORD = PASSWORD('123456');
```

这里介绍 推荐的写法 ： 

1. 使用ALTER USER命令来修改当前用户密码 用户可以使用ALTER命令来修改自身密码，如下语句代表修 改当前登录用户的密码。基本语法如下： 

   ```shell
   ALTER USER USER() IDENTIFIED BY 'NEW PASSWORD';
   ```

2. 使用SET语句来修改当前用户密码 使用root用户登录MySQL后，可以使用SET语句来修改密码，具体 SQL语句如下：

   ```shell
   SET PASSWORD='new_password';
   ```

该语句会自动将密码加密后再赋给当前用户。

#### 1.6 修改其它用户密码 

**使用ALTER语句来修改普通用户的密码** 

可以使用ALTER USER语句来修改普通用户的密码。基本语法形 式如下： 

```shell
ALTER USER user [IDENTIFIED BY 'new_password']
[,user [IDENTIFIED BY 'new_password']]...;
```

举例：

```shell
ALTER USER 'chen'@'localhost' IDENTIFIED BY 'Hello123.'
```

**使用SET命令来修改普通用户的密码** 

使用root用户登录到MySQL服务器后，可以使用SET语句来修改普 通用户的密码。SET语句的代码如下： 

```shell
SET PASSWORD FOR 'username'@'hostname'='new_password';
```

**使用UPDATE语句修改普通用户的密码（不推荐）**

```shell
UPDATE MySQL.user SET authentication_string=PASSWORD("123456")
WHERE User = "username" AND Host = "hostname";
```

### 2.权限管理

#### 2.1权限列表

MySQL到底有哪些权限？

```shell
SHOW PRIVILEGES;
```

（1）**CREATE和DROP权限** ，可以创建新的数据库和表，或删除（移掉）已有的数据库和表。如果将 MySQL数据库中的DROP权限授予某用户，用户就可以删除MySQL访问权限保存的数据库。 

（2） **SELECT、INSERT、UPDATE和DELETE权限** 允许在一个数据库现有的表上实施操作。 

（3） **SELECT权限** 只有在它们真正从一个表中检索行时才被用到。 

（4） **INDEX权限** 允许创建或删除索引，INDEX适用于已 有的表。如果具有某个表的CREATE权限，就可以在CREATE TABLE语句中包括索引定义。 

（5） **ALTER权 限** 可以使用ALTER TABLE来更改表的结构和重新命名表。

（6） **CREATE ROUTINE权限** 用来创建保存的 程序（函数和程序），ALTER ROUTINE权限用来更改和删除保存的程序， EXECUTE权限 用来执行保存的 程序。 

（7） **GRANT权限** 允许授权给其他用户，可用于数据库、表和保存的程序。 

（8） **FILE权限** 使用 户可以使用LOAD DATA INFILE和SELECT ... INTO OUTFILE语句读或写服务器上的文件，任何被授予FILE权 限的用户都能读或写MySQL服务器上的任何文件（说明用户可以读任何数据库目录下的文件，因为服务 器可以访问这些文件）。

#### 2.2授予权限的原则

权限控制主要是出于安全因素，因此需要遵循以下几个 ==经验原则== ： 

1、只授予能 ==满足需要的最小权限== ，防止用户干坏事。比如用户只是需要查询，那就只给select权限就可 以了，不要给用户赋予update、insert或者delete权限。 

2、创建用户的时候 ==限制用户的登录主机== ，一般是限制成指定IP或者内网IP段。 

3、为每个用户 ===设置满足密码复杂度的密码== 。 

4、 ==定期清理不需要的用户== ，回收权限或者删除用户。

#### 2.3授予权限

给用户授权方式有两种。分别是通过把`角色赋予用户给用户授权`和`直接给用户授权`。用户是数据库的 使用者，我们可以通过给用户授予访问数据库中资源的权限，来控制使用者对数据库的访问，消除安全 隐患。

授权命令：

```shell
GRANT 权限1,权限2,…权限n ON 数据库名称.表名称 TO 用户名@用户地址 [IDENTIFIED BY ‘密码口令’];
```

- 该权限如果发现没有该用户，则会直接新建一个用户。 

比如： 

- 给li4用户用本地命令行方式，授予atguigudb这个库下的所有表的插删改查的权限。

```shell
GRANT SELECT,INSERT,DELETE,UPDATE ON atguigudb.* TO li4@localhost ;
```

- 授予通过网络方式登录的joe用户 ，对所有库所有表的全部权限，密码设为123。注意这里唯独不包 括grant的权限

```shell
GRANT ALL PRIVILEGES ON *.* TO joe@'%' IDENTIFIED BY '123';
```

> 我们在开发应用的时候，经常会遇到一种需求，就是要根据用户的不同，对数据进行横向和纵向的 分组。 
>
> - 所谓横向的分组，就是指用户可以接触到的数据的范围，比如可以看到哪些表的数据； 
> - 所谓纵向的分组，就是指用户对接触到的数据能访问到什么程度，比如能看、能改，甚至是 删除。

#### 2.4查看权限

- 查看当前用户权限

```shell
SHOW GRANTS;
# 或
SHOW GRANTS FOR CURRENT_USER;
# 或
SHOW GRANTS FOR CURRENT_USER();
```

- 查看某用户的全局权限

```shell
SHOW GRANTS FOR 'user'@'主机地址';
```

#### 2.5 收回权限 

收回权限就是取消已经赋予用户的某些权限。**收回用户不必要的权限可以在一定程度上保证系统的安全性**。MySQL中使用 ==REVOKE==语句 取消用户的某些权限。使用REVOKE收回权限之后，用户账户的记录将从 db、host、tables_priv和columns_priv表中删除，但是用户账户记录仍然在user表中保存（删除user表中 的账户记录使用DROP USER语句）。 

**注意：在将用户账户从user表删除之前，应该收回相应用户的所有权限。** 

- 收回权限命令 

  ```shell
  REVOKE 权限1,权限2,…权限n ON 数据库名称.表名称 FROM 用户名@用户地址;
  ```

- 举例:

```shell
#收回全库全表的所有权限
REVOKE ALL PRIVILEGES ON *.* FROM joe@'%';

#收回mysql库下的所有表的插删改查权限
REVOKE SELECT,INSERT,UPDATE,DELETE ON mysql.* FROM joe@localhost;
```

- 注意： 须用户重新登录后才能生效 

# 尚硅谷MySQL课后练习题

## 数据库资源地址：链接：https://pan.baidu.com/s/1D1mNUa9bTCcED3SothrgpQ 提取码：1328

## 一、基本的SELECT语句

```sql
【题目】
# 1.查询员工12个月的工资总和，并起别名为ANNUAL SALARY
# 2.查询employees表中去除重复的job_id以后的数据
# 3.查询工资大于12000的员工姓名和工资
# 4.查询员工号为176的员工的姓名和部门号
# 5.显示表 departments 的结构，并查询其中的全部数据
```

### 1.查询员工12个月的工资总和，并起别名为ANNUAL SALARY

```sql
SELECT employee_id , last_name,salary * 12 "ANNUAL SALARY"
FROM employees;

SELECT employee_id,last_name,salary * 12 * (1 + IFNULL(commission_pct,0)) "ANNUAL SALARY"
FROM employees;
```

### 2.查询employees表中去除重复的job_id以后的数据

```sql
SELECT DISTINCT job_id 
FROM employees;
```

### 3.查询工资大于12000的员工姓名和工资

```sql
SELECT last_name,salary
FROM employees
WHERE salary > 12000;
```

### 4.查询员工号为176的员工的姓名和部门号

```sql
SELECT last_name,department_id
FROM employees
WHERE employee_id = 176;
```

### 5.显示表 departments 的结构，并查询其中的全部数据

```sql
DESC departments;

SELECT * FROM departments;
```

## 二、运算符练习

```sql
【题目】
# 1.选择工资不在5000到12000的员工的姓名和工资

# 2.选择在20或50号部门工作的员工姓名和部门号

# 3.选择公司中没有管理者的员工姓名及job_id

# 4.选择公司中有奖金的员工姓名，工资和奖金级别

# 5.选择员工姓名的第三个字母是a的员工姓名

# 6.选择姓名中有字母a和k的员工姓名

# 7.显示出表 employees 表中 first_name 以 'e'结尾的员工信息

# 8.显示出表 employees 部门编号在 80-100 之间的姓名、工种

# 9.显示出表 employees 的 manager_id 是 100,101,110 的员工姓名、工资、管理者id
```

### 1.选择工资不在5000到12000的员工的姓名和工资

```sql
SELECT last_name,salary 
FROM employees 
WHERE salary < 5000 OR salary > 12000;

SELECT last_name,salary 
FROM employees 
WHERE NOT BETWEEN 5000 AND 12000;
```

### 2.选择在20或50号部门工作的员工姓名和部门号

```sql
SELECT last_name,department_id 
FROM employees 
WHERE department_id = 20 OR department_id = 50;

SELECT last_name,department_id 
FROM employees 
WHERE department_id IN(20, 50);
```

### 3.选择公司中没有管理者的员工姓名及job_id

```sql
SELECT last_name,job_id 
FROM employees 
WHERE manager_id <=> NULL;

SELECT last_name,job_id 
FROM employees 
WHERE manager_id IS NULL;
```

### 4.选择公司中有奖金的员工姓名，工资和奖金级别

```sql
SELECT last_name,salary,commission_pct 
FROM employees 
WHERE commission_pct IS NOT NULL;
```

### 5.选择员工姓名的第三个字母是a的员工姓名

```sql
SELECT last_name 
FROM employees 
WHERE last_name 
LIKE '__a%';
```

### 6.选择姓名中有字母a和k的员工姓名

```sql
SELECT last_name 
FROM employees 
WHERE last_name LIKE '%a%k%' OR last_name LIKE '%k%a%';
```

### 7.显示出表 employees 表中 first_name 以 'e'结尾的员工信息

```sql
SELECT employee_id,first_name,last_name 
FROM employees 
WHERE first_name LIKE '%e';
```

### 8.显示出表 employees 部门编号在 80-100 之间的姓名、工种

```sql
SELECT last_name,job_id 
FROM employees 
WHERE department_id IN(80, 100);

SELECT last_name,job_id 
FROM employees 
WHERE department_id BETWEEN 80 AND 100;
```

### 9.显示出表 employees 的 manager_id 是 100,101,110 的员工姓名、工资、管理者id

```sql
SELECT last_name,salary,manager_id
FROM employees
WHERE manager_id = 100 
OR manager_id = 101 
OR manager_id = 110;

SELECT last_name,salary,manager_id
FROM employees
WHERE manager_id IN (100,101,110);
```

## 第05章_排序与分页

### 题目：

```sql
1. 查询员工的姓名和部门号和年薪，按年薪降序 按姓名升序显示

2. 选择工资不在 8000 到 17000 的员工的姓名和工资，按工资降序，显示第21到40位置的数据
   
3. 查询邮箱中包含 e 的员工信息，并先按邮箱的字节数降序，再按部门号升序
```

### 1. 查询员工的姓名和部门号和年薪，按年薪降序 按姓名升序显示

```sql
SELECT last_name,department_id, salary * 12 annual_sal 
FROM employees 
ORDER BY salary * 12 DESC, last_name ASC;
```

### 2. 选择工资不在 8000 到 17000 的员工的姓名和工资，按工资降序，显示第21到40位置的数据

```sql
SELECT last_name,salary 
FROM employees 
WHERE salary NOT BETWEEN 8000 AND 17000
ORDER BY salary ASC
LIMIT 20,20;
```

### 3. 查询邮箱中包含 e 的员工信息，并先按邮箱的字节数降序，再按部门号升序

```sql
SELECT last_name,email,department_id
FROM employees
#WHERE email LIKE '%e%'
WHERE email REGEXP '[e]'
ORDER BY LENGTH(email) DESC,department_id ASC;
```

## 第06章_多表查询课后练习

### 多表查询-1

**【题目】:**

1.显示所有员工的姓名，部门号和部门名称。

2.查询90号部门员工的job_id和90号部门的location_id

3.选择所有有奖金的员工的 last_name , department_name , location_id , city

4.选择city在Toronto工作的员工的 last_name , job_id,department_id,department_name

5.查询员工所在的部门名称、部门地址、姓名、工作、工资，其中员工所在部门的部门名称为’Executive’

6.选择指定员工的姓名，员工号，以及他的管理者的姓名和员工号，结果类似于下面的格式

	employees   Emp		# manager Mgr#
	kochhar 	101 	  king 	  100

7.查询哪些部门没有员工

8. 查询哪个城市没有部门

9. 查询部门名为 Sales 或 IT 的员工信息

### 1.显示所有员工的姓名，部门号和部门名称。

```sql
SELECT last_name,e.department_id,d.department_name 
FROM employees e 
LEFT JOIN departments d 
ON e.department_id = d.department_id;
```

### 2.查询90号部门员工的job_id和90号部门的location_id

```SQL
SELECT e.job_id job_id,d.location_id location_id 
FROM employees e 
JOIN departments d 
ON e.department_id = d.department_id 
WHERE e.department_id = 90;
```

### 3.选择所有有奖金的员工的 last_name , department_name , location_id , city

```sql
SELECT e.last_name,d.department_name,d.location_id,l.city
FROM employees e
LEFT JOIN departments d
ON e.department_id = d.department_id
JOIN locations l
ON d.location_id = l.location_id
WHERE e.commission_pct IS NOT NULL;
```

### 4.选择city在Toronto工作的员工的 last_name , job_id,department_id,department_name

```sql
SELECT e.last_name,e.job_id,d.department_id,d.department_name
FROM employees e
LEFT JOIN departments d
ON e.department_id = d.department_id
LEFT JOIN locations l
ON d.location_id = l.location_id
WHERE l.city = 'Toronto';
```

### 5.查询员工所在的部门名称、部门地址、姓名、工作、工资，其中员工所在部门的部门名称为’Executive’

```sql
SELECT department_name,street_address,last_name,job_id,salary
FROM employees e
LEFT JOIN departments d
ON e.department_id = d.department_id
LEFT JOIN locations l
ON d.location_id = l.location_id
WHERE d.department_name = 'Executive';
```

```txt
6.选择指定员工的姓名，员工号，以及他的管理者的姓名和员工号，结果类似于下面的格式
	employees   Emp		# manager Mgr#
	kochhar 	101 	  king 	  100
```

```sql
SELECT emp.last_name employees, emp.employee_id "Emp#", mgr.last_name manager,
mgr.employee_id "Mgr#"
FROM employees emp
LEFT OUTER JOIN employees mgr
ON emp.manager_id = mgr.employee_id;
```

### 7.查询哪些部门没有员工

```sql
SELECT d.department_id
FROM departments d
LEFT JOIN employees e
ON e.department_id = d.department_id
WHERE e.department_id IS NULL;
```

```sql
SELECT department_id
FROM departments d
WHERE NOT EXISTS (
	SELECT * 
    FROM employees e
    WHERE e.department_id = d.department_id
);
```

### 8. 查询哪个城市没有部门

```sql
SELECT l.location_id,l.city
FROM locations l LEFT JOIN departments d
ON l.`location_id` = d.`location_id`
WHERE d.`location_id` IS NULL
```

### 9. 查询部门名为 Sales 或 IT 的员工信息

```sql
SELECT employee_id,last_name,department_name
FROM employees e,departments d
WHERE e.department_id = d.`department_id`
AND d.`department_name` IN ('Sales','IT');
```





## 第06章_多表查询

### 多表查询-1

```sql 
【题目】
# 1.显示所有员工的姓名，部门号和部门名称。
# 2.查询90号部门员工的job_id和90号部门的location_id
# 3.选择所有有奖金的员工的 last_name , department_name , location_id , city
# 4.选择city在Toronto工作的员工的 last_name , job_id , department_id , department_name
# 5.查询员工所在的部门名称、部门地址、姓名、工作、工资，其中员工所在部门的部门名称为’Executive’
# 6.选择指定员工的姓名，员工号，以及他的管理者的姓名和员工号，结果类似于下面的格式
	employees   Emp  # manager   Mgr#
	kochhar 	101    king      100
# 7.查询哪些部门没有员工
# 8. 查询哪个城市没有部门
# 9. 查询部门名为 Sales 或 IT 的员工信息
```

### 1.显示所有员工的姓名，部门号和部门名称。

```sql
SELECT last_name,e.department_id,department_name 
FROM employees e 
JOIN depatments d 
ON e.`department_id` = d.`department_id`;
```

### 2.查询90号部门员工的job_id和90号部门的location_id

```sql
SELECT job_id,location_id 
FROM employees e 
JOIN departments d 
ON e.`department_id` = d.`department_id`
WHERE e.`department_id` = 90;
```

``` sql
SELECT job_id,location_id
FROM employees e,departments d
WHERE e.`department_id` = d.`department_id` 
AND e.`department_id` = 90;
```

### 3.选择所有有奖金的员工的 last_name , department_name , location_id , city

```sql
SELECT last_name,department_name,d.location_id,city 
FROM employees e
LEFT JOIN departments d
ON e.`department_id` = d.`department_id`
LEFT JOIN locations l
ON d.`location_id` = l.`location_id`
WHERE commission_pct IS NOT NULL;
```

### 4.选择city在Toronto工作的员工的 last_name , job_id , department_id , department_name

```SQL
SELECT last_name,job_id,e.department_id,department_name
FROM employees e
LEFT JOIN departments d
ON e.`department_id` = d.`department_id`
LEFT JOIN locations l
ON d.`location_id` = l.`location_id`
WHERE city = 'Toronto';
```

### 5.查询员工所在的部门名称、部门地址、姓名、工作、工资，其中员工所在部门的部门名称为’Executive’

```sql
SELECT last_name,job_id,salary,department_name,street_address
FROM employees e
LEFT JOIN departments d
ON e.`department_id` = d.`department_id`
LEFT JOIN locations l
ON d.`location_id` = l.`location_id`
WHERE department_name = 'Executive';
```

```sql
SELECT last_name , job_id , e.department_id , department_name
FROM employees e, departments d, locations l
WHERE e.`department_id` = d.`department_id`
AND d.`location_id` = l.`location_id`
AND city = 'Toronto';
```

###  6.选择指定员工的姓名，员工号，以及他的管理者的姓名和员工号，结果类似于下面的格式

```sql
	employees   Emp  # manager   Mgr#
	kochhar 	101    king      100
```

```sql
SELECT emp.last_name employees,emp.employee_id "Emp#",mgr.last_name manager,mgr.employee_id "Mgr#"
FROM employees emp
LEFT JOIN employees mgr
ON emp.`manager_id` = mgr.`employee_id`;
```

### 7.查询哪些部门没有员工

```sql
SELECT d.department_id
FROM departments d 
LEFT JOIN employees e
ON e.`department_id` = d.`department_id`
WHERE e.department_id IS NULL;
```

### 8. 查询哪个城市没有部门

```sql
SELECT l.location_id,l.city
FROM locations l
LEFT JOIN departments d
ON l.`location_id` = d.`location_id`
WHERE d.location_id IS NULL;
```

### 9. 查询部门名为 Sales 或 IT 的员工信息

```sql
SELECT employee_id,last_name,department_name
FROM employees e,departments d
WHERE e.department_id = d.`department_id`
AND d.`department_name` IN ('Sales','IT');
```

## 第07章_单行函数

```sql
# 1.显示系统时间(注：日期+时间)
# 2.查询员工号，姓名，工资，以及工资提高百分之20%后的结果（new salary）
# 3.将员工的姓名按首字母排序，并写出姓名的长度（length）
# 4.查询员工id,last_name,salary，并作为一个列输出，别名为OUT_PUT
# 5.查询公司各员工工作的年数、工作的天数，并按工作年数的降序排序
# 6.查询员工姓名，hire_date , department_id，满足以下条件：雇用时间在1997年之后，department_id为80 或 90 或110, commission_pct不为空
# 7.查询公司中入职超过10000天的员工姓名、入职时间
# 8.做一个查询，产生下面的结果
<last_name> earns <salary> monthly but wants <salary*3>
# 9.使用case-when，按照下面的条件：
job 		grade
AD_PRES 	A
ST_MAN 		B
IT_PROG 	C
SA_REP 		D
ST_CLERK 	E
```

### 1.显示系统时间(注：日期+时间)

```sql
SELECT NOW()
FROM DAUL;
```

### 2.查询员工号，姓名，工资，以及工资提高百分之20%后的结果（new salary）

```SQL
SELECT employee_id,last_name,salary,salary * 1.2 "new salary"
FROM employees;
```

### 3.将员工的姓名按首字母排序，并写出姓名的长度（length）

```sql
SELECT last_name,length(last_name)
FROM employees
ORDER BY last_name DESC;
```

### 4.查询员工id,last_name,salary，并作为一个列输出，别名为OUT_PUT

```sql
SELECT CONCAT(employee_id, ',' ,last_name, ',' ,salary) OUT_PUT
FROM employees;
```

### 5.查询公司各员工工作的年数、工作的天数，并按工作年数的降序排序

```sql
SELECT DATEIFF(SYSDATE(), hire_date) / 365 worked_years,DATEIFF(SYSDATE(),hire_date) worked_days
FROM employees
ORDER BY worked_days DESC; 
```

### 6.查询员工姓名，hire_date , department_id，满足以下条件：雇用时间在1997年之后，department_id为80 或 90 或110, commission_pct不为空

```SQL
SELECT last_name,hire_date,department_id
FROM employees
#WHERE hire_date >= 1997;
#WHERE hire_date >= STR_TO_DATE('1997-01-01', '%Y-%m-%d')
WHERE DATE_FORMAT(hire_date, '%Y') >= '1997'
AND department_id IN (80, 90, 110)
AND commission_pct IS NOT NULL;	
```

### 7.查询公司中入职超过10000天的员工姓名、入职时间

```SQL
SELECT last_name,hire_date
FROM employees
#WHERE TO_DAYS(NOW()) - to_days(hire_date) > 10000;
WHERE DATEDIFF(NOW(), hire_date) > 10000;
```

### 8.做一个查询，产生下面的结果

```SQL
-- <last_name> earns `<salary>` monthly but wants <salary*3>
-- Dream Salary
-- King earns 24000 monthly but wants 72000
```

```SQL
SELECT CONCAT(last_name, ' earns ', TRUNCATE(salary, 0) , ' monthly but wants ',
TRUNCATE(salary * 3, 0)) "Dream Salary"
FROM employees;
```

### 9.使用CASE-WHEN，按照下面的条件：

```SQL
-- job 			grade
-- AD_PRES 		A
-- ST_MAN 		B
-- IT_PROG 		C
-- SA_REP 		D
-- ST_CLERK 	E
-- 产生下面的结果
-- Last_name 		Job_id Grade
-- king AD_PRES 	A
```

```SQL
SELECT last_name Last_name, job_id Job_id, CASE job_id 
WHEN 'AD_PRES' THEN 'A'
WHEN 'ST_MAN' THEN 'B'
WHEN 'IT_PROG' THEN 'C'
WHEN 'SA_REP' THEN 'D'
WHEN 'ST_CLERK' THEN 'E'
ELSE 'F'
END "grade"
FROM employees;
```

## 第08章_聚合函数

1. 查询公司员工工资的最大值，最小值，平均值，总和
2. 查询各job_id的员工工资的最大值，最小值，平均值，总和
3. 选择具有各个job_id的员工人数
4. 查询员工最高工资和最低工资的差距（DIFFERENCE）
5. 查询各个管理者手下员工的最低工资，其中最低工资不能低于6000，没有管理者的员工不计算在内
6. 查询所有部门的名字，location_id，员工数量和平均工资，并按平均工资降序
7. 查询每个工种、每个部门的部门名、工种名和最低工资

### 1.查询公司员工工资的最大值，最小值，平均值，总和

```sql
SELECT MAX(salary),MIN(salary),SUM(salary)
FROM employees;
```

### 2.查询各job_id的员工工资的最大值，最小值，平均值，总和

```sql
SELECT job_id, MAX(salary), MIN(salary), AVG(salary), SUM(salary)
FROM employees
GROUP BY job_id;
```

### 3.选择具有各个job_id的员工人数

```sql
SELECT job_id, COUNT(*)
FROM employees
GROUP BY job_id;
```

### 4.查询员工最高工资和最低工资的差距（DIFFERENCE）

```sql
SELECT MAX(salary), MIN(salary), MAX(salary) - MIN(salary) DIFFERENCE
FROM employees;
```

### 5.查询各个管理者手下员工的最低工资，其中最低工资不能低于6000，没有管理者的员工不计算在内

```sql
SELECT manager_id, MIN(salary)
FROM employees
WHERE manager_id IS NOT NULL
GROUP BY manager_id
HAVING MIN(salary) > 6000;
```

### 6.查询所有部门的名字，location_id，员工数量和平均工资，并按平均工资降序

```sql
SELECT department_name, location_id, COUNT(employee_id), AVG(salary) avg_sal
FROM employees e RIGHT JOIN departments d
ON e.`department_id` = d.`department_id`
GROUP BY department_name, location_id
ORDER BY avg_sal DESC;
```

### 7.查询每个工种、每个部门的部门名、工种名和最低工资

```SQL
SELECT department_name, job_id,MIN(salary)
FROM departments d LEFT JOIN employees e
ON e.`department_id` = d.`department_id`
GROUP BY department_name,job_id;
```

## 第09章_子查询案例分析

### 1、查询工资大于149号员工工资的员工信息

```sql
#第一步 先查询149号的工资
SELECT employee_id,salary
FROM employees
WHERE employee_id = 149;
#第二步 在进行查询员工的信息
SELECT employee_id,salary
FROM employees
WHERE salary > (SELECT salary
				FROM employees
				WHERE employee_id = 149
               );
```

### 2、返回job_id与141号员工相同，salary比143号员工多的员工姓名，job_id和工资

```sql
#第一步 查询143号员工的工资
SELECT salary
FROM employees
WHERE employee_id = 143;
# 第二步查询信息
SELECT last_name,job_id,salary
FROM employees
WHERE job_id = (SELECT job_id
                FROM employees
                WHERE employee_id = 141
                )
AND salary > (
			  SELECT salary
			  FROM employees
	          WHERE employee_id = 143
			  );
```

### 3、返回公司工资最少的员工的last_name,job_id和salary

```sql
# 第一步 先查询工资最少的员工
SELECT MIN(salary)
FROM employees;
SELECT salary
FROM employees
ORDER BY salary ASC
LIMIT 1;
# 第二步 进行查询last_name,job_id和salary
SELECT last_name,job_id,salary
FROM employees
WHERE salary = (SELECT MIN(salary) 
                FROM employees
               );
SELECT last_name,job_id,salary
FROM employees
WHERE salary = (SELECT salary
				FROM employees
				ORDER BY salary ASC
				LIMIT 1
               );
```

### 4、查询与141号员工的manager_id和department_id相同的其他员工的employee_id,manager_id,department_id

```sql
#第一步 查询141号员工的manager_id和department_id
SELECT manager_id
FROM employees
WHERE employee_id = 141
#######################
SELECT department_id
FROM employees
WHERE employee_id = 141

#第二步 查询其他员工的employee_id,manager_id,department_id
SELECT employee_id,manager_id,department_id
FROM employees
WHERE manager_id = (SELECT manager_id
                    FROM employees
                    WHERE employee_id = 141
                   )
AND department_id = (SELECT department_id
                     FROM employees
                     WHERE employee_id = 141
                    )
AND employee_id <> 141;
```

### 5、查询最低工资大于50号部门最低工资的部门id和其最低工资

```sql
# 第一步 查询50号部门的最低工资
SELECT MIN(salary)
FROM employees
WHERE department_id = 50;

# 第二步 查询最低工资大于50号部门最低工资的部门id和其最低工资
SELECT department_id,MIN(salary)
FROM employees
GROUP BY department_id
HAVING MIN(salary) > (SELECT MIN(salary)
				      FROM employees
					  WHERE department_id = 50
                      );
```

### 6、显示员工的employee_id,last_name和location,其中，若员工的department_id与location_id为1800的department_id相同，则location为'Canada'，其余则为'USA'

```sql
# 第一步 查询location_id为1800员工的department_id
SELECT department_id
FROM departments
WHERE location_id = 1800;
# 第二步 
SELECT employee_id,last_name,CASE department_id WHEN (
								  SELECT department_id
								  FROM departments
								  WHERE location_id = 1800)
								  THEN 'Canada'
								  ELSE 'USA' END "location"
FROM employees;
```

### 7、查询平均工资最低的部门id

```sql
# 第一步 查询每个部门的最低工资按部门分组
SELECT AVG(salary) avg_sal
FROM employees
GROUP BY department_id;
# 第二步 查询最低平均工资
SELECT MIN(avg_sal)
FROM (SELECT AVG(salary) avg_sal
	  FROM employees
	  GROUP BY department_id) dept_avg_sal;
# 1、第三步 查询平均工资最低的部门id
SELECT department_id
FROM employees
GROUP BY department_id
HAVING AVG(salary) = (
						SELECT MIN(avg_sal)
						FROM (
                            SELECT AVG(salary) avg_sal
	  						FROM employees
	  						GROUP BY department_id 
                        ) dept_avg_sal
					 );
# 2、第三步 查询平均工资最低的部门id
SELECT department_id
FROM employees
GROUP BY department_id
HAVING AVG(salary) <= ALL(
							SELECT AVG(salary) avg_sal
	  						FROM employees
	  						GROUP BY department_id
							);
```

### 8、查询员工中工资大于本部门平均工资的员工的last_name,salary和其department_id

```sql
SELECT last_name,salary,department_id
FROM employees e1
WHERE salary > (
				SELECT AVG(salary)
				FROM employees e2
				WHERE department_id = e1.`department_id`
				);
```

```sql
SELECT e.last_name,e.salary,e.department_id
FROM employees e,(
				SELECT department_id,AVG(salary) avg_sal
				FROM employees
				GROUP BY department_id) tdas
WHERE e.department_id = tdas.department_id
AND e.salary > tdas.avg_sal;
```

### 9、查询员工的id,salary,按照department_name 排序

```sql
SELECT employee_id,salary
FROM employees e
ORDER BY (
		SELECT department_name
		FROM departments d
		WHERE e.`department_id` = d.`department_id`);
```

### 10、若employees表中employee_id与job_history表中employee_id相同的数目不小于2，输出这些员工的employee_id和其job_id

```sql
SELECT employee_id,last_name,job_id
FROM employees e
WHERE 2 <= (
    		SELECT COUNT(*)
			FROM job_history j
			WHERE e.`employee_id` = j.`employee_id`
			);
```

### 11、查询公司管理者的employee_id,last_name,job_id,department_id信息

第一种：自连接

```sql
SELECT DISTINCT mgr.employee_id,mgr.last_name,mgr.job_id,mgr.department_id
FROM employees emp JOIN employees mgr
ON emp.`manager_id` = mgr.`employee_id`;
```

第二种：子查询

```sql
SELECT employee_id,last_name,job_id,department_id
FROM employees
WHERE employee_id IN (
					SELECT DISTINCT manager_id
					FROM employees);
```

第三种：使用EXISTS

```sql
SELECT employee_id,last_name,job_id,department_id
FROM employees e1
WHERE EXISTS (
				SELECT *
				FROM employees e2
				WHERE e1.`employee_id` = e2.`manager_id`
			);
```

### 12、查询departments表中，不存在于employees表中的部门的department_id和department_name

第一种：多表连接

```sql
SELECT d.department_id,d.department_name
FROM employees e RIGHT JOIN departments d
ON e.`department_id` = d.`department_id`
WHERE e.`department_id` IS NULL;
```

第二种：使用EXISTS

```sql
SELECT department_id,department_name
FROM departments d
WHERE NOT EXISTS (
					SELECT *
					FROM employees e
					WHERE d.`department_id` = e.`department_id`
					);
```

### 13、谁的工资比Abel高？

第一种：自连接

```	sql
SELECT e2.last_name,e2.salary
FROM employees e1,employees e2
WHERE e1.last_name = 'Abel'
AND e1.`salary` < e2.`salary`;
```

第二种：子查询

```sql
SELECT last_name,salary
FROM employees
WHERE salary > (SELECT salary 
               FROM employees
               WHERE last_name = 'Abel'
               );
```

## 第09章_子查询 练习题

**【题目】：**

1. 查询和Zlotkey相同部门的员工姓名和工资

2. 查询工资比公司平均工资高的员工的员工号，姓名和工资。

3. 选择工资大于所有JOB_ID = 'SA_MAN'的员工的工资的员工的last_name, job_id, salary

4. 查询和姓名中包含字母u的员工在相同部门的员工的员工号和姓名

5. 查询在部门的location_id为1700的部门工作的员工的员工号

6. 查询管理者是King的员工姓名和工资

7. 查询工资最低的员工信息: last_name, salary

8. 查询平均工资最低的部门信息

9. 查询平均工资最低的部门信息和该部门的平均工资（相关子查询）

10. 查询平均工资最高的 job 信息

11. 查询平均工资高于公司平均工资的部门有哪些?

12. 查询出公司中所有 manager 的详细信息

13. 各个部门中 最高工资中最低的那个部门的 最低工资是多少?

14. 查询平均工资最高的部门的 manager 的详细信息: last_name, department_id, email, salary

15. 查询部门的部门号，其中不包括job_id是"ST_CLERK"的部门号
16. 选择所有没有管理者的员工的last_name
17. 查询员工号、姓名、雇用时间、工资，其中员工的管理者为 'De Haan'
18. 查询各部门中工资比本部门平均工资高的员工的员工号, 姓名和工资（相关子查询）
19. 查询每个部门下的部门人数大于 5 的部门名称（相关子查询）
20. 查询每个国家下的部门个数大于 2 的国家编号（相关子查询）

### 1.查询和Zlotkey相同部门的员工姓名和工资

```sql
SELECT last_name,salary
FROM employees
WHERE department_id = (
						SELECT department_id
						FROM employees
						WHERE last_name = 'Zlotkey');
```

### 2.查询工资比公司平均工资高的员工的员工号，姓名和工资。

```sql
SELECT employee_id,last_name,salary
FROM employees
WHERE salary > (
				SELECT AVG(salary)
				FROM employees
				);
```

### 3.选择工资大于所有JOB_ID = 'SA_MAN'的员工的工资的员工的last_name, job_id, salary

```sql
SELECT last_name,job_id,salary
FROM employees
WHERE salary > ALL(
				SELECT salary
				FROM employees
				WHERE job_id = 'SA_MAN');
```

### 4.查询和姓名中包含字母u的员工在相同部门的员工的员工号和姓名

```sql
SELECT employee_id,last_name
FROM employees
WHERE department_id = ANY(
						SELECT DISTINCT department_id
						FROM employees
						WHERE last_name LIKE '%u%');
```

### 5.查询在部门的location_id为1700的部门工作的员工的员工号

```sql
SELECT employee_id
FROM employees
WHERE department_id IN(
						SELECT department_id
						FROM departments
						WHERE location_id = 1700);
```

### 6.查询管理者是King的员工姓名和工资

```sql
SELECT last_name,salary
FROM employees
WHERE manager_id IN (
					SELECT employee_id
					FROM employees
					WHERE last_name = 'King');
```

### 7.查询工资最低的员工信息: last_name, salary

```sql
SELECT last_name,salary
FROM employees
WHERE salary = (
				SELECT MIN(salary)
				FROM employees
				);
```

### 8.查询平均工资最低的部门信息

方式一：

```SQL
SELECT * 
FROM departments
WHERE department_id = (
    					SELECT department_id
						FROM employees
						GROUP BY department_id
						HAVING AVG(salary) = (
                        			SELECT MIN(dept_avgsal)
                        			FROM (
                                    		SELECT AVG(salary) dept_avgsal
                                    		FROM employees
                                    		GROUP BY department_id) avg_sal
                        				  )
						);
```

方式二：

```sql
SELECT * 
FROM departments
WHERE department_id = (
						SELECT department_id
						FROM employees
						GROUP BY department_id
						HAVING AVG(salary) <= ALL(
                        						SELECT AVG(salary)
                        						FROM employees
                        						GROUP BY department_id
                        						)
					);
```

方式三：

```sql
SELECT *
FROM departments
WHERE department_id = (
						SELECT department_id
						FROM employees
						GROUP BY department_id
						HAVING AVG(salary) = (
                        					SELECT AVG(salary) avg_sal
                        					FROM employees
                        					GROUP BY department_id
                        					ORDER BY avg_sal
                        					LIMIT 0,1
                        					)
						);
```

方式四：

```sql
SELECT d.*
FROM departments d,(
					SELECT department_id,AVG(salary) avg_sal
					FROM employees
					GROUP BY department_id
					ORDER BY avg_sal
 					LIMIT 0,1) dept_avg_sal
WHERE d.department_id = dept_avg_sal.department_id;
```

### 9.查询平均工资最低的部门信息和该部门的平均工资（相关子查询）

方式一：

```sql
SELECT d.*,(SELECT AVG(salary) FROM employees WHERE department_id = d.department_id) avg_sal
FROM departments d
WHERE department_id = (
						SELECT department_id
						FROM employees
						GROUP BY department_id
						HAVING AVG(salary) = (
                        				SELECT MIN(dept_avgsal)
                        				FROM (
                                        		SELECT AVG(salary) dept_avgsal  
                                            	FROM employees
 												GROUP BY department_id
 												) avg_sal
                        			 		)
					);
```

方式二：

```sql
SELECT d.*,(SELECT AVG(salary) FROM employees WHERE department_id = d.department_id) avg_sal
FROM departments d
WHERE department_id = (
						SELECT department_id
						FROM employees
						GROUP BY department_id
						HAVING AVG(salary) <= ALL(
                        				SELECT AVG(salary) avg_sal
                        				FROM employees
                        				GROUP BY department_id
                        				)
					);
```

方式三：

```sql
SELECT d.*,(SELECT AVG(salary) FROM employees WHERE department_id = d.department_id) avg_sal
FROM departments d
WHERE department_id = (
						SELECT department_id
						FROM employees
						GROUP BY department_id
						HAVING AVG(salary) = (
                        				SELECT AVG(salary) avg_sal
                        				FROM employees
                        				GROUP BY department_id
                            			ORDER BY avg_sal
                            			LIMIT 0,1
                        				)
    				);
```

方式四：

```sql
SELECT d.*,dept_avg_sal.avg_sal
FROM departments d,(
    SELECT department_id,AVG(salary) avg_sal
    FROM employees
    GROUP BY department_id
    ORDER BY avg_sal
    LIMIT 0,1) dept_avg_sal
WHERE d.`department_id` = dept_avg_sal.`department_id`;
```

### 10. 查询平均工资最高的 job 信息

方式一：

```sql
SELECT *
FROM jobs
WHERE job_id = (
				SELECT job_id
				FROM employees
				GROUP BY job_id
				HAVING AVG(salary) = (
                			SELECT MAX(avg_sal)
                			FROM (
                            		SELECT AVG(salary) avg_sal
                            		FROM employees
                            		GROUP BY job_id
                            		) job_avgsal
                				)
    );
```

方式二：

```sql
SELECT *
FROM jobs
WHERE job_id = (
				SELECT job_id
				FROM employees
				GROUP BY job_id
				HAVING AVG(salary) >= ALL(
                					SELECT AVG(salary)
                					FROM employees
                					GROUP BY job_id
                					)
    			);
```

方式三：

```sql
SELECT *
FROM jobs
WHERE job_id = (
				SELECT job_id
				FROM employees
				GROUP BY job_id
				HAVING AVG(salary) = (
                					SELECT AVG(salary) avg_sal
                					FROM employees
                					GROUP BY job_id
                					ORDER BY avg_sal DESC
                					LIMIT 0,1
                					)
    			);
```

方式四：

```sql
SELECT j.*
FROM jobs j,(
			SELECT job_id,AVG(salary) avg_sal
			FROM employees
			GROUP BY job_id
			ORDER BY avg_sal DESC
			LIMIT 0,1) job_avg_sal
WHERE j.`job_id` = job_avg_sal.`job_id`;
```

### 11.查询平均工资高于公司平均工资的部门有哪些?

```sql
SELECT department_id
FROM employees
WHERE department_id IS NOT NULL
GROUP BY department_id
HAVING AVG(salary) > (
					SELECT AVG(salary)
					FROM employees
					);
```

### 12.查询出公司中所有 manager 的详细信息

方式一：

```sql
SELECT employee_id,last_name,salary
FROM employees
WHERE employee_id IN (
					SELECT DISTINCT manager_id
					FROM employees
					);
```

方式二：

``` sql
SELECT DISTINCT e1.employee_id,e1.last_name,e1.salary
FROM employees e1 JOIN employees e2
WHERE e1.`employee_id` = e2.`manager_id`;
```

方式三：

```sql
SELECT employee_id,last_name,salary
FROM employees e1
WHERE EXISTS (
				SELECT *
				FROM employees e2
				WHERE e2.manager_id = e1.employee_id
			);
```

### 13.各个部门中 最高工资中最低的那个部门的 最低工资是多少?

方式一：

```sql
# 第一步 先查询各部门最高工资
SELECT MAX(salary) 
FROM employees 
GROUP BY department_id;
# 第二步 查询最低的工资
SELECT MIN(max_sal)
FROM (
    SELECT MAX(salary) max_sal
	FROM employees 
	GROUP BY department_id
	) dept_max_sal;
# 第三步 查询最低工资的部门id
SELECT department_id
FROM employees
GROUP BY department_id
HAVING MAX(salary) = (
			SELECT MIN(max_sal)
			FROM (
    			SELECT MAX(salary) max_sal
				FROM employees 
				GROUP BY department_id
				) dept_max_sal
		);
# 第四步 查询最低工资
SELECT MIN(salary)
FROM employees
WHERE department_id = (
					SELECT department_id
					FROM employees
					GROUP BY department_id
					HAVING MAX(salary) = (
							SELECT MIN(max_sal)
							FROM (
    								SELECT MAX(salary) max_sal
									FROM employees 
									GROUP BY department_id
									) dept_max_sal
					)
			);
SELECT * 
FROM employees 
WHERE department_id = 10;
```

方式二：

```sql
# 第一步 先查询各部门的最高工资按部门id分组
SELECT MAX(salary) max_sal
FROM employees
GROUP BY department_id;
# 第二步 查询最高工资的部门id
SELECT department_id
FROM employees
GROUP BY department_id
HAVING MAX(salary) <= ALL(
    SELECT MAX(salary) max_sal
	FROM employees
	GROUP BY department_id
	);
# 第三步 查询最高工资的部门中的最低工资
SELECT MIN(salary)
FROM employees
WHERE department_id = (
						SELECT department_id
						FROM employees
						GROUP BY department_id
						HAVING MAX(salary) <= ALL(
    								SELECT MAX(salary) max_sal
									FROM employees
									GROUP BY department_id
									)
					);
```

方式三：

```sql
SELECT MIN(salary) 
FROM employees
WHERE department_id = (
     SELECT department_id  
     FROM employees
 	 GROUP BY department_id  
     HAVING MAX(salary) = (
          SELECT MAX(salary) max_sal  
          FROM employees
 		  GROUP BY department_id  
          ORDER BY max_sal
 		  LIMIT 0,1
         )
    );
```

方式四：

```sql
SELECT employee_id,MIN(salary)
FROM employees e,(
					SELECT department_id,MAX(salary) max_sal
					FROM employees
					GROUP BY department_id
					ORDER BY max_sal
					LIMIT 0,1) dept_max_sal
WHERE e.`department_id` = dept_max_sal.`department_id`;
```

### 14.查询平均工资最高的部门的 manager 的详细信息: last_name, department_id, email, salary 

方式一：

```sql
SELECT employee_id,last_name,department_id,email,salary
FROM employees
WHERE employee_id IN(
					SELECT DISTINCT manager_id
					FROM employees
					WHERE department_id = (
									SELECT department_id
									FROM employees
									GROUP BY department_id
									HAVING AVG(salary) = (
											SELECT MAX(max_sal)
											FROM (
													SELECT AVG(salary) max_sal
													FROM employees
													GROUP BY department_id) dept_sal
						)
					)
			);
```

方式二：

```sql
#方式二：
SELECT employee_id,last_name, department_id, email, salary 
FROM employees
WHERE employee_id IN (
					SELECT DISTINCT manager_id FROM employees
					WHERE department_id = (
								SELECT department_id FROM employees  e GROUP BY department_id
								HAVING AVG(salary) >= ALL(
										SELECT AVG(salary) FROM employees
										GROUP BY department_id
										)
								)
					);
```

方式三：

```sql
#方式三： 
SELECT *
FROM employees
WHERE employee_id IN (
						SELECT DISTINCT manager_id FROM employees e,(
									SELECT department_id,AVG(salary) avg_sal FROM employees
									GROUP BY department_id ORDER BY avg_sal DESC LIMIT 0,1) dept_avg_sal
						WHERE e.department_id = dept_avg_sal.department_id
);
```

### 15. 查询部门的部门号，其中不包括job_id是"ST_CLERK"的部门号

方式一：

```sql
SELECT department_id
FROM departments
WHERE department_id NOT IN (
							SELECT DISTINCT department_id
							FROM employees
							WHERE job_id = 'ST_CLERK'
							);
```

方式二：

```sql
SELECT department_id
FROM departments d
WHERE NOT EXISTS (
				SELECT *
				FROM employees e
				WHERE d.`department_id` = e.`department_id`
				AND job_id = 'ST_CLERK'
);
```

### 16. 选择所有没有管理者的员工的last_name

```sql
SELECT last_name
FROM employees e1
WHERE NOT EXISTS (
					SELECT *
					FROM employees e2
					WHERE e1.`manager_id` = e2.`employee_id`
				 );
```

### 17．查询员工号、姓名、雇用时间、工资，其中员工的`管理者`为 'De Haan'

```sql
#方式1：
SELECT employee_id, last_name, hire_date, salary
FROM employees
WHERE manager_id = (
					SELECT employee_id
					FROM employees
					WHERE last_name = 'De Haan'
);
```

方式二：

```sql
#方式2：
SELECT employee_id, last_name, hire_date, salary
FROM employees e1
WHERE EXISTS (
				SELECT *
				FROM employees e2
				WHERE e2.`employee_id` = e1.manager_id
				AND e2.last_name = 'De Haan'
);
```

### 18.查询各部门中工资比本部门平均工资高的员工的员工号, 姓名和工资(难)

方式一：

```sql
#方式一：相关子查询
SELECT employee_id,last_name,salary
FROM employees e1
WHERE salary > (
				# 查询某员工所在部门的平均
				SELECT AVG(salary)
				FROM employees e2
				WHERE e2.department_id = e1.`department_id`
);
```

方式二：

```sql
#方式二：
SELECT employee_id,last_name,salary
FROM employees e1,
				(	SELECT department_id,AVG(salary) avg_sal
					FROM employees e2 GROUP BY department_id
				) dept_avg_sal
WHERE e1.`department_id` = dept_avg_sal.department_id
AND e1.`salary` > dept_avg_sal.avg_sal;
```

### 19.查询每个部门下的部门人数大于 5 的部门名称

```sql
SELECT department_name,department_id
FROM departments d
WHERE 5 < (
			SELECT COUNT(*)
			FROM employees e
			WHERE d.`department_id` = e.`department_id`
);
```

### 20.查询每个国家下的部门个数大于 2 的国家编号

```sql
SELECT country_id
FROM locations l
WHERE 2 < (
			SELECT COUNT(*)
			FROM departments d
			WHERE l.`location_id` = d.`location_id`
);
```





## 第10章_创建和管理表

### 练习1

```sql
#1. 创建数据库test01_office,指明字符集为utf8。并在此数据库下执行下述操作
#2. 创建表dept01
/*
字段 	   类型
id 		INT(7)
NAME 	VARCHAR(25)
*/
#3. 将表departments中的数据插入新表dept02中
#4. 创建表emp01
/*
字段 	   		类型
id 			 INT(7)
first_name   VARCHAR (25)
last_name    VARCHAR(25)
dept_id 	 INT(7)
*/
#5. 将列last_name的长度增加到50
#6. 根据表employees创建emp02
#7. 删除表emp01
#8. 将表emp02重命名为emp01
#9.在表dept02和emp01中添加新列test_column，并检查所作的操作
#10.直接删除表emp01中的列 department_id
```

### 1. 创建数据库test01_office,指明字符集为utf8。并在此数据库下执行下述操作

```sql
CREATE DATABASE IF NOT EXISTS test01_office CHARACTER SET 'utf8';

USE test01_office;
```

### 2. 创建表dept01

```sql
CREATE TABLE dept01(
`id` INT(7),
`name` VARCHAR(25)
);
```

### 3. 将表departments中的数据插入新表dept02中

```sql
CREATE TABLE dept02 AS SELECT * FROM atguigudb.departments;
```

### 4. 创建表emp01

```sql
CREATE TABLE emp01(
`id` INT(7),
`first_name` VARCHAR(25)
`last_name` VARCHAR(25)
`dept_id` INT(7)
);
```

### 5. 将列last_name的长度增加到50

```sql
ALTER TABLE emp01 MODIFY last_name VARCHAR(50);
```

### 6. 根据表employees创建emp02

```sql\
CREATE TABLE emp02 AS SELECT * FROM atguigudb.`employees`;
```

### 7. 删除表emp01

```sql
DROP TABLE IF EXISTS emp01;
```

### 8. 将表emp02重命名为emp01

```SQL
ALTER TABLE emp02 RENAME emp01;
RENAME TABLE emp02 TO emp01;
```

### 9.在表dept02和emp01中添加新列test_column，并检查所作的操作

```sql
ALTER TABLE emp01 ADD test_column VARCHAR(10);
DESC emp01;

ALTER TABLE dept02 ADD test_column VARCHAR(10);
DESC dept02;
```

### 10.直接删除表emp01中的列 department_id

```sql
ALTER TABLE emp01 DROP COLUMN department_id;
```

## 第11章 数据的增删改

```sql
# 储备工作
USE atguigudb;

# 创建表
CREATE TABLE IF NOT EXISTS emp1(
id INT(7),
`name` VARCHAR(15),
hire_date DATE,
salary DOUBLE(10,2)
 );

DESC emp1;

# 1.添加数据
# 方式一：一条一条添加数据
INSERT INTO emp1 VALUES (1,'Tom','2000-07-28',12000);
# 指明要添加的字段
INSERT INTO emp1(id, `name`, salary)
VALUES (2, 'Jerry', 15000);
# 方式二：将查询结果插入到表中
INSERT INTO emp1(id, `name`, salary, hire_date)
SELECT employee_id,last_name,salary,hire_date
FROM employees
WHERE department_id IN (60,70);
```

## 综合练习

```sql
# 1、创建数据库test01_library
CREATE DATEBASE IF NOT EXISTS test01_library;
# 2、创建表 books，表结构如下：
字段名		字段说明	数据类型
id 		  书编号	   INT
name 	  书名		VARCHAR(50)
authors   作者		VARCHAR(100)
price 	  价格		FLOAT
pubdate   出版日期	   YEAR
note 	  说明		VARCHAR(100)
num 	  库存		INT
CREATE TABLE IF NOT EXISTS books (
id INT(7) NOT NULL,
`name` VARCHAR(50),
authors VARCHAR(100),
price FLOAT,
pubdate YEAR,
note VARCHAR(100),
num INT(10)
);

# 3、向books表中插入记录
# 1）不指定字段名称，插入第一条记录
INSERT INTO books
VALUES(1,'Tal of AAA','Dickes',23,1995,'novel',11);
# 2）指定所有字段名称，插入第二记录
INSERT INTO books (id,name,`authors`,price,pubdate,note,num)
VALUES(2,'EmmaT','Jane lura',35,1993,'Joke',22);
# 3）同时插入多条记录（剩下的所有记录）
INSERT INTO books VALUES(3, 'Story of Jane', 'Jane Tim', 40, '2001', 'novel', 0),
						(4, 'Lovey Day', 'George Byron', 20, '2005', 'novel', 30),
						(5, 'Old land', 'Honore Blade', 30, '2010', 'law', 0),
						(6, 'The Battle', 'Upton Sara', 30, '1999', 'medicine', 40);
						
1 	Tal of AAA 		Dickes 				23 		1995 	novel 		11
2 	EmmaT 			Jane lura 			35 		1993 	joke 		22
3 	Story of Jane 	Jane Tim 			40 		2001 	novel 		0
4 	Lovey Day 		George Byron 		20 		2005 	novel 		30
5 	Old land 		Honore Blade 		30 		2010 	law 		0
6 	The Battle 		Upton Sara 			30 		1999 	medicine 	40
7 	Rose Hood 		Richard haggard 	28 		2008 	cartoon 	28
						
# 4、将小说类型(novel)的书的价格都增加5。
UPDATE books SET price = price + 5 WHERE note = 'novel';

# 5、将名称为EmmaT的书的价格改为40，并将说明改为drama。
UPDATE books SET price = 40,note = drama WHERE name = 'Emmat';

# 6、删除库存为0的记录。
DELETE FROM books WHERE num = 0;

# 7、统计书名中包含a字母的书
SELECT * FROM books WHERE name LIKE '%a%';

# 8、统计书名中包含a字母的书的数量和库存总量
SELECT COUNT(*),SUM(num) FROM books WHERE name LIKE '%a%';

# 9、找出“novel”类型的书，按照价格降序排列
SELECT * FROM books WHERE note = 'novel' ORDER BY price DESC;

# 10、查询图书信息，按照库存量降序排列，如果库存量相同的按照note升序排列
SELECT * FROM books ORDER BY num DESC , note ASC;

# 11、按照note分类统计书的数量
SELECT SUM(num) FROM books GROUP BY note;

# 12、按照note分类统计书的库存量，显示库存量超过30本的
SELECT SUM(num) sum_num FROM books GROUP BY note HAVING sum_num > 30;

# 13、查询所有图书，每页显示5本，显示第二页
SELECT * FROM books LIMIT 5,5;

# 14、按照note分类统计书的库存量，显示库存量最多的
SELECT note,SUM(num) sum_num FROM books GROUP BY note ORDER BY sum_num DESC LIMIT 0,1;

# 15、查询书名达到10个字符的书，不包括里面的空格
SELECT * FROM books WHERE CHAR_LENGTH(REPLACE(name,' ','')) >= 10;

# 16、查询书名和类型，其中note值为novel显示小说，law显示法律，medicine显示医药，cartoon显示卡通，joke显示笑话
SELECT name AS "书名",note, CASE note
WHEN 'novel' THEN '小说'
WHEN 'law' THEN '法律'
WHEN 'medicine' THEN '医药'
WHEN 'cartoon' THEN '卡通'
WHEN 'joke' THEN '笑话'
END AS "类型"
FROM books;

# 17、查询书名、库存，其中num值超过30本的，显示滞销，大于0并低于10的，显示畅销，为0的显示需要无货
SELECT name,num,CASE
WHEN num>30 THEN '滞销'
WHEN num>0 AND num<10 THEN '畅销'
WHEN num=0 THEN '无货'
ELSE '正常'
END AS "库存状态"
FROM books;

# 18、统计每一种note的库存量，并合计总量
SELECT IFNULL(note,'合计库存总量') AS note,SUM(num) FROM books GROUP BY note WITH ROLLUP;

# 19、统计每一种note的数量，并合计总量
SELECT IFNULL(note,'合计库存总量') AS note,COUNT(*) FROM books GROUP BY note WITH ROLLUP;

# 20、统计库存量前三名的图书
SELECT * FROM books ORDER BY num DESC LIMIT 0,3;

# 21、找出最早出版的一本书
SELECT * FROM books ORDER BY pubdate ASC LIMIT 0,1;

# 22、找出novel中价格最高的一本书
SELECT * FROM books WHERE note = 'novel' ORDER BY price DESC LIMIT 0,1;

# 23、找出书名中字数最多的一本书，不含空格
SELECT * FROM books ORDER BY CHAR_LENGTH(REPLACE(name,' ','')) LIMIT 0,1;
```

## 第13章_约束

### 基础练习：

### 练习1

已经存在数据库test04_emp，两张表emp2和dept2

```SQL
CREATE DATABASE test04_emp;
use test04_emp;
CREATE TABLE emp2(
id INT,
emp_name VARCHAR(15)
);
CREATE TABLE dept2(
id INT,
dept_name VARCHAR(15)
);
```

### 题目：

```sql
#1. 向表emp2的id列中添加PRIMARY KEY约束
#2. 向表dept2的id列中添加PRIMARY KEY约束
#3. 向表emp2中添加列dept_id，并在其中定义FOREIGN KEY约束，与之相关联的列是dept2表中的id列。
```

### 答案：

```sql
#1. 向表emp2的id列中添加PRIMARY KEY约束
ALTER TABLE emp2
MODIFY COLUMN id INT PRIMARY KEY;

#2. 向表dept2的id列中添加PRIMARY KEY约束
ALTER TABLE dept2
MODIFY COLUMN id INT PRIMARY KEY;

#3. 向表emp2中添加列dept_id，并在其中定义FOREIGN KEY约束，与之相关联的列是dept2表中的id列。
ALTER TABLE emp2 ADD COLUMN dept_id INT;
ALTER TABLE emp2 ADD CONSTRAINT fk_emp2_deptid FOREIGN KEY(dept_id) REFERNCES dept2(id);

```

### 练习2

承接《第11章_数据处理之增删改》的综合案例。

### 题目：

```sql
# 1、创建数据库test01_library
# 2、创建表 books，表结构如下：
# 3、使用ALTER语句给books按如下要求增加相应的约束
#给id增加主键约束
ALTER TABLE books MODIFY COLUMN id INT PRIMARY KEY;
#给id字段增加自增约束
ALTER TABLE books MODIFY COLUMN id INT AUTO_INCREMENT;
#给name等字段增加非空约束
ALTER TABLE books name VARCHAR(50) NOT NULL;
ALTER TABLE books `authors` VARCHAR(100) NOT NULL;
ALTER TABLE books price FLOAT NOT NULL;
ALTER TABLE books pubdate DATE NOT NULL;
ALTER TABLE books num INT NOT NULL;
```

### 练习3

### 题目：

```sql
#1. 创建数据库test04_company
#2. 按照下表给出的表结构在test04_company数据库中创建两个数据表offices和employees
```

- offices表：

  <img src="C:/Users/10655/AppData/Roaming/Typora/typora-user-images/image-20220214210432365.png" alt="image-20220214210432365" style="zoom:150%;" />

- employees表：

- <img src="C:/Users/10655/AppData/Roaming/Typora/typora-user-images/image-20220214210509893.png" alt="image-20220214210509893" style="zoom:150%;" />

```sql
#3. 将表employees的mobile字段修改到officeCode字段后面
#4. 将表employees的birth字段改名为employee_birth
#5. 修改sex字段，数据类型为CHAR(1)，非空约束
#6. 删除字段note
#7. 增加字段名favoriate_activity，数据类型为VARCHAR(100)
#8. 将表employees名称修改为employees_info
```

### 答案：

```sql
#1. 创建数据库test04_company
CREATE DATABASE IF NOT EXISTS test04_company;

#2. 按照下表给出的表结构在test04_company数据库中创建两个数据表offices和employees
USE test04_company;

CREATE TABLE offices(
office_code INT(10),
city VARCHAR(50) NOT NULL,
address VARCHAR(50),
country VARCHAR(50) NOT NULL,
postal_code VARCHAR(50) UNIQUE
PRIMARY KEY(office_code)
);

CREATE TABLE employees(
employee_id INT(11) PRIMARY KEY AUTO_INCREMENT,
last_name VARCHAR(50) NOT NULL,
first_name VARCHAR(50) NOT NULL,
mobile VARCHAR(25) UNIQUE,
office_code INT(10) NOT NULL,
job_title VARCHAR(50) NOT NULL,
birth DATETIME NOT NULL,
note VARCHAR(255),
sex VARCHAR(5),
CONSTRAINT fk_emp_ofcode FOREIGN KEY(office_code) REFERENCES offices(office_code)
);

#3. 将表employees的mobile字段修改到officeCode字段后面
ALTER TABLE employees MODIFY mobile VARCHAR(25) AFTER office_code;
SELECT * FROM employees;

#4. 将表employees的birth字段改名为employee_birth
ALTER TABLE employees CHANGE birth employee_birth DATETIME;

#5. 修改sex字段，数据类型为CHAR(1)，非空约束
ALTER TABLE employees MODIFY sex CHAR(1) NOT NULL;

#6. 删除字段note
ALTER TABLE employees DROP COLUMN note;

#7. 增加字段名favoriate_activity，数据类型为VARCHAR(100)
ALTER TABLE employees ADD favoriate_activity VARCHAR(100);

#8. 将表employees名称修改为employees_info
ALTER TABLE employees RENAME employees_info;
```





