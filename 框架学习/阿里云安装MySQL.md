# Linux上安装MySQL

## 第一步：确保服务器在最新的状态（可有可无）

```shell
[root@chenstudy ~]# yum -y update
```

![image-20221007212426624](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221007212426624.png)

## 第二步：检测系统是否自带安装MySQL

- 我之前安装过MySQL，现在已经卸载了

```shell
[root@chenstudy ~]# rpm -qa | grep mysql
```

![image-20221007213106039](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221007213106039.png)

- 如果你系统有安装，那可以选择进行卸载:

```shell
rpm -e mysql　　// 普通删除模式
rpm -e --nodeps mysql　　// 强力删除模式，如果使用上面命令删除时，提示有依赖的其它文件，则用该命令可以对其进行强力删除
```

- 也可以使用以下方式删除

```shell
rpm -qa | grep mysql
yum remove mysql-community-libs-8.0.30-1.el7.x86_64
yum remove mysql-community-client-plugins-8.0.30-1.el7.x86_64
yum remove mysql-community-server-8.0.30-1.el7.x86_64
```

## 第三步：安装MySQL

**接下来我们在 Centos7 系统下使用 yum 命令安装 MySQL：**

- 我们把MySQL安装在/usr/local/mysql目录下

```shell
[root@chenstudy ~]# cd /usr/local
[root@chenstudy local]# mikdir mysql
[root@chenstudy local]# cd mysql/
[root@chenstudy mysql]# ls
```

- 下载YUM资源包

```shell
[root@chenstudy mysql]# wget http://repo.mysql.com/mysql80-community-release-el7-5.noarch.rpm
```

![image-20221007214713846](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221007214713846.png)



```shell
[root@chenstudy mysql]# rpm -ivh mysql80-community-release-el7-7.noarch.rpm
```

![image-20221007215140540](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221007215140540.png)

```shell
[root@chenstudy mysql]# yum update
```

![image-20221007215230223](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221007215230223.png)



```shell
[root@chenstudy mysql]# yum install mysql-server
```

![image-20221007215609815](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221007215609815.png)

![image-20221007215711089](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221007215711089.png)

## 第四步：配置MySQL

### 4.1、**权限设置：**

```shell
[root@chenstudy mysql]# chown -R mysql:mysql /var/lib/mysql
```

![image-20221007215936324](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221007215936324.png)

### 4.2、初始化MySQL

```shell
[root@chenstudy mysql]# mysqld --initialize
```

![image-20221007220209058](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221007220209058.png)

### 4.3、启动MySQL

- 启动时报错：`Job for mysqld.service failed because the control process exited with error code. See "systemctl status mysqld.service" and "journalctl -xe" for details.`
- 执行以下语句：

```shell
[root@chenstudy mysql]# chown mysql:mysql -R /var/lib/mysql
```

![image-20221007220718510](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221007220718510.png)

### 4.4、查看MySQL运行状态

```shell
[root@chenstudy mysql]# systemctl status mysqld
```

![image-20221007220931548](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221007220931548.png)

### 4.5、设置MySQL开机自启动

```shell
[root@chenstudy mysql]# systemctl enable mysqld
```

![image-20221007221103495](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221007221103495.png)

*注意：*\\**如果我们是第一次启动 mysql 服务，mysql 服务器首先会进行初始化的配置。**

> 此外,你也可以使用 MariaDB 代替，MariaDB 数据库管理系统是 MySQL 的一个分支，主要由开源社区在维护，采用 GPL 授权许可。开发这个分支的原因之一是：甲骨文公司收购了 MySQL 后，有将 MySQL 闭源的潜在风险，因此社区采用分支的方式来避开这个风险。
>
> MariaDB的目的是完全兼容MySQL，包括API和命令行，使之能轻松成为MySQL的代替品。
>
> ```
> yum install mariadb-server mariadb 
> ```
>
> mariadb数据库的相关命令是：
>
> ```
> systemctl start mariadb  #启动MariaDB
> systemctl stop mariadb  #停止MariaDB
> systemctl restart mariadb  #重启MariaDB
> systemctl enable mariadb  #设置开机启动
> ```

