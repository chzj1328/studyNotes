# Linux

## 1、vi和vim

```bash
vi 进入文本编译器
vim 具有程序的编译能力，可以主动的以字体的颜色辨别语法的正确性
vim xxx 进入编辑模式
按下i、I、o、O、a、A、r、R都会进入到编辑模式
按ESC会退出编译模式
:wq(保存并退出) :q(退出) :q!(强制退出不保存)
```

## 2、关机和重启命令

```bash
shutdown -h now   立刻进行关机
shutdown -h 1     "hello,1分钟后会关机了"
shutdown -r now   现在重新启动计算机
halt              关机
reboot            现在重新启动计算机
sync              把内存的数据同步到磁盘
注意：不管是重启系统还是关闭系统，首先要运行sync命令，把内存中的数据写到磁盘中。
```

## 3、用户管理

```bash
useradd 用户名
passwd 用户名
pwd 显示当前用户在哪一个目录下面
userdel 删除用户
userdel -r 用户名 把根目录也删除了
id 用户名 查询用户名信息
su - 用户名 切换到指定用户
who am i 显示你第一次登录到的地址
groupadd 组名  添加组
groupadd 组名  删除组
useradd -g 用户组 用户名
usermod -g 新组名 用户名
```

## 4、用户和组相关文件

```bash
/etc/passwd 文件
	用户(user)的配置文件，记录用用户的各种信息
	每行的含义：用户名:口令;用户标识号:组标识号:注释性描述:主目录:登录shell
/etc/shadow 文件
	口令配置文件
	每行的含义:登录名:加密口令:最后一次修改时间:最小时间间隔:警告时间:不活动时间:失效时间:标志
/etc/group 文件
	组(group)的配置文件，记录Linux包含的组的信息
	每行含义：组名:口令:组标识号:z
```

## 5、运行级别

```bash
0:关机
3:多用户状态有网络服务
5:图形界面
3和5时常用的运行级别
init[0123456] 通过init来切换不同的运行级别
```

## 6、找回root密码

```bash
1、重启虚拟机在选系统界面按e键
2、找到UTF-8 在后面添加init=/bin/sh 按Ctrl+x
3、在光标闪烁位置输入:mount -o remount,rw /
4、在新的一行输入:passwd
5、重新输入新的密码即可
6、在新的一个命令行输入:touch /.autorelabel
7、在新的一个命令行输入:exec /sbin/init
8、等待重新启动即可
```

## 7、帮助指令

```bash
man [命令或配置文件] 获取当前命令或配置文件的帮助信息
help 命令  获得shell内置命令的帮助信息
```

## 8、文件目录指令

``` bash
pwd  显示当前工作目录的绝对路径
ls 指令
ls -a 可以查看隐藏文件
ls -l 以列表的方式显示信息
ls -al /root 可以看root文件下的所有文件
ls -lh 更人性化的显示文件的大小

cd 指令
cd [参数] 切换到指定的目录
cd ~、cd: 回到自己的家目录
cd ..:回到当前目录的上一级目录
cd ../../root 以相对路径的方式回到root目录
cd /root 使用绝对路径到/root目录

mkdir指令用于创建目录
mkdir [选项] 要创建的目录
mkdir -p /home/animal/tiger 创建多级目录
mkdir /home/dog

rmdir指令用于删除空目录
rmdir [选项] 要删除的空目录
rmdir /home/dog
如需要删除非空目录则使用
rm -rf /home/animal/  注意：删除时一定要小心谨慎

touch 指令用于创建一个空文件
touch 文件名称

cp指令拷贝文件到指定目录
cp [选项] source dest source:源文件 dest:要拷贝到的文件位置
cp /home/hello.txt /home/bbb
cp -r /home/bbb/ /opt/
\cp cp -r /home/bbb/ /opt/ 跳过提示强制覆盖

rm指令移除文件或目录
rm [选项] 要删除的文件或目录
常用选项 -r:递归删除真个文件夹 -f:强制删除不提示
rm /home/hello.txt
rm -rf /home/bbb/ 

mv指令用于移动文件与目录或重命名
mv oldNameFile newNameFile 重命名
mv /temp/movefile/targetFolder 移动文件
mv pig.txt /home/cat.txt 移动文件并重命m
mv /home/bbb /home/uuu 移动整个目录并且重命名

cat指令查看文件内容
cat [选项] 要查看的文件
cat -n /etc/profile 显示行号
| more 管道命令
cat -n /etc/profile | more

less指令用来查看大文件
less 文件名
空白键       向下翻动一页
[pagedown]  向下翻动一页
[pageup]    向上翻动一页
/字串        向下搜寻字串的功能:n向下查找;N向上查找;
?字串        向上搜寻字串的功能:n向下查找;N向上查找;
q            离开less这个程序

echo指令 echo输出内容到控制台
echo [选项] [输出内容]
echo $PATH
echo $HOSTNAME
tail -f 文件名  可以实时监控某个文件的状态
>和>>指令是重定向和追加
ls -l > 文件 覆盖重写该文件
ls -al >> 文件 列表的内容追加到文件的末尾
cat 文件1 > 文件2 将文件1的内容覆盖到文件2
echo "内容" >> 文件
cal 当前日期

ln指令软连接也称为符号链接
ln -s [原文件或目录] [软连接名] 给原文件创建一个软链接
history	指令查看执行过的历史命令
!数字 执行曾经执行过的指令
```