## 第五步：验证MySQL安装

在成功安装 MySQL 后，一些基础表会表初始化，在服务器启动后，你可以通过简单的测试来验证 MySQL 是否工作正常。

使用 mysqladmin 工具来获取服务器状态：

使用 mysqladmin 命令来检查服务器的版本, 在 linux 上该二进制文件位于 /usr/bin 目录，在 Windows 上该二进制文件位于C:\mysql\bin 。

```shell
[root@chenstudy mysql]# clear
[root@chenstudy mysql]# mysqladmin --version
mysqladmin  Ver 8.0.30 for Linux on x86_64 (MySQL Community Server - GPL)
```

![image-20221007221408663](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221007221408663.png)

## 第六步：登录MySQL

- 查看MySQL安装后的默认密码

```shell
[root@chenstudy mysql]# cd /var/log/
[root@chenstudy log]# ls
[root@chenstudy log]# cat mysqld.log
```

![image-20221007222325906](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221007222325906.png)

![image-20221007222449321](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221007222449321.png)

- 登录MySQL

```shell
[root@chenstudy log]# mysql -uroot -p
Enter password: 刚查到的密码
```

![image-20221007222652325](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221007222652325.png)

- 修改密码

```shell
mysql> alter user root@localhost identified by '密码';
Query OK, 0 rows affected (0.02 sec)
mysql> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.01 sec)
mysql> exit
Bye
[root@chenstudy log]# mysql -uroot -p
Enter password:‘新密码’

```



![image-20221007223446855](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221007223446855.png)

![image-20221007223554920](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221007223554920.png)

## 第七步： 开启远程登录，授权root远程登录

**解释：不要以为阿里云服务器可以远程登录root用户，就以为我们也可以以mysql的root用户身份远程登录mysql数据库**

```shell
mysql> USE mysql;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed

# 查看用户表权限
select host,user from user;
```

![image-20221007223929765](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221007223929765.png)

```shell
#运行下面两句话之后就可以通过root账户远程登陆。 
update user set host='%' where user='root';

#命令立即执行生效(千万不要忘记刷新！！！！！)
#这句表示从mysql数据库的grant表中重新加载权限数据
flush privileges;
```

![image-20221007224107320](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221007224107320.png)

- 查看阿里云安全组是否开放了3306端口（没有的可以配置下）

![image-20221007224640686](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221007224640686.png)

- 最后在Navicat上面链接下

![image-20221007224941051](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221007224941051.png)

## 完结撒花！！！！

# Linux上安装LDK、Nginx

## 一、安装JDK

### 第一步：检测系统中是否存在JDK

```shell
[root@chenstudy ~]# java --version
```

**显示：**

```shell
-bash: java: command not found
```

*说明系统中未安装JDK*

### 第二步：安装新的JDK

#### 2.1、下载JDK