## 9、时间日期类

```bash
date指令-显示当前日期
date		显示当前时间
date "+%Y"   显示当前年份
date "+%m"   显示当前月份
date "+%d"   显示当前是哪一天
date "+%Y-%m-%d %H:%M:%S"  显示年月时分秒
date -s 字符串时间  设置系统的当前的时间 
cal 查看日历的指令
cal 2020 显示2020年整年的指令
```

## 10、搜索查找类

```bash
find指令
find [搜索范围] [选项]
find -name<查询方式>   按照指定的文件名查找文件
find -user<用户名>     查找属于指定用户名所有文件
find -size<文件大小>    按照指定的文件大小查找文件 (+n大于;-n小于;n等于单位有K,M,G) 

locate 指令可以快速定位文件路径
locate 搜索文件 说明:第一次使用locate指令时，必须使用updatedb指令创建locate数据库
which 指令可以查看某个指令在那个目录下  

grep和管道符号 |
grep过滤查找，管道符,"|" 表示将前一个命令的处理结果输出传递给后面的命令处理
grep -n(显示匹配行及行号) 查找内容 源文件
grep -i(忽略字母大小写) 查找内容 源文件
cat /home/hello.txt | grep "yes"
grep -n "yes" /home/hello.txt
```

## 11、压缩和解压类

```bash
gzip/gunzip指令
gzip 用于压缩文件
gunzip 用于解压文件 
```

![image-20211117164627787](C:\Users\10655\AppData\Roaming\Typora\typora-user-images\image-20211117164627787.png)

```bash
zip/unzip指令
zip -r xxx  压缩文件和目录的命令
unzip -d xxx 解压文件或目录命令
tar [选项] xxx.tar.gz 打包的文件 
选项说明:
	-c  产生.tar打包文件
	-v  显示详细信息
	-f  指定压缩后的文件名
	-z  打包同时压缩
	-x  解包.tar文件
```

## 12、组和权限管理

```bash
groupadd 组名 组的创建
groupadd monster
useradd -g monster fox
ls -ahl 查看文件目录所在组
chgrp 组名 文件名  修改文件所在组
usermod -g 组名 用户名 改变用户所在组
usermod -d 目录名 用户名 改变该用户登陆的初始目录
权限管理
0位
	l 是链接
	d 是目录
	c 是字符设备文件，鼠标，键盘
	b 是块设备
1-3位
	确定所有者拥有该文件的权限   --User
4-6位
	确定所属组拥有该文件的权限   --Group
7-9位
	确定其他用户拥有该文件的权限  --Other
rwx权限详解
rwx作用到文件
	1、r代表可以读取，查看
	2、w代表可以修改但不代表可以删除
	3、x代表可执行
rwx作用到目录
	1、r代表可以读取，ls查看目录内容
	2、w代表可以修改对目录内创建+删除+重命名目录
	3、x代表可以进入该目录
修改权限-chmod
u:所有者 g:所有组 o:其他人 a:所有人
chmod u=rwx,g=rx,o=x 文件/目录名
chmod o+w 文件/目录名
chmod a-x 文件/目录名
第二种方式
	r=4 w=2 r=1 rwx=4+2+1=7
	chmod u=rwx,g=rx,o=x 文件名
	相当于 chmod 751 文件名
chown newowner 文件/目录	改变所有者
chown newowner:newgroup 文件/目录	改变所有者和所在组
-R表示该目录下所有子文件或目录递归生效
chgrp newgroup 文件目录		改变所在组
```