**JDK的下载链接**：[https://www.oracle.com/cn/java/technologies/downloads/](https://www.oracle.com/cn/java/technologies/downloads/)

![image-20221008093059883](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221008093059883.png)

#### 2.2、使用xftp上传文件

![image-20221008093623993](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221008093623993.png)

![image-20221008093806726](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221008093806726.png)

#### 2.3、进入到目录，解压刚上传的文件

```shell
[root@chenstudy local]# cd jdk1.8/
[root@chenstudy jdk1.8]# ll
total 144692
-rw-r--r-- 1 root root 148162542 Oct  8 09:37 jdk-8u341-linux-x64.tar.gz
[root@chenstudy jdk1.8]# tar -zxvf jdk-8u341-linux-x64.tar.gz 
[root@chenstudy jdk1.8]# ll
total 144696
drwxr-xr-x 8 root root      4096 Oct  8 09:39 jdk1.8.0_341
-rw-r--r-- 1 root root 148162542 Oct  8 09:37 jdk-8u341-linux-x64.tar.gz
[root@chenstudy jdk1.8]# mv jdk1.8.0_341 jdk1.8 # 重命名
[root@chenstudy jdk1.8]# ll
total 144696
drwxr-xr-x 8 root root      4096 Oct  8 09:39 jdk1.8
-rw-r--r-- 1 root root 148162542 Oct  8 09:37 jdk-8u341-linux-x64.tar.gz
[root@chenstudy jdk1.8]# 
```

**得到Jdk目录路径/usr/local/jdk1.8/jdk1.8**

![](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221008094757567.png)

#### 2.4、给所有的用户配置java环境

**编辑 /etc/profile文件**

**按下i键，然后移动到最后一行，添加**

```shell
#configuration java development enviroument
export JAVA_HOME=/usr/local/jdk1.8/jdk1.8
export PATH=$JAVA_HOME/bin:$PATH
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
```

**然后按下esc键，输入:wq**

![image-20221008094733815](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221008094733815.png)

**再执行source /etc/profile之后输入 java -version如果能看到对应版本信息，则说明java环境变量配置成功。**

```shell
[root@chenstudy jdk1.8]# source /etc/profile
[root@chenstudy jdk1.8]# java -version
java version "1.8.0_341"
Java(TM) SE Runtime Environment (build 1.8.0_341-b10)
Java HotSpot(TM) 64-Bit Server VM (build 25.341-b10, mixed mode)
```

![image-20221008095114264](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221008095114264.png)

## 二、Nginx 安装

### 第一步：安装编译工具及库文件

```shell
[root@chenstudy src]# yum -y install make zlib zlib-devel gcc-c++ libtool  openssl openssl-devel
```

### 第二步、首先要安装 PCRE

**PCRE 作用是让 Nginx 支持 Rewrite 功能。**

#### 2.1、下载 PCRE 安装包，

PCRE下载地址： http://downloads.sourceforge.net/project/pcre/pcre/8.35/pcre-8.35.tar.gz

```shell
[root@chenstudy src]# cd /usr/local/src
[root@chenstudy src]# wget http://downloads.sourceforge.net/project/pcre/pcre/8.35/pcre-8.35.tar.gz
```

![image-20221008100357281](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221008100357281.png)

#### 2.2、解压安装包

```shell
[root@chenstudy src]# tar -zxvf pcre-8.35.tar.gz
```

![image-20221008100650114](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221008100650114.png)

#### 2.3、进入安装包目录

```shell
[root@chenstudy src]# cd pcre-8.35
```

#### 2.4、编译安装

```shell
[root@chenstudy pcre-8.35]# ./configure
[root@chenstudy pcre-8.35]# make && make install
```

![image-20221008101022851](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221008101022851.png)

#### 2.5、查看pcre版本

```shell
[root@chenstudy pcre-8.35]# pcre-config --version
8.35
```

![image-20221008101142325](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221008101142325.png)

### 第三步：安装nginx

#### 3.1、下载nginx

下载地址：https://nginx.org/en/download.html

```shell
[root@chenstudy pcre-8.35]# cd /usr/local/src/
[root@chenstudy src]# wget wget http://nginx.org/download/nginx-1.22.0.tar.gz
```

![image-20221008101538710](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221008101538710.png)

#### 3.2、解压安装包

```shell
[root@chenstudy src]# tar -zxvf nginx-1.22.0.tar.gz
```

![image-20221008101704182](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221008101704182.png)

#### 3.3、进入安装包目录

```shell
[root@chenstudy src]# cd nginx-1.22.0
```

#### 3.4、编译安装

```shell
[root@chenstudy nginx-1.22.0]# ./configure --prefix=/usr/local/webserver/nginx --with-http_stub_status_module --with-http_ssl_module --with-pcre=/usr/local/src/pcre-8.35
[root@chenstudy nginx-1.22.0]# make && make install
```

**到此，nginx安装完成**

## 第三步：Nginx 配置

### 第一步：创建 Nginx 运行使用的用户 www：

```shell
[root@chenstudy nginx-1.22.0]# cd /usr/local/webserver/nginx/conf/nginx.conf
[root@chenstudy conf]# /usr/sbin/groupadd www
[root@chenstudy conf]# /usr/sbin/useradd -g www www
```

![image-20221008102549221](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221008102549221.png)

### 第二步：配置nginx.conf 

**将/usr/local/webserver/nginx/conf/nginx.conf替换为以下内容：**

```shell
[root@chenstudy conf]# cat nginx.conf

user  www www;
worker_processes  1;

error_log  /usr/local/webserver/nginx/logs/nginx_error.log crit; # 日志位置和日志级别
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

pid        /usr/local/webserver/nginx/nginx.pid;
#Specifies the value for maximum file descriptors that can be opened by this process.
worker_rlimit_nofile 65535;

events {
    use epoll;
    worker_connections  65535;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    server_names_hash_bucket_size 128;
    client_header_buffer_size 32k;
    large_client_header_buffers 4 32k;
    client_max_body_size 8m;
     
    sendfile on;
    tcp_nopush on;
    keepalive_timeout 60;
    tcp_nodelay on;
    fastcgi_connect_timeout 300;
    fastcgi_send_timeout 300;
    fastcgi_read_timeout 300;
    fastcgi_buffer_size 64k;
    fastcgi_buffers 4 64k;
    fastcgi_busy_buffers_size 128k;
    fastcgi_temp_file_write_size 128k;
    gzip on; 
    gzip_min_length 1k;
    gzip_buffers 4 16k;
    gzip_http_version 1.0;
    gzip_comp_level 2;
    gzip_types text/plain application/x-javascript text/css application/xml;
    gzip_vary on;

    server {
        listen       80;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            root   html;
            index  index.html index.htm;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }


    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}


    # HTTPS server
    #
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;

    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;

    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;

    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}

}

```

**检查配置文件nginx.conf的正确性命令：**

```shell
[root@chenstudy conf]# /usr/local/webserver/nginx/sbin/nginx -t
```

![image-20221008104143473](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221008104143473.png)

### 第三步：启动 Nginx

**Nginx 启动命令如下：**

```shell
[root@chenstudy conf]# /usr/local/webserver/nginx/sbin/nginx
[root@chenstudy conf]# ps -ef | grep nginx
root      5820     1  0 10:45 ?        00:00:00 nginx: master process /usr/local/webserver/nginx/sbin/nginx
www       5821  5820  0 10:45 ?        00:00:00 nginx: worker process
root      5851 24210  0 10:46 pts/0    00:00:00 grep --color=auto nginx
```

![image-20221008104623935](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221008104623935.png)

**访问站点：**

![image-20221008104811247](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221008104811247.png)

## Nginx 其他命令

**以下包含了 Nginx 常用的几个命令：**

```shell
/usr/local/webserver/nginx/sbin/nginx -s reload            # 重新载入配置文件
/usr/local/webserver/nginx/sbin/nginx -s reopen            # 重启 Nginx
/usr/local/webserver/nginx/sbin/nginx -s stop              # 停止 Nginx
```

# 线上部署项目

```shell
tailf nohup.out

nohup java -jar demo-0.0.1-SNAPSHOT.jar --spring.profiles.active=prod &

location / {
            root   /home/user/server/dist;
            index  index.html index.htm;
            try_files $uri $uri/ index.html;
        }

location /api {
        proxy_pass http://localhost:9090/;
    }    
```

# Linux上安装文件服务器FTP

由于FTP、HTTP、Telnet等协议的数据都是使用明文进行传输的，因此从设计上就是不可靠的。人们为了满足以密文方式传输文件的需求，发明了vsftpd服务程序。vsftpd（very secure ftp daemon，非常安全的FTP守护进程）是一款运行在Linux操作系统上的FTP服务程序，不仅完全开源而且免费。此外，它还具有很高的安全性、传输速度，以及支持虚拟用户验证等其他FTP服务程序不具备的特点。在不影响使用的前提下，管理者可以自行决定客户端是采用匿名开放、本地用户还是虚拟用户的验证方式来登录vsftpd服务器。这样即便黑客拿到了虚拟用户的账号密码，也不见得能成功登录vsftpd服务器。

## 安装VSFTP

### 下载dnf

```sh
[root@chenstudy ~]# yum install epel-release
```

![image-20221212151043438](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221212151043438.png)

```sh
[root@chenstudy ~]# yum install dnf
```

![image-20221212151400707](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221212151400707.png)

![image-20221212151435242](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221212151435242.png)

### 下载VSFTP

```sh
[root@chenstudy ~]# dnf install vsftpd
```

![image-20221212151836706](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221212151836706.png)

### 清除防火墙的iptables缓存

iptables防火墙管理工具默认禁止了FTP协议的端口号，因此在正式配置vsftpd服务程序之前，为了避免这些默认的防火墙策略“捣乱”，还需要清空iptables防火墙的默认策略，并把当前已经被清理的防火墙策略状态保存下来：

```sh
[root@chenstudy ~]# iptables -F
[root@chenstudy ~]# iptables-save
```



![image-20221212152116686](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221212152116686.png)

**然后再把FTP协议添加到firewalld服务的允许列表中（前期准备工作一定要做充足）:**

```sh
[root@chenstudy ~]# firewall-cmd --permanent --zone=public --add-service=ftp
success
[root@chenstudy ~]# firewall-cmd --reload
success
```

![image-20221212152751773](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221212152751773.png)

**查看vsftpd服务程序的主配置文件（/etc/vsftpd/vsftpd.conf）：**

```sh
[root@chenstudy ~]# mv /etc/vsftpd/vsftpd.conf /etc/vsftpd/vsftpd.conf_bak
[root@chenstudy ~]# grep -v "#" /etc/vsftpd/vsftpd.conf_bak  > /etc/vsftpd/vsftpd.conf
[root@chenstudy ~]# cat /etc/vsftpd/vsftpd.conf
anonymous_enable=YES
local_enable=YES
write_enable=YES
local_umask=022
dirmessage_enable=YES
xferlog_enable=YES
connect_from_port_20=YES
xferlog_std_format=YES
listen=NO
listen_ipv6=YES
pam_service_name=vsftpd
userlist_enable=YES
tcp_wrappers=YES
[root@chenstudy ~]# 

```

![image-20221212153251812](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221212153251812.png)

   																	 **vsftpd服务程序常用的参数以及作用**

|                       参数                        |                             作用                             |
| :-----------------------------------------------: | :----------------------------------------------------------: |
|                 listen=[YES\|NO]                  |                 是否以独立运行的方式监听服务                 |
|               listen_address=IP地址               |                      设置要监听的IP地址                      |
|                  listen_port=21                   |                    设置FTP服务的监听端口                     |
|            download_enable＝[YES\|NO]             |                       是否允许下载文件                       |
| userlist_enable=[YES\|NO] userlist_deny=[YES\|NO] |              设置用户列表为“允许”还是“禁止”操作              |
|                   max_clients=0                   |                 最大客户端连接数，0为不限制                  |
|                   max_per_ip=0                    |              同一IP地址的最大连接数，0为不限制               |
|            anonymous_enable=[YES\|NO]             |                     是否允许匿名用户访问                     |
|           anon_upload_enable=[YES\|NO]            |                   是否允许匿名用户上传文件                   |
|                  anon_umask=022                   |                  匿名用户上传文件的umask值                   |
|                anon_root=/var/ftp                 |                     匿名用户的FTP根目录                      |
|         anon_mkdir_write_enable=[YES\|NO]         |                   是否允许匿名用户创建目录                   |
|         anon_other_write_enable=[YES\|NO]         | 是否开放匿名用户的其他写入权限（包括重命名、删除等操作权限） |
|                  anon_max_rate=0                  |         匿名用户的最大传输速率（字节/秒），0为不限制         |
|              local_enable=[YES\|NO]               |                   是否允许本地用户登录FTP                    |
|                  local_umask=022                  |                  本地用户上传文件的umask值                   |
|                local_root=/var/ftp                |                     本地用户的FTP根目录                      |
|            chroot_local_user=[YES\|NO]            |           是否将用户权限禁锢在FTP目录，以确保安全            |
|                 local_max_rate=0                  |          本地用户最大传输速率（字节/秒），0为不限制          |

## 下载FTP

vsftpd作为更加安全的文件传输协议服务程序，允许用户以3种认证模式登录FTP服务器。

- **匿名开放模式**：是最不安全的一种认证模式，任何人都可以无须密码验证而直接登录到FTP服务器。

- **本地用户模式**：是通过Linux系统本地的账户密码信息进行认证的模式，相较于匿名开放模式更安全，而且配置起来也很简单。但是如果黑客破解了账户的信息，就可以畅通无阻地登录FTP服务器，从而完全控制整台服务器。

- **虚拟用户模式**：更安全的一种认证模式，它需要为FTP服务单独建立用户数据库文件，虚拟出用来进行密码验证的账户信息，而这些账户信息在服务器系统中实际上是不存在的，仅供FTP服务程序进行认证使用。这样，即使黑客破解了账户信息也无法登录服务器，从而有效降低了破坏范围和影响。

**ftp是Linux系统中以命令行界面的方式来管理FTP传输服务的客户端工具。我们首先手动安装这个ftp客户端工具：**

```sh
[root@chenstudy ~]# dnf install ftp
```

![image-20221212153835464](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221212153835464.png)

### 匿名访问模式

vsftpd服务程序中，匿名开放模式是最不安全的一种认证模式。任何人都可以无须密码验证而直接登录FTP服务器。这种模式一般用来访问不重要的公开文件（在生产环境中尽量不要存放重要文件）。当然，如果采用第8章中介绍的防火墙管理工具（如TCP Wrapper服务程序）将vsftpd服务程序允许访问的主机范围设置为企业内网，也可以提供基本的安全性。

vsftpd服务程序默认关闭了匿名开放模式，我们需要做的就是开放匿名用户的上传、下载文件的权限，以及让匿名用户创建、删除、更名文件的权限。需要注意的是，针对匿名用户放开这些权限会带来潜在危险，我们只是为了在Linux系统中练习配置vsftpd服务程序而放开了这些权限，不建议在生产环境中如此行事。表11-2罗列了可以向匿名用户开放的权限参数以及作用。

​															                向匿名用户开放的权限参数以及作用

| 参数                        | 作用                               |
| --------------------------- | ---------------------------------- |
| anonymous_enable=YES        | 允许匿名访问模式                   |
| anon_umask=022              | 匿名用户上传文件的umask值          |
| anon_upload_enable=YES      | 允许匿名用户上传文件               |
| anon_mkdir_write_enable=YES | 允许匿名用户创建目录               |
| anon_other_write_enable=YES | 允许匿名用户修改目录名称或删除目录 |

**配置vsftp配置文件：**

```sh
[root@chenstudy ~]# vim /etc/vsftpd/vsftpd.conf
# 重启vsftp
[root@chenstudy ~]# systemctl restart vsftpd
# 把vsftp加入开机自启动
[root@chenstudy ~]# systemctl enable vsftpd
Created symlink from /etc/systemd/system/multi-user.target.wants/vsftpd.service to /usr/lib/systemd/system/vsftpd.service.
[root@chenstudy ~]# 
```



![image-20221212160751359](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221212160751359.png)

**在linux中采用匿名访问ftp**

```sh
[root@chenstudy ~]# ftp 192.168.200.130
Connected to 192.168.200.130 (192.168.200.130).
220 (vsFTPd 3.0.2)
Name (192.168.200.130:root): anonymous
331 Please specify the password.
Password: 敲回车
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> cd pub
250 Directory successfully changed.
ftp> mkdir files
550 Create directory operation failed.
ftp> 
# 退出ftp客户端，修改所有者身份
[root@chenstudy ~]# ls -ld /var/ftp/pub
drwxr-xr-x. 2 root root 6 Jun 10  2021 /var/ftp/pub
[root@chenstudy ~]# chown -R ftp /var/ftp/pub
[root@chenstudy ~]# ls -ld /var/ftp/pub
drwxr-xr-x. 2 ftp root 6 Jun 10  2021 /var/ftp/pub
[root@chenstudy ~]# 
```

![image-20221212161200821](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221212161200821.png)

系统提示“创建目录的操作失败”（Create directory operation failed），我猜应该是SELinux服务在“捣乱”

```sh
[root@chenstudy ~]# getsebool -a | grep ftp
ftpd_anon_write --> off
ftpd_connect_all_unreserved --> off
ftpd_connect_db --> off
ftpd_full_access --> off
ftpd_use_cifs --> off
ftpd_use_fusefs --> off
ftpd_use_nfs --> off
ftpd_use_passive_mode --> off
httpd_can_connect_ftp --> off
httpd_enable_ftp_server --> off
tftp_anon_write --> off
tftp_home_dir --> off
[root@chenstudy ~]# setsebool -P ftpd_full_access=on
```

![image-20221212161449935](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221212161449935.png)

**SELinux域策略就可以顺利执行文件的创建、修改及删除等操作了：**

![image-20221212162002322](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221212162002322.png)

### 本地用户模式

本地用户模式要更安全，而且配置起来也很简单

 本地用户模式使用的权限参数以及作用

|        参数         |                       作用                        |
| :-----------------: | :-----------------------------------------------: |
| anonymous_enable=NO |                 禁止匿名访问模式                  |
|  local_enable=YES   |                 允许本地用户模式                  |
|  write_enable=YES   |                   设置可写权限                    |
|   local_umask=022   |           本地用户模式创建文件的umask值           |
|  userlist_deny=YES  | 启用“禁止用户名单”，名单文件为ftpusers和user_list |
| userlist_enable=YES |             开启用户作用名单文件功能              |

**修改vsftp的配置文件：**

```sh
[root@chenstudy ~]# vim /etc/vsftpd/vsftpd.conf
	anonymous_enable=NO
	local_enable=YES
	write_enable=YES
	local_umask=022
	dirmessage_enable=YES
	xferlog_enable=YES
	connect_from_port_20=YES
	xferlog_std_format=YES
	listen=NO
	listen_ipv6=YES
	
	pam_service_name=vsftpd
	userlist_enable=YES
	tcp_wrappers=YES
```

![image-20221212162723751](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221212162723751.png)

**在我们输入root管理员的密码之前，就已经被系统拒绝访问了。这是因为vsftpd服务程序所在的目录中默认存放着两个名为“用户名单”的文件（ftpusers和user_list）:vsftpd服务程序目录中的这两个文件也有类似的功能—只要里面写有某位用户的名字，就不再允许这位用户登录到FTP服务器上。**

```sh
[root@chenstudy ~]# cat /etc/vsftpd/user_list 
# vsftpd userlist
# If userlist_deny=NO, only allow users in this file
# If userlist_deny=YES (default), never allow users in this file, and
# do not even prompt for a password.
# Note that the default vsftpd pam config also checks /etc/vsftpd/ftpusers
# for users that are denied.
root
bin
daemon
adm
lp
sync
shutdown
halt
mail
news
uucp
operator
games
nobody
[root@chenstudy ~]# cat /etc/vsftpd/ftpusers
# Users that are not allowed to login via ftp
root
bin
daemon
adm
lp
sync
shutdown
halt
mail
news
uucp
operator
games
nobody
[root@chenstudy ~]# 
```

![image-20221212162653761](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221212162653761.png)

**我们可以使用普通用户登录vsftp服务器：**

```sh
[root@chenstudy ~]# ftp 192.168.200.130
Connected to 192.168.200.130 (192.168.200.130).
220 (vsFTPd 3.0.2)
Name (192.168.200.130:root): chen             
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> 
```

![image-20221212163228776](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/image-20221212163228776.png)

## 