## 13、crond任务调度

```bash
任务调度:是指系统在某个时间执行的特定的命令或程序
任务调度的分类:1、系统工作:有些重要的工作必须周而复始地执行，如病毒扫描等
			 2、个别用户工作:个别用户可能希望执行某些程序，比如对mysql数据库的备份
crontab 进行定时任务的设置
crontab -e 编辑crontab定时任务
crontab -l 查询crontab任务
crontab -r 删除当前用户所有的crontab任务
crotab  -u 指定要设定计时器的用户名称

设置任务调度文件:/etc/crontab
设置个人任务调度。执行crontab -e命令
如:*/1 * * * * ls -l /etc/ > /tem/to.txt
第一个*:一小时当中的第几分钟
第二个*:一天当中的第几个小时
第三个*:一个月当中的地几个月
第四个*:一年当中的第几个月
第五个*:一周当中的星期几

*代表任何时间
,代表不连续的时间 "0 8,12,16命令"，就代表在每天的八点十二点和16点个执行一次
-代表连续的时间范围 "0 5 * * 1-6命令"就代表在周一到周六的五点执行
*/n代表每隔多久执行一次

crontab相关指令
crontab -t:终止任务调度
crontab -l:列出当前有那些任务调度
service crond restart [重启任务调度]

每隔一分钟将当前日期和日历都追加到/home/mycal文件中
vim /home/my.sh
写入内容 date >> /home/mycal
		cal >> /home/mycal
增加权限：chmod u+x /home/my.sh
crontab -e
*/1 * * * * /etc/ > /tmp/to.txt
*/1 * * * * /home/my.sh

每天凌晨2点将mysq数据库testdb,备份到文件中。
(1)crontab -e
(2)0 2 * * * mysqldump -uroot -proot > /home/db.bak

at定时任务
at命令是一次性定时计划任务，at的守护进程atd会以后台模式运行，检查队列来运行。
在使用at命令的时候，一定要保证atd进程的启动可以使用 ps-ef查看当前所运行的进程有那些
ps -ef | grep at 可以检测atd是否在运行
at [选项] [时间]   Ctrl + D 结束at命令的输入
-m 当指定的任务被完成后，将给用户发送邮件，
-I atq的别名
-d atrm的别名
-v 显示任务呗执行的时间
-c 打印任务的内容到标准输出
-V 显示版本信息
-q<队列> 使用指定的队列
-f<文件> 从指定文件读入任务而不是从标准输入读入
-t<时间参数> 以时间参数的形式提交要运行的任务

案例：
1、两天后的下午5点执行 /bin/ls /home
at 5pm + 2 days
at> /bin/ls /home 输入两次Ctrl + D
atq 查看系统中有没有执行的工作任务
2、明天17点，输出时间到指定的文件内，如 /root/date100.log
at 5pm tomorrow
at> date > /root/date100.log 输入两次Ctrl + D
3、2分钟后，输出时间到指定的文件内 比如 /root/date200.log
at now + 2 minutes
at> date > /root/date200.log
atrm + 数字   可以删除已设置的任务
```

## 14、Linux分区

```bash
lsblk 或 lsblk -f 查看所有设备挂在情况
```

![image-20211122150040286](C:\Users\10655\AppData\Roaming\Typora\typora-user-images\image-20211122150040286.png)

>  永久挂载

```bash
修改etc/fstab文件下的
/dev/sdbl  /newdisk  ext4  default    0 0 
添加成功后执行mount -a即刻生效
```

> 查询磁盘的使用情况

```bash
df -h // 查询磁盘使用情况
du -h // 查询指定目录的磁盘占用情况，默认当前目录
-s 指定目录占用大小汇总
-h 带计量单位
-a 含文件
--max-depth=1 // 子目录深度
-c // 列出明细的同时，增加汇总值
```

![image-20211221192749002](C:\Users\10655\AppData\Roaming\Typora\typora-user-images\image-20211221192749002.png)

> 工作实用指令

```bash
1.统计/opt文件夹下文件的个数
ls -l /opt | grep "^_" | wc l
2.统计/opt文件夹下目录的个数
ls -l /opt | grep "^d" | wc l
3.统计/opt文件夹下文件的个数，包括子文件夹里的
ls -lR /opt | grep "^_" | wc l
4.统计/opt文件夹下的目录个数，包括子文件夹里的
ls -lR /opt | grep "^d" | wc l
5.树状显示目录结构
先下载tree 用yum install tree
tree /home/
```

> 卸载文件系统

1、umount是“unmount”的缩写，译为“不挂在。所以它的”的作用是卸载已安装的文件系统、目录或文件。使用umount命令可以卸载文件系统。利用设备名或挂载点都能umount文件系统，不过最好还是通过挂载点卸载，一面使用绑定挂在（一个设备，多个挂载点）时产生混乱。

```bash
语法格式：umount [参数]

常用参数：

-a	#卸载/etc/mtab中记录的所有文件系统
-h	#显示帮助
-n	#卸载时不要将信息存入/etc/mtab文件中
-r	#若无法成功卸载，则尝试以只读的方式重新挂入文件系统
-t	#文件系统类型：仅卸载选项中所指定的文件系统
-v	#执行时显示详细的信息
-V	#显示版本信息
```

**参考实例**

通过设备名卸载：

```bash
[root@linuxcool ~] umount -v  /dev/sda1
/dev/sda1 umounted 
```

通过挂载点卸载：

```bash
umount -v /mnt/mymount/
/tem/diskboot.img umounted 
```

对系统文件正忙时执行延时卸载：

```bash
umount -v1 /mnt/mymount/ 
```

卸载挂载在/media/E_pan目录下的文件系统：

```bash
umount /media/E_pan 
```

卸载文件和目录：

```bash
umount /home/user/test 
```



## 15、Linux网络环境配置

### 15.1、**直接修改配置文件来指定IP，并可以连接到外网**

```bash
编辑   vim /etc/sysconfig/network-scripts/ifcfg-ens33
要求：将ip地址配置成静态的，比如IP地址为192.168.200.130
重启网络服务或者重启系统生效
service network restart
reboot
```

> 设置主机名

1.为了方便记忆，可以给Linux系统设置主机名，也可以根据需要修改主机名

```bash
hostname #查看主机名称
#修改文件在/etc/hostname指定
#修改后，重启生效
```

> 设置hosts映射

**windows**

在C:\Windows\System32\drivers\etc\hosts文件指定即可

案例：192.168.200.130  chStudy

**Linux**

在/etc/hosts文件指定

案例:192.168.200.1  chenhao 

### 15.2、服务(service)管理

#### 15.2.1、**服务（service）本质就是进程，在后台监听某个端口。**

```bash
#service 管理指令
service 服务名 [start | stop | restart | reload | status]
#在CentOS7.0之后 很多服务不再使用service，而是systemctl
#service指令管理的服务在/etc/init.d 查看
setup  #查看所有的服务名称
```

#### 15.2.2、**服务的运行级别（runlevel）**

+ 运行级别3：完全多用户状态（有NFS），无界面，登陆后进入控制台命令行模式
+ 运行级别５：X11控制台，登录后进入图形GUI模式

```bash
#获取当前的运行级别
systemctl get-default
#修改运行级别
systemctl set-default multi-user.target
multi-user.target:analogous to runlevel 5
graphical.target:analogous to runlevel 3
```

#### 15.2.3、chkconfig指令

#### **通过chkconfig指令可以给服务的各个运行级别设置自启动/关闭**

```bash
#chkconfig基本语法
#查看服务
chkconfig --list [| grep XXX]
chkconfig 服务名 --list
chkconfig --level 5 服务名 on/off
```

#### 15.2.4、systemctl管理指令

基本语法：systemctl [start | stop | restart | status] 服务名

systemctl 指令管理的服务在 /usr /lib/systemd/system 查看

**systemctl设置服务的自启动状态**

```bash
systemctl list-unit-files [| grep 服务名] #（查看服务开机启动状态，grep进行过滤）
systemctl enable 服务名                   #（设置服务开机启动）
systemctl disable 服务名 				   #（关闭服务开机启动）
systemctl is-enabled 服务名   			 #（查询某个服务是否是自启动的）
```

#### 15.2.5、打开或关闭指定端口

**firewall指令**

```shell
firewall-cmd --permanent --add-port=端口/协议  # 打开端口
firewall-cmd --permanent --remove-port=端口/协议 # 关闭端口
firewall-cmd --reload # 重新载入，才能生效
firewall-cmd --query-port=端口/协议 # 查看端口是否开放
```

### 15.3、动态监控进程

**top指令：top在执行一段时间可以更新正在运行的进程**

`语法：[选项]`

```shell
top -d 秒数 # 指定top命令每隔几秒更新，默认是3秒。
top -i # 使top不显示任何闲置或者僵死的进程
top -p # 通过指定监控进程ID来仅仅监控某个进程的状态
```

**交互操作说明：**

- P             以CPU使用率排序，默认就是此选项

- M            以内存的使用率排序

- N             以PID排序

- q              退出top

  

```shell
# 监控特定的用户
top # 查看执行的进程
u   # 回车，再输入用户名，即可

# 终止指定的进程
top # 查看执行的进程
k # 回车，再输入要结束的进程的ID号
```

## 16、RPM与YUM指令

### 16.1、rpm管理指令

**rpm包的简单查询指令**

```shell
rpm -qa|grep xx   #查询已安装的rpm列表
```

**rpm包的其他查询指令：**

```shell
rpm -qa #查看已安装的所有的rpm软件包
rpm -qa | more
rpm -qa | grep X
rpm -q 软件包名 # 查询软件是否安装
rpm -qi 软件包名 # 查询软件包信息
rpm -ql 软件包名 # 查询软件包中的文件
rpm -qf 文件全路径名 # 查询文件所属的软件包
```

**rpm卸载**

```shell
rpm -e RPM包的名称
# 提示错误是在后面增加参数 --nodeps,进行强制删除
rpm -e --nodeps RPM包的名称
```

**安装rpm包**

```shell
rpm -ivh RPM包的全路径名称
# i = install 安装
# v = verbose 提示
# h = hash 进度条
```

### 16.2、yum管理指令

**yum介绍**

`yum是一个Shell前端软件安装包管理器。能够从指定的服务器自动下载RPM包且安装，可以自动处理依赖性关系，且一次性安装所有的软件包`

**yum的基本指令**

```shell
# 查询yum服务器是否有需要安装的软件
yum list|grep xx软件列表
# 安装指定的yum包
yum install xxx # 下载安装
```

## 17、安装JavaEE开发环境

### 17.1、安装JDK

**安装步骤：**

- 创建目录 
  - **mkdir /opt/jdk**

- 将JDK文件通过xftp上传到`/opt/jdk`文件夹下
- 进入到jdk目录
  - **cd /opt/jdk**
- 解压 `tar -zvxf jdk-8u321-linux-x64.tar.gz`
- 创建目录 
  - **mkdir /usr/local/java**
- 移动刚解压的文件
  - **mv /opt/jdk/jdk1.8.0_321 /usr/local/java**
- 配置环境变量的文件
  - **vim /etc/profile**
  - **export JAVA_HOME=/usr/local/java/jdk1.8.0_321**
  - **export PATH = $JAVA_HOME/bin:$PATH**

- 让文件生效

  - **source  /etc/profile**


### 17.2、安装Tomcat

**安装步骤：**

- 上传文件，并解压缩到`/opt/tomcat`
- 进入解压目录/bin，启动tomcat `./startup.sh`
- 开放端口 8080

### 17.3、安装mysql

**安装步骤**

#### 1、新建文件夹

```shell
mkdir /opt/mysql
# 进入到文件夹中
cd /opt/mysql
```

#### 2、上传下载的文件到/opt/mysql

```shell
# 通过xftp7进行文件的上传
```

#### 3、解压刚上传的文件

```shel
tar -xvf mysql-5.7.37-1.el7.x86_64.rpm-bundle.tar 
```

#### 4、查询mariadb相关安装包

```shell
rpm -qa | grep mari
```

#### 5、开始安装MySQL

**依次执行如下命令：**

```shell
rpm -ivh mysql-community-common-5.7.37-1.el7.x86_64.rpm 
rpm -ivh mysql-community-libs-5.7.37-1.el7.x86_64.rpm 
rpm -ivh mysql-community-client-5.7.37-1.el7.x86_64.rpm
rpm -ivh mysql-community-server-5.7.37-1.el7.x86_64.rpm 
```

#### 6、启动MySQL

```shell
systemctl start mysqld.service
```

#### 7、设置root用户密码

- 查看初始密码

```shell
cat /var/log/mysqld.log
# 或
grep "password" /var/log/mysqld.log
```

#### 8、进入到MySQL并设置root密码

```sql
mysql -uroot -p*****

# 设置root密码
set password for 'root'@'localhost' = password('*****');
# 刷新权限
flush privileges;
```

## 18、Shell编程

### 18.1、Shell脚本的执行方式

- 格式要求
  - 脚本要以#!/bin/bash开头
  - 脚本要有可执行权限

- 脚本的常用执行方式
  - 方式一：输入脚本的绝对路径或相对路径
    - 说明：首先要赋予helloworld.sh 脚本的+x权限,再执行脚本
  - 方式二：sh + 脚本
    - 不用赋予脚本+x权限,直接执行即可

**第一个Shell程序hello.sh**

```sh
#!/bin/bash
echo "hello,world~"
```

**执行该Shell脚本**

```bash
# 增加权限
chmod u+x hello,sh
# 执行该脚本
./hello,sh

# 在没有权限时可以用方式二
sh hello.sh
```

![image-20220221173327917](C:\Users\10655\AppData\Roaming\Typora\typora-user-images\image-20220221173327917.png)

### 18.2、Shell的系统变量

#### 18.2.1、Linux Shell中的变量分为，系统变量和用户自定义变量

- 系统变量：$HOME, $PWD, $SHELL, $USER等等
- 显示当前shell中所有变量：set

#### 18.2.2、设置环境变量

**基本语法**

- **export 变量名=变量值**(功能描述：将Shell变量输出为环境变量)
- **source 配置文件**(功能描述：让修改后的配置信息立即生效)
- **echo $变量名**(功能描述：查询环境变量的值)

**设置完环境变量后，需要刷新配置文件**

#### 18.2.3、预定义变量

**基本语法**

- **$$**(功能描述：当前进程的进程号(PID))
- **$!**(功能描述：后台运行的最后一个进程的进程号(PID))
- **$?**(功能描述：最后一次执行的命令的返回状态)

#### 18.2.4、条件判断

**基本语法**

- **[ condition ]** (非空返回true,可使用$?验证( 0为true, >1为false))

```markdown
1) = 字符串比较
2) 两个整数比较
-lt 小于		-le 小于等于		-eq 等于		-gt 大于		-ge 大于等于		-ne 不等于
3) 按照文件权限进行判断
-r 有读的权限
-w 有写的权限
-x 有执行的权限
4) 按照文件类型进行判断
-f 文件存在并且是一个常规文件
-e 文件存在
-d 文件存在并是一个目录
```

#### 18.2.5、流程控制

**基本语法**

```sh
if [ 条件判断式 ]
then
代码
fi
或者
if [ 条件判断式 ]
then
代码
elif [ 条件判断式 ]
then
代码
fi
```

#### 18.2.6、系统函数

- **basename**( 功能：返回完整路径最后/的部分，常用于获取文件名)
- **dirname**( 功能： 返回完整路径最后 /的前面的部分，常用于返回路径部分)

### 18.3、Shell编程综合案例

- **需求分析**
  - 每天凌晨 2:30 备份数据库 chenedu 到 /data/backup/db
  - 备份开始和备份结束能够给出相应的提示信息
  - 备份后的文件要求以备份时间为文件名，并打包成`.tar.gz` 的形式
  - 在备份的同时，检查是否有10前备份的数据库文件，如果有就将其删除

## 19、系统日志

### 19.1、系统常用日志

- **/var/log/boot.log** (系统启动日志)
- **/var/log/cron**(记录与系统定时任务相关的日志)
- **/var/log/cups/**(记录打印信息的日志)
- **/var/log/dmesg**(记录了系统在开机时内核自检的信息)
- **/var/log/btmp**(记录错误登录的日志，二进制文件不能直接使用vi查看，需使用lastb查看)
- **/var/log/lasllog**(记录系统中所有用户最后一次登录时间的日志，二进制文件使用lastlog查看)
- **/var/log/mailog**(记录邮件信息的日志)
- **/var/log/message**(记录系统重要消息的日志，他会记录Linux系统的绝大部分重要信息)
- **/var/log/secure**(记录验证和授权方面的信息，只要涉及账户和密码的程序都会记录)
- **/var/log/wtmp**(永久记录所有用户的登录，注销信息，同时记录系统的启动、重启、关机事件)
- **/var/log/ulmp**(记录当前已经登录的用户的信息，需要使用w、who、users等命令查看)

### 19.2、日志管理服务

**配置文件：/etc/rsyslog.conf**

**日志类型分为：**

- **auth**(pam产生的日志)
- **authpriv**(ssh、ftp等登录信息的验证信息)
- **cron**(时间任务相关)
- **kern**(内核)
- **lpr**(打印)
- **mail**(邮件)
- **mark(syslog)-rsyslog**(服务内部的信息，时间标识)
- **news**(新闻组)
- **user**(用户程序产生的相关信息)
- **uucp**(unix to unix copy 主机之间相关的通信)
- **local 1-7**(自定义的日志设备)

**日志级别分为：**

- **debug**(有调试信息的，日志通信最多)
- **info**(一般信息日志，最常用)
- **notice**(最具有重要性的普通条件的信息)
- **waring**(警告级别)
- **err**(错误级别，阻止某个功能或模块不能正常工作的信息)
- **crit**(严重级别，阻止真个系统或整个软件不能正常工作的信息)
- **alert**(需要立刻修改的信息)
- **emerg**(内核崩溃等重要信息)

### 19.3、日志轮替

**基本介绍**

日志轮替就是把旧的日志文件移动并改名，同时建立新的控日志文件夹，当旧的日志文件超出保存范围之后，就会进行删除。

### 19.4、查看内存日志

**journalctl** 可以查看内存日志

**常用指令：**

- **journalctl**(查看全部)
- **journalctl -n 3**(查看最新的3条)
- **journalctl --since 19:00 --until 19:10:10**(查看起始时间到截止时间的日志可加日期)
- **journalctl -p err**(报错日志)
- **journalctl _PID=1245 _COMM=sshd** (查看包含这些参数的日志)
- **journalctl | grep sshd**(查看当前参数的日志)

## 20、备份和恢复

### 20.1、基本介绍

**备份和回复的两种方法：**

- 把需要的文件(或者分区)用TAR打包就行，恢复时直接解压就行
- 使用dump和restore命令

### 20.2、安装dump和restore

- 先安装dump和restore指令
  - yum -y install dump
  - yum -y install restore

### 20.3、使用dump完成备份

#### 20.3.1、基本介绍

dump支持分卷和增量备份(所谓的增量备份是指备份上次备份后 修改/增加过的文件)

#### 20.3.2、dump语法

**dump [-cu] [-123456789] [-f<备份后的文件名>] [-T <日期>] [目录或文件系统]**

````bash
-c #创建新的归档文件，并将由一个或多个文件参数所指定的内容写入文档文件的开头
-123456789 #备份的层级。0为最完整的备份，会备份所有
-f #调用bzlib库压缩备份文件，也就是将备份后的文件压缩成bz2格式，让文件更小
-T <日期> # 指定开始备份的时间与日期
-u # 备份完毕后，在/etc/dumpdares中记录备份的文件系统，层级，日期与时间等
-t # 指定文件名，若该文件已存在备份文件中，则列出名称
-W # 显示需要备份的文件及其最后一次备份的层级，时间，日期
-w # 与-W类似，单仅显示需要备份的文件。
````

#### 20.3.3、使用restore完成恢复

**基本介绍：**

`restroe命令用来恢复已备份的文件，可以从dump生成的备份文件中恢复原文件`

**基本语法：**

**restore [模式选项] [选项]**

说明下面四种模式，不能混用，再一次命令中只能指定一种

- **-C**	：使用对比模式，将备份的文件与已存在的文件相互对比

- **-i**     ：使用交互模式，在进行还原操作时，restore指令将依序询问用户

- **-r**    ：进行还原模式

- **-t**    ：查看模式，看备份文件有哪些文件

  

**选项**

- **-f** <备份设备>    ：从指定的文件中读取备份数据，进行还原操作	
