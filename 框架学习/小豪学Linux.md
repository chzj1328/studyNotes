# 小豪学Linux（一）

## 1、课前小菜

### 1、**RPM软件包管理命令**

- **安装软件的命令格式 rpm -ivh filename.rpm**  
- **升级软件的命令格式 rpm -Uvh filename.rpm**  
- **卸载软件的命令格式 rpm -e filename.rpm**  
- **查询软件描述信息的命令格式 rpm -qpi filename.rpm**  
- **列出软件文件信息的命令格式 rpm -qpl filename.rpm**  
- **查询文件属于哪个 RPM 的命令格式 rpm -qf filename** 

### 2、**YUM软件包管理命令**

- **yum repolist all 				列出所有仓库** 
	 **yum list all 					列出仓库中所有软件包** 
	 **yum info 软件包名称 			查看软件包信息**
	  **yum install 软件包名称 		安装软件包**
	  **yum reinstall 软件包名称		重新安装软件包** 
	 **yum update 软件包名称 		升级软件包** 
	 **yum remove 软件包 			移除软件包** 
	 **yum clean all 				清除所有仓库缓存**
	  **yum check-update 			检查可更新的软件包** 
	 **yum grouplist 				查看系统中已经安装的软件包组** 
	 **yum groupinstall 软件包组 	安装指定的软件包组** 
	 **yum groupremove 软件包组 	移除指定的软件包组** 
	 **yum groupinfo 软件包组 		查询指定的软件包组信息** 

### 3、**systemctl管理服务的常用命令**

- **systemctl start foo.service 				启动服务** 
	 **systemctl restart foo.service 			重启服务** 
	 **systemctl stop foo.service 				停止服务**  
	 **systemctl reload foo.service 				重新加载配置文件（不终止服务）** 
	 **systemctl status foo.service 				查看服务状态** 
	 **systemctl enable foo.service 				开机自动启动**
	 **systemctl disable foo.service 			开机不自动启动**
	 **systemctl is-enabled foo.service			查看特定服务是否为开机自动启动**
	 **systemctl list-unit-files --type=service	查看各个级别下服务的启动与禁用情况**

## 2、新手必须掌握的Linux常用命令

### 1.1、echo命令

```shell
[root@chenstudy ~]# echo chen
chen
[root@chenstudy ~]# echo $SHELL
/bin/bash
```

![1649514887392](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1649514887392.png)

### 1.2、date命令

```shell
# 查看当前系统时间
[root@chenstudy ~]# date
2022年 04月 09日 星期六 22:32:21 CST
# 按照“年-月-日 小时:分钟:秒”的格式查看当前系统时间
[root@chenstudy ~]# date "+%Y-%m-%d %H:%M:%S"
2022-04-09 22:32:46
# 将当前系统的时间设置为2022年04月0922点36分
[root@chenstudy ~]# date -s "20220409 22:36:00"
2022年 04月 09日 星期六 22:36:00 CST
# 再次使用date查看系统当前时间
[root@chenstudy ~]# date
2022年 04月 09日 星期六 22:36:11 CST
# dtae "+j" 用来查看今天是当年中的第几天
[root@chenstudy ~]# date "+%j"
099
```

![1649515261944](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1649515261944.png)

### 1.3、wget命令

```shell
# wget 命令用于在终端中下载网络文件，格式为“wget [参数] 下载地址”。
[root@chenstudy ~]# cd /home
[root@chenstudy home]# wget http://www.linuxprobe.com/docs/LinuxProbe.pdf

-b # 后台下载模式
-P # 下载到指定目录
-t # 最大尝试次数
-c # 断点续传
-p # 下载页面内所有资源，包括图片、视频等
-r # 递归下载

# wget 命令递归下载 www.linuxprobe.com 网站内的所有页面数据以及文件，下载完后会自动保存到当前路径下一个名为 www.linuxprobe.com 的目录中
[root@chenstudy home]# wget -r -p http://www.linuxprobe.com，
```

![1649515964579](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1649515964579.png)

### 1.4、ps命令

- ps 命令用于查看系统中的进程状态，格式为“ps [参数]”
- 参数
  - **-a # 显示所有进程（包括其他用户的进程）**
  - **-u # 用户以及其他详细信息**
  - **-x # 显示没有控制终端的进程**
- Linux的进程分为五个进程状态 运行、中断、不可中断、僵死与停止
  - R（运行）：进程正在运行或在运行队列中等待。
  - S（中断）：进程处于休眠中，当某个条件形成后或者接收到信号时，则脱离该状态。
  - D（不可中断）：进程不响应系统异步信号，即便用 kill 命令也不能将其中断。
  - Z（僵死）：进程已经终止，但进程描述符依然存在, 直到父进程调用 wait4()系统函数后将进程释放。
  - T（停止）：进程收到停止信号后停止运行。

```shell
[root@chenstudy home]# ps aux
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.0  0.1 125504  3440 ?        Ss   3月24   0:15 /usr/lib/systemd/systemd --switched-root --system --de
root         2  0.0  0.0      0     0 ?        S    3月24   0:00 [kthreadd]
root         4  0.0  0.0      0     0 ?        S<   3月24   0:00 [kworker/0:0H]
root         6  0.0  0.0      0     0 ?        S    3月24   0:18 [ksoftirqd/0]
root         7  0.0  0.0      0     0 ?        S    3月24   0:00 [migration/0]
root         8  0.0  0.0      0     0 ?        S    3月24   0:00 [rcu_bh]

# USER 		进程的所有者
# PID  		进程ID号
# %CPU 		运算器占用率
# %MEM 		内存占用率
# VSZ		虚拟内存使用量（单位是 KB）
# RSS 		占用的固定内存量（单位是KB）
# TTY		所在终端
# STAT		进程状态
# START		被启动的时间
# TIME 		实际使用CPU的时间
# COMMAND	命令名称与参数
```

![1649516165430](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1649516165430.png)

### 1.5、top 命令 

- top 命令用于动态地监视进程活动与系统负载等信息，其格式为 top 

```shell
[root@chenstudy home]# top
```

![1649578895585](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1649578895585.png)

- top 命令执行结果的前 5 行为系统整体的统计信息，其所代表的含义如下:
  - 第 1 行：系统时间、运行时间、登录终端数、系统负载（三个数值分别为 1 分钟、5分钟、15 分钟内的平均值，数值越小意味着负载越低）
  - 第 2 行：进程总数、运行中的进程数、睡眠中的进程数、停止的进程数、僵死的进程数
  - 第 3 行：用户占用资源百分比、系统内核占用资源百分比、改变过优先级的进程资源百分比、空闲的资源百分比等
  - 第 4 行：物理内存总量、内存使用量、内存空闲量、作为内核缓存的内存量
  - 第 5 行：虚拟内存总量、虚拟内存使用量、虚拟内存空闲量、已被提前加载的内存量

### 1.6、pidof和kill命令

- pidof 命令用于查询某个指定服务进程的 PID 值，格式为“pidof [参数 服务名称]” 
- kill 命令用于终止某个指定 PID 的服务进程，格式为“kill [参数 进程 PID]” 
- killall 命令用于终止某个指定名称的服务所对应的全部进程，格式为：“killall [参数 进程名称]” 

```shell
# 查询本机上 sshd 服务程序的 PID
[root@chenstudy ~]# pidof sshd
24486 1156
[root@chenstudy ~]# kill 1156

[root@chenstudy ~]# pidof httpd
[root@chenstudy ~]# killall httpd
```

![1649579588076](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1649579588076.png)

### 1.7、ifconfig命令

- ifconfig 命令用于获取网卡配置与网络状态等信息，格式为“ifconfig [网络设备 参数]” 

```shell
[root@chenstudy ~]# ifconfig
```

![1649949443485](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/MdMd1649949443485.png)

### 1.8、uname 命令 

- uname 命令用于查看系统内核与系统版本等信息，格式为“uname [-a]” 
- 查看当前系统版本的详细信息，则需要查看 redhat-release 文件， 

```shell
[root@chenstudy ~]# uname -a
Linux chenstudy 3.10.0-1160.53.1.el7.x86_64 #1 SMP Fri Jan 14 13:59:45 UTC 2022 x86_64 x86_64 x86_64 GNU/Linux
[root@chenstudy etc]# cat centos-release
CentOS Linux release 7.9.2009 (Core)
[root@chenstudy etc]# 
```

![1649584235212](Linux学习笔记.assets/1649584235212.png)

![1649584267938](Linux学习笔记.assets/1649584267938.png)

### 1.9、uptime和free命令

- uptime 用于查看系统的负载信息，格式为 uptime
  - 它可以显示当前系统时间、系统已运行时间、启用终端数量以 及平均负载值等信息 
- free 用于显示当前系统中内存的使用量信息，格式为“free [-h]” 

```shell
[root@chenstudy ~]# uptime
 23:11:18 up 17 days,  6:46,  2 users,  load average: 0.05, 0.22, 0.28
[root@chenstudy ~]# free -h
              total        used        free      shared  buff/cache   available
Mem:           1.8G        347M        192M        612K        1.3G        1.3G
Swap:            0B          0B          0B
# total 		内存总量
# used 			已用量
# free 			可用量
# shared		进程共享的内存量
# buff/cache	磁盘缓存的内存量/缓存的内存量
# available		可利用的内存量
```

![1649603596527](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1649603596527.png)

### 1.10、who和last命令

- **who 用于查看当前登入主机的用户终端信息，格式为“who [参数]”**

```shell
[root@chenstudy ~]# who
root     pts/0        2022-04-10 20:07 (117.158.163.68)
root     pts/1        2022-04-10 20:07 (117.158.163.68)
[root@chenstudy ~]# 
```

![1649604289613](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1649604289613.png)

**执行 who 命令的结果** 

| 登录的用户名 | 终端设备 |         登录到系统的时间          |
| :----------: | :------: | :-------------------------------: |
|     root     |  pts/0   | 2022-04-10 20:07 (117.158.163.68) |
|     root     |  pts/1   | 2022-04-10 20:07 (117.158.163.68) |

- **last 命令用于查看所有系统的登录记录，格式为“last [参数]”** 

```shell
# 使用 last 命令可以查看本机的登录记录由于这些信息都是以日志文件的形式保存在系统中，因此黑客可以很容易地对内容进行篡改
[root@chenstudy ~]# last
root     pts/1        117.158.163.68   Sun Apr 10 20:07   still logged in   
root     pts/0        117.158.163.68   Sun Apr 10 20:07   still logged in   
root     pts/1        117.158.163.67   Sun Apr 10 15:12 - 19:27  (04:14)    
root     pts/0        117.158.163.67   Sun Apr 10 15:12 - 19:27  (04:14)    
root     pts/1        117.158.163.67   Sun Apr 10 08:29 - 12:27  (03:58)    
root     pts/0        117.158.163.67   Sun Apr 10 08:29 - 12:27  (03:58)    
root     pts/1        117.158.163.79   Sat Apr  9 22:25 - 23:49  (01:24)    
root     pts/0        117.158.163.79   Sat Apr  9 22:25 - 23:49  (01:24)    
root     pts/1        117.158.163.78   Thu Apr  7 08:39 - 08:42  (00:03)    
root     pts/0        117.158.163.78   Thu Apr  7 08:39 - 08:42  (00:03)    
root     pts/1        117.158.163.78   Wed Apr  6 16:22 - 20:14  (03:52)    
root     pts/3        117.158.163.78   Wed Apr  6 15:45 - 20:14  (04:28)    
root     pts/4        117.158.163.78   Wed Apr  6 15:38 - 15:40  (00:01)    
```

![1649604659050](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1649604659050.png)

### 1.11、history和sosreport 命令 

- **history 命令用于显示历史执行过的命令，格式为“history [-c]”** 
  - **-c会清空所有的命令历史记录**
- **sosreport 命令用于收集系统配置及架构信息并输出诊断文档，格式为 sosreport** 

```shell
[root@chenstudy ~]# history
    1  clear
    2  cd /
    3  uname -f
    4  clear
    5  uname -r
    6  clear
    7  cat /etc/os-release
# 历史命令会被保存到用户家目录中的.bash_history 文件中
[root@chenstudy ~]# cat ~/.bash_history
clear
cd /
uname -f
clear
uname -r
clear
cat /etc/os-release
# 清空当前用户在本机上执行的Linux命令历史记录
[root@chenstudy ~]# history -c
```

![1649605037351](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1649605037351.png)

![1649605155053](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1649605155053.png)

![1649605214267](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1649605214267.png)

![1649605388194](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1649605388194.png)

### 2.1、pwd、cd、ls命令

- **pwd 命令用于显示用户当前所处的工作目录，格式为“pwd [选项]”** 
- **cd 命令用于切换工作路径，格式为“cd [目录名称]”** 
- **ls 命令用于显示目录中的文件信息，格式为“ls [选项  文件] ”** 

```shell
[root@chenstudy ~]# pwd
/root
[root@chenstudy ~]# cd /home
[root@chenstudy home]# ls
ceshi  chen  chen.java  dockerfile  LinuxProbe.pdf  mysql  test.java
# 使用 ls 命令的“-a”参数看到全部文件（包括隐藏文件）
# 使用“-l”参数可以查看文件的属性、大小等详细信息
[root@chenstudy home]# ls -al
总用量 17288
drwxr-xr-x.  6 root root     4096 4月   9 22:52 .
dr-xr-xr-x. 18 root root     4096 3月  24 16:24 ..
drwxr-xr-x   2 root root     4096 4月   2 09:29 ceshi
drwxr-xr-x   3 root root     4096 4月   6 15:32 chen
-rw-r--r--   1 root root        0 3月  30 23:26 chen.java
drwxr-xr-x   2 root root     4096 4月   5 10:54 dockerfile
-rw-r--r--   1 root root 17676281 9月  22 2020 LinuxProbe.pdf
drwxr-xr-x   4 root root     4096 4月   2 09:49 mysql
-rw-r--r--   1 root root        0 3月  30 23:28 test.java
# 查看目录属性信息，则需要额外添加一个-d 参数
[root@chenstudy home]# ls -ld /etc
drwxr-xr-x. 78 root root 4096 3月  29 18:00 /etc
[root@chenstudy home]# 
```

![1649657666969](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1649657666969.png)

### 2.2、touch、mkdir、cp命令

- **touch 命令用于创建空白文件或设置文件的时间，格式为“touch [选项  文件]”** 
  - **b-a 仅修改“读取时间”（atime）** 
  - **-m 仅修改“修改时间”（mtime）** 
  - **-d 同时修改 atime 与 mtime** 

```shell
[root@chenstudy home]# ls
ceshi  chen  chen.java  dockerfile  LinuxProbe.pdf  mysql  test.java
[root@chenstudy home]# ls -l chen.java 
-rw-r--r-- 1 root root 54 4月  11 14:24 chen.java
[root@chenstudy home]# echo "System.out.println("Hello");" >> chen.java 
[root@chenstudy home]# ls -l chen.java 
-rw-r--r-- 1 root root 81 4月  11 14:25 chen.java
[root@chenstudy home]# touch -d "2021-06-28 15:44" chen.java 
[root@chenstudy home]# ls -l chen.java 
-rw-r--r-- 1 root root 81 6月  28 2021 chen.java
[root@chenstudy home]# 
```

![1649658424973](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1649658424973.png)

- **mkdir 命令用于创建空白的目录，格式为“mkdir [选项] 目录”** 
  - **-p 参数来递归创建出具有嵌套叠层关系的文件目录** 

```shell
[root@chenstudy home]# mkdir linux
[root@chenstudy home]# cd linux/
[root@chenstudy linux]# mkdir -p a/b/c
[root@chenstudy linux]# cd a/
[root@chenstudy a]# cd b/
[root@chenstudy b]# 
```

![1649658572337](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1649658572337.png)



- **cp 命令用于复制文件或目录，格式为“cp [选项] 源文件 目标文件”** 
  - **-p 保留原始文件的属性** 
  - **-d 若对象为“链接文件”，则保留该“链接文件”的属性** 
  - **-r 递归持续复制（用于目录）** 
  - **-i 若目标文件存在则询问是否覆盖** 
  - **-a 相当于-pdr（p、d、r 为上述参数）** 

```shell
[root@chenstudy linux]# touch install.log
[root@chenstudy linux]# cp install.log x.log
[root@chenstudy linux]# ls
a  install.log  x.log
[root@chenstudy linux]# 
```

![1649667421150](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1649667421150.png)

### 2.3、mv、rm、dd和file命令

- **mv 命令用于剪切文件或将文件重命名，格式为“mv [选项] 源文件 [目标路径|目标文件名]”** 

```shell
[root@chenstudy linux]# mv x.log linux.log
[root@chenstudy linux]# ls
a  install.log  linux.log
[root@chenstudy linux]# 
```



![1649667665827](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1649667665827.png)

- **rm 命令用于删除文件或目录，格式为“rm [选项] 文件”** 

```shell
[root@chenstudy linux]# rm install.log 
rm：是否删除普通空文件 "install.log"？y
[root@chenstudy linux]# ls
a  linux.log
[root@chenstudy linux]# rm -f linux.log 
[root@chenstudy linux]# ls
a
# 删除目录
[root@chenstudy linux]# rm -rf a
[root@chenstudy linux]# ls
[root@chenstudy linux]# 
```



![1649667889128](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1649667889128.png)

- **dd 命令用于按照指定大小和个数的数据块来复制文件或转换文件，格式为“dd [参数]”** 
  - **if 输入的文件名称** 
  - **of 输出的文件名称**
  - **bs 设置每个“块”的大小** 
  - **count 设置要复制“块”的个数** 

```shell
# /dev/zero 设备文件中取出一个大小为 560MB 的数据块,保存成名为 560_file 的文件
[root@chenstudy home]# dd if=/dev/zero of=560_file count=1 bs=560MB
记录了1+0 的读入
记录了1+0 的写出
560000000字节(560 MB)已复制，4.66672 秒，120 MB/秒
[root@chenstudy home]# 
# dd命令可以把光驱设备中的光盘制作成 iso 格式的镜像文件
[root@chenstudy home]#  dd if=/dev/cdrom of=RHEL-server-7.0-x86_64-LinuxProbe.Com.iso
7311360+0 records in
7311360+0 records out
3743416320 bytes (3.7 GB) copied, 370.758 s, 10.1 MB/s
```



- **file 命令用于查看文件的类型，格式为“file 文件名”** 

```shell
[root@chenstudy home]# file chen.java 
chen.java: ASCII text
[root@chenstudy home]# file /home/dockerfile/
/home/dockerfile/: directory
[root@chenstudy home]# 
```

![1649668639856](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1649668639856.png)

### 2.4、tar、grep和find命令

- **tar 命令用于对文件进行打包压缩或解压，格式为“tar [选项 文件]”** 
  - **-c 创建压缩文件** 
  - **-x 解开压缩文件** 
  - **-t 查看压缩包内有哪些文件** 
  - **-z 用 Gzip 压缩或解压** 
  - **-j 用 bzip2 压缩或解压** 
  - **-v 显示压缩或解压的过程** 
  - **-f 目标文件名** 
  - **-p 保留原始的权限与属性** 
  - **-P 使用绝对路径来压缩** 
  - **-C 指定解压到的目录**

```shell
[root@chenstudy home]# tar -czvf etc.tar.gz /etc 
tar: 从成员名中删除开头的“/”
/etc/
/etc/depmod.d/
/etc/depmod.d/dist.conf
/etc/sudoers
/etc/gss/
/etc/gss/mech.d/
/etc/anacrontab
/etc/wgetrc
/etc/python/
# 将打包后的压缩包文件指定解压到/root/etc 目录中
[root@chenstudy home]# mkdir /root/etc
[root@chenstudy home]# tar xzvf etc.tar.gz -C /root/etc
```

![1649689372407](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1649689372407.png)

![1649689899210](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1649689899210.png)

- **grep 命令用于在文本中执行关键词搜索，并显示匹配的结果，格式为“grep [选项 文件]”** 
  - **-b 将可执行文件（binary）当作文本文件（text）来搜索** 
  - **-c 仅显示找到的行数** 
  - **-i 忽略大小写** 
  - **-n 显示行号** 
  - **-v 反向选择—仅列出没有“关键词”的行** 

```shell
[root@chenstudy home]# grep /sbin/nologin /etc/passwd
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
operator:x:11:0:operator:/root:/sbin/nologin
games:x:12:100:games:/usr/games:/sbin/nologin
ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
nobody:x:99:99:Nobody:/:/sbin/nologin
```

![1649690102945](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1649690102945.png)

- **find 命令用于按照指定条件来查找文件，格式为“find [查找路径] 寻找条件 操作”** 
  - **-name 匹配名称** 
  - **-perm 匹配权限（mode 为完全匹配，-mode 为包含即可）** 
  - **-user 匹配所有者** 
  - **-group 匹配所有组** 
  - **-mtime -n +n 匹配修改内容的时间（-n 指 n 天以内，+n 指 n 天以前）** 
  - **-atime -n +n 匹配访问文件的时间（-n 指 n 天以内，+n 指 n 天以前）** 
  - **-ctime -n +n 匹配修改文件权限的时间（-n 指 n 天以内，+n 指 n 天以前）** 
  - **-nouser 匹配无所有者的文件** 
  - **-nogroup 匹配无所有组的文件** 
  - **-newer f1 !f2 匹配比文件 f1 新但比 f2 旧的文件** 
  - **--type b/d/c/p/l/f 匹配文件类型（后面的字幕参数依次表示块设备、目录、字符设备、管道、 链接文件、文本文件）** 
  - **-size  匹配文件的大小（+50KB 为查找超过 50KB 的文件，而-50KB 为查找小于 50KB 的文件）** 
  - **-prune 忽略某个目录** 
  - **-exec …… {}\;    后面可跟用于进一步处理搜索结果的命令（下文会有演示）** 

```shell
[root@chenstudy home]# find /etc -name "host*" -print
/etc/hosts.allow
/etc/hosts.deny
/etc/host.conf
/etc/hostname
/etc/cloud/templates/hosts.suse.tmpl
/etc/cloud/templates/hosts.debian.tmpl
/etc/cloud/templates/hosts.freebsd.tmpl
/etc/cloud/templates/hosts.redhat.tmpl
/etc/selinux/targeted/active/modules/100/hostname
/etc/hosts
# 要在整个系统中搜索权限中包括 SUID 权限的所有文件只需使用-4000 即可
# CentOS 报错未知断言 Red Hat里可以
[root@chenstudy home]# find / -perm -4000 -print
/usr/bin/fusermount
/usr/bin/su
/usr/bin/umount
/usr/bin/passwd
/usr/sbin/userhelper
/usr/sbin/usernetctl 
```

![1649690531957](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1649690531957.png)

### 小结：复习题

```txt
1．在 RHEL 7 系统及众多的 Linux 系统中，最常使用的 Shell 终端是什么？
	答：Bash（Bourne-Again SHell）解释器。 
2．执行 Linux 系统命令时，添加参数的目的是什么？
	答：为了让 Linux 系统命令能够更贴合用户的实际需求进行工作。
3．Linux 系统命令、命令参数及命令对象之间，普遍应该使用什么来间隔？
	答：应该使用一个或多个空格进行间隔。
4．请写出用 echo 命令把 SHELL 变量值输出到屏幕终端的命令。
	答：echo $SHELL。
5．简述 Linux 系统中 5 种进程的名称及含义。
	答：在 Linux 系统中，有下面 5 种进程名称。
		R（运行）：进程正在运行或在运行队列中等待。
		S（中断）：进程处于休眠中，当某个条件形成后或者接收到信号时，则脱离该状态。
		D（不可中断）：进程不响应系统异步信号，即便用 kill 命令也不能将其中断。
		Z（僵死）：进程已经终止，但进程描述符依然存在, 直到父进程调用 wait4()系统函数后将进程释放。
		T（停止）：进程收到停止信号后停止运行。
6．请尝试使用 Linux 系统命令关闭 PID 为 5529 的服务进程。
	答：执行 kill 5529 命令即可；若知道服务的名称，则可以使用 killall 命令进行关闭。
7．使用 ifconfig 命令查看网络状态信息时，需要重点查看的 4 项信息分别是什么？
	答：这 4 项重要信息分别是网卡名称、IP 地址、网卡物理地址以及 RX/TX 的收发流量数据大小。
8．使用 uptime 命令查看系统负载时，对应的负载数值如果是 0.91、0.56、0.32，那么最近15分钟内负载压力最大的是哪个时间段？
	答：通过负载数值可以看出，最近 1 分钟内的负载压力是最大的。
9．使用 history 命令查看历史命令的执行记录时，命令前面的数字除了排序外还有什么用处？
	答：还可以用“!数字”的命令格式重复执行某一次的命令记录，从而避免了重复输入较长命令的麻烦。
10．若想查看的文件具有较长的内容，那么使用 cat、more、head、tail 中的哪个命令最
合适？
	答：文件内容较长，使用 more 命令；反之使用 cat 命令。
11．在使用 mkdir 命令创建有嵌套关系的目录时，应该加上什么参数呢？
	答：应该加上-p 递归迭代参数，从而自动化创建有嵌套关系的目录。
12．在使用 rm 命令删除文件或目录时，可使用哪个参数来避免二次确认呢？
	答：可使用-f 参数，这样即可无需二次确认。
13．若有一个名为 backup.tar.gz 的压缩包文件，那么解压的命令应该是什么？
	答：应该用 tar 命令进行解压，执行 tar -xzvf backup.tar.gz 命令即可。
14．使用 grep 命令对某个文件进行关键词搜索时，若想要进行文件内容反选，应使用什么
参数？
	答：可使用-v 参数来进行匹配内容的反向选择，即显示出不包含某个关键词的行。
```

## 3、管道符、重定向与环境变量 

### 3.1、输入输出重定向

- 标准输入重定向(STDIN，文件描述符为0)默认从键盘输入，还可以从其他文件或命令输入
- 标准输出重定向(STDOUT,文件描述符为1):默认输出到屏幕
- 错误输出重定向(STDERR,文件描述符为2):默认输出到屏幕



```shell
[root@chenstudy ~]# cd /home
[root@chenstudy home]# touch linux
[root@chenstudy home]# ls -l linux 
-rw-r--r--. 1 root root 0 4月  14 19:48 linux
[root@chenstudy home]# ls -l xxxxx
ls: 无法访问xxxxx: 没有那个文件或目录
[root@chenstudy home]# 
# 在上述命令中，名为 linux 的文件是存在的，输出信息是该文件的一些相关权限、所有者、所属组、文件大小及修改时间等信息，这也是该命令的标准输出信息,而名为 xxxxxx的第二个文件是不存在的，因此在执行完 ls 命令之后显示的报错提示信息也是该命令的错误输出信息
```



![1649937538825](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1649937538825.png)

- **输入重定向中用到的符号及其作用** 

| 符号                   | 作用                                            |
| :--------------------- | :---------------------------------------------- |
| 命令  < 文件           | 将文件作为命令的标准输入                        |
| 命令 < < 分界符        | 从标准输入中读入，直到遇见分界符才停止          |
| 命令 < 文件 1 > 文件 2 | 将文件 1 作为命令的标准输入并将标准输出到文件 2 |



- **输出重定向中用到的符号及其作用** 

|                符号                |                             作用                             |
| :--------------------------------: | :----------------------------------------------------------: |
|            命令 > 文件             |      将标准输出重定向到一个文件中（清空原有文件的数据）      |
|            命令 2> 文件            |      将错误输出重定向到一个文件中（清空原有文件的数据）      |
|            命令 >> 文件            |     将标准输出重定向到一个文件中（追加到原有内容的后面）     |
|           命令 2>> 文件            |     将错误输出重定向到一个文件中（追加到原有内容的后面）     |
| 命令 >> 文件 2>&1 或 命令 &>> 文件 | 将标准输出与错误输出共同写入到文件中（追加到原有内容的 后面） |

**对于重定向中的标准输出模式，可以省略文件描述符 1 不写 而错误输出模式的文件描述符 2 是必须要写的** 

```shell
# 通过标准输出重定向将 man bash 命令原本要输出到屏幕的信息写入到文件readme.txt 中，然后显示readme.txt 文件中的内容
[root@chenstudy home]# man bash > readme.txt
[root@chenstudy home]# cat readme.txt 
 -c string 如果有 -c 选项，那么命令将从 string 中读取。如果 string 后面有参数 (argument)，它们将用于给位置参数 (positional parameter，以 $0 起始) 赋值。
       -i        如果有 -i 选项，shell 将交互地执行 ( interactive )。
       -l        选项使得 bash 以类似登录 shell (login shell) 的方式启动 (参见下面的 启
       -r        如果有 -r 选项，shell 成为受限的 ( restricted ) (参见下面的 受
       -s        如果有 -s 选项，或者如果选项处理完以后，没有参数剩余，那么命令将从标准输入读取。 这个选项允许在启动一个交互 shell 时可以设置位置参数。
       -D        向标准输出打印一个以 $ 为前导的，以双引号引用的字符串列表。 这是在当前语言环境不是 C 或 POSIX 时，脚本中需要翻译的字符串。 这个选项隐含了 -n 选项；不会执行命令。
       [-+]O [shopt_option]
                 shopt_option  是一个  shopt  内建命令可接受的选项  (参见下面的  shell  内
                 取消它。 如果没有给出 shopt_option，shopt 将在标准输出上打印设为允许的选项的名称和值。 如果启动选项是 +O，输出将以一种可以重用为输入的格式显示。
       --        -- 标志选项的结束，禁止其余的选项处理。任何 -- 之后的参数将作为文件名和参数对待。参数 - 与此等价。

       Bash 也解释一些多字节的选项。在命令行中，这些选项必须置于需要被识别的单字符参数之前。

       --dump-po-strings
              等价于 -D，但是输出是 GNU gettext po (可移植对象) 文件格式
       --dump-strings
              等价于 -D
       --help 在标准输出显示用法信息并成功退出
       --init-file file
```



![1649937655308](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1649937655308.png)

**重写readme.txt文件，并写入：Wlecome to XYNU,然后再追加Hello 22**

```shell
[root@chenstudy home]# echo "Wlecome to XYNU" > readme.txt 
[root@chenstudy home]# echo "Hello 22" >> readme.txt 
[root@chenstudy home]# cat readme.txt 
Wlecome to XYNU
Hello 22
```



![1649937894551](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1649937894551.png)

**对于输处重定向技术，不同命令的标准输出和错误输出还是有区别的**

对于真实存在的文件，使用标准输出即将原本输出到屏幕的信息写入到文件中，而错误的输出重定向则依然把信息输出到屏幕上

```shell
[root@chenstudy home]# ls -l linux 
-rw-r--r--. 1 root root 0 4月  14 19:48 linux
[root@chenstudy home]# ls -l linux > /root/stderr.txt
[root@chenstudy home]# ls -l linux 2> /root/stderr.txt
-rw-r--r--. 1 root root 0 4月  14 19:48 linux
[root@chenstudy home]# 
```



![1649938475797](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1649938475797.png)**如何把错误信息写入到文件中呢？**

```shell
[root@chenstudy home]# ls -l xxxx
ls: 无法访问xxxx: 没有那个文件或目录
[root@chenstudy home]# ls -l xxxx > /root/stderr.txt
ls: 无法访问xxxx: 没有那个文件或目录
[root@chenstudy home]# ls -l xxxx 2> /root/stderr.txt
[root@chenstudy home]# cat /root/stderr.txt 
ls: 无法访问xxxx: 没有那个文件或目录
[root@chenstudy home]# 
```



![1649938640969](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/1649938640969.png)

### 3.2、 管道命令符

**执行格式为“命令 A | 命令 B” **

```shell
# 找出被限制登录用户的命令是 grep "/sbin/nologin" /etc/passwd；
# 统计文本行数的命令则是 wc -l
[root@chenstudy home]# grep "/sbin/nologin" /etc/passwd | wc -l
39
[root@chenstudy home]# 

# 查看/etc 目录中的文件列表及属性信息
[root@chenstudy home]# ls -l /etc/ | more
```

![1649939353633](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1649939353633.png)

**在修改用户密码时，通常都需要输入两次密码以进行确认，这在编写自动化脚本时将成 第3章 管道符、重定向与环境变量 64 为一个非常致命的缺陷。通过把管道符和 passwd 命令的--stdin 参数相结合，我们可以用一条 命令来完成密码重置操作:**

```shell
[root@chenstudy home]# echo "root" | passwd --stdin root
更改用户 root 的密码 。
passwd：所有的身份验证令牌已经成功更新。
[root@chenstudy home]# 
```

**管道符命令可以组合使用多个命令A | 命令B | 命令C**

### 3.3、命令行统配符

**我们有时候也会遇到明明一 个文件的名称就在嘴边但就是想不起来的情况批量查看所有硬 盘文件的相关权限属性，一种方式是这样的：**

```shell
[root@chenstudy home]# ls -l /dev/sda
brw-rw----. 1 root disk 8, 0 4月  14 19:40 /dev/sda
[root@chenstudy home]# ls -l /dev/sda1
brw-rw----. 1 root disk 8, 1 4月  14 19:40 /dev/sda1
#通配符就是通用的匹配信息的符号，比如星号（*）代表匹配零个或多个字符，问号（?）代表匹配单个字符，中括号内加上数字[0-9]代表匹配 0～9之间的单个数字的字符，而中括号内加上字母[abc]则是代表匹配 a、b、c 三个字符中的任意一个字符
# 比如星号（*）代表匹配零个或多个字符
[root@chenstudy home]# ls -l /dev/sda*
brw-rw----. 1 root disk 8, 0 4月  14 19:40 /dev/sda
brw-rw----. 1 root disk 8, 1 4月  14 19:40 /dev/sda1
brw-rw----. 1 root disk 8, 2 4月  14 19:40 /dev/sda2
brw-rw----. 1 root disk 8, 3 4月  14 19:40 /dev/sda3
# 只想查看文件名为 sda 开头，但是后面还紧跟其他某一个字符的文件的相关信息就需要用到问号来进行通配了
[root@chenstudy home]# ls -l /dev/sda?
brw-rw----. 1 root disk 8, 1 4月  14 19:40 /dev/sda1
brw-rw----. 1 root disk 8, 2 4月  14 19:40 /dev/sda2
brw-rw----. 1 root disk 8, 3 4月  14 19:40 /dev/sda3
# 匹配任意的数字0-9
[root@chenstudy home]# ls -l /dev/sda[0-9]
brw-rw----. 1 root disk 8, 1 4月  14 19:40 /dev/sda1
brw-rw----. 1 root disk 8, 2 4月  14 19:40 /dev/sda2
brw-rw----. 1 root disk 8, 3 4月  14 19:40 /dev/sda3
[root@chenstudy home]# ls -l /dev/sda[135]
brw-rw----. 1 root disk 8, 1 4月  14 19:40 /dev/sda1
brw-rw----. 1 root disk 8, 3 4月  14 19:40 /dev/sda3
```

  

![1649943113029](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1649943113029.png)

 ### 3.4、 常用的转义字符  

**常用的转义字符：为了能够更好地理解用户的表达，Shell 解释器还提供了特别丰富的转义字符来处理输入 的特殊数据** 

- **反斜杠（\）：使反斜杠后面的一个变量变为单纯的字符串**
- **反斜杠（\）：使反斜杠后面的一个变量变为单纯的字符串** 
- **双引号（""）：保留其中的变量属性，不进行转义处理** 
- **反引号（``）：把其中的命令执行后返回结果** 

```shell
#先定义一个名为 PRICE 的变量并赋值为 5，然后输出以双引号括起来的字符串与变量信息：
[root@chenstudy ~]# PRICE=5
[root@chenstudy ~]# echo "PRICE IS $PRICE"
PRICE IS 5
# 输出“Price is $5”
[root@chenstudy ~]# echo "PRICE IS \$$PRICE"
PRICE IS $5
# 反引号与 uname -a 命令结合，然后使用 echo 命令来查看本机的 Linux版本和内核信息：
[root@chenstudy ~]# echo `uname -a` 
Linux chenstudy 3.10.0-1160.el7.x86_64 #1 SMP Mon Oct 19 16:18:59 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
```

### 3.5、重要的环境变量

- 判断用户是否以绝对路径或相对路径的方式输入命令（如/bin/ls），如果是的话 则直接执行 
- Linux 系统检查用户输入的命令是否为“别名命令”，即用一个自定义的命令 名称来替换原本的命令名称 
  - 用 alias 命令来创建一个属于自己的命令别名，格式为 “alias 别名=命令” 
  - 要取消一个命令别名，则是用 unalias 命令，格式为“unalias 别名” 

```shell
[root@chenstudy ~]# ls
anaconda-ks.cfg  ch  initial-setup-ks.cfg  stderr.txt  公共  模板  视频  图片  文档  下载  音乐  桌面
[root@chenstudy ~]# rm anaconda-ks.cfg 
rm：是否删除普通文件 "anaconda-ks.cfg"？y
[root@chenstudy ~]# alias rm 
alias rm='rm -i'
[root@chenstudy ~]# unalias rm
[root@chenstudy ~]# rm initial-setup-ks.cfg 
[root@chenstudy ~]# 
```



![1649947461019](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1649947461019.png)

- Bash 解释器判断用户输入的是内部命令还是外部命令 ，可以使用“type 命令名称”来判断用户输入的命令是内部命令还是外部命令 
- 系统在多个路径中查找用户输入的命令文件，而定义这些路径的变量叫作 PATH，可 以简单地把它理解是“解释器的小助手”，作用是告诉 Bash 解释器待执行的命令可能存放 的位置，然后 Bash 解释器就会乖乖地在这些位置中逐个查找 

```shell
[root@chenstudy ~]# echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin
[root@chenstudy ~]# PATH=$PATH:/root/bin
[root@chenstudy ~]# echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin:/root/bin
[root@chenstudy ~]#
```



![1649947650572](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1649947650572.png)

- **Linux 系统中最重要的 10 个环境变量** 

|   变量名称   |               作用               |
| :----------: | :------------------------------: |
|     HOME     |     用户的主目录（即家目录）     |
|    SHELL     |   用户在使用的Shell解释器名称    |
|   HISTSIZE   |      输出的历史命令记录条数      |
| HISTFILESIZE |      保存的历史命令记录条数      |
|     MAIL     |           邮件保存路径           |
|     LANG     |        系统语言、语系名称        |
|    RANDOM    |         生成一个随机数字         |
|     PS1      |        Bash解释器的提示符        |
|     PATH     | 定义解释器搜索用户执行命令的路径 |
|    EDITOR    |       用户默认的文本编辑器       |

```shell
[root@chenstudy ~]# echo $HOME
/root
[root@chenstudy ~]# su - chen
上一次登录：二 4月 12 22:16:24 CST 2022:0 上
[chen@chenstudy ~]$ echo $HOME
/home/chen
# 变量是由固定的变量名与用户或系统设置的变量值两部分组成的，我们完全可以自行创建变量，来满足工作需求
[root@chenstudy ~]# mkdir /home/workdir
[root@chenstudy ~]# WORKDIR=/home/workdir
[root@chenstudy ~]# cd $WORKDIR
[root@chenstudy workdir]# pwd
/home/workdir
[root@chenstudy workdir]# 
# 上述方式的变量不具有全局性，工作需要时可以使用export命令将其提升为全局变量
[root@chenstudy workdir]# su - chen
上一次登录：四 4月 14 23:02:17 CST 2022pts/2 上
[chen@chenstudy ~]$ cd $WORKDIR
[chen@chenstudy ~]$ echo $WORKDIR
[chen@chenstudy ~]$ exit
登出
[root@chenstudy workdir]# su - root
上一次登录：四 4月 14 23:03:02 CST 2022pts/2 上
[root@chenstudy ~]# export WORKDIR
[root@chenstudy ~]# cd /home/workdir/
[root@chenstudy workdir]# su - chen
上一次登录：四 4月 14 23:06:08 CST 2022pts/2 上
```

## 4、Vim编辑器与Shell命令脚本

### 4.1、Vim文本编辑器

**Vim编辑器的三种模式及切换方法**

- 命令模式：控制光标移动，可对文本进行复制、粘贴、删除和查找等
- 输入模式：正常文本录入
- 保存或退出我能当，以及设置编译环境

![1650035750060](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1650035750060.png)



- 命令模式中常用的命令

| 命令 |                        作用                        |
| :--: | :------------------------------------------------: |
|  dd  |              删除（剪切）光标所在整行              |
| 5dd  |           删除（剪切）从光标处开始的5行            |
|  yy  |                 复制光标所在的整行                 |
| 5yy  |               复制从光标处开始的5行                |
|  n   |           显示搜索命令定位到的下一个字符           |
|  N   |           显示搜索命令定位到的上一个字符           |
|  u   |                  撤销上一步的操作                  |
|  P   | 将之前删除（dd）或复制（yy）过的数据粘贴到光标后面 |

- 末行模式常用的命令

|     命令      |                 作用                 |
| :-----------: | :----------------------------------: |
|      :w       |                 保存                 |
|      :q       |                 退出                 |
|      :q!      |   强制退出（放弃对文档的修改内容）   |
|     :wq!      |             强制保存退出             |
|    :set nu    |               显示行号               |
|   :set nonu   |              不显示行号              |
|     :命令     |              执行该命令              |
|     :整数     |              跳到该命令              |
|  :s/one/two   | 将当前贯标所在行的第一个one替换成two |
| :s/one/two/g  | 将当前贯标所在行的所有的one替换成two |
| :%s/one/two/g |     将全文中的所有的one替换成two     |
|    ?字符串    |     在文本中从下至上所搜改字符串     |
|    /字符串    |     在文本中从上至下所搜改字符串     |

### 4.1.1、编写简单的文档

**给文档取名字：linux.txt，如果存在则打开，否则创建**

![1650037220807](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1650037220807.png)

**按a、i、o等键进入输入模式**

![1650037286261](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1650037286261.png)

**输入之后按Esc退出**

![1650037389121](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1650037389121.png)

**输入:wq!强制保存并退出，使用cat查看内容**

![1650037490867](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1650037490867.png)

![1650037534795](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1650037534795.png)

**在文件中追加语句，并强制退出不保存，使用cat查看**

![1650037708230](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1650037708230.png)

![1650037696452](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1650037696452.png)

![1650037753328](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1650037753328.png)

### 4.1.2、配置主机名称

**在Linux系统中，主机名大多保存在/etc/hostname文件中**

```shell
[root@chenstudy ~]# vim /etc/hostname
chen
# 重启虚拟机之后在进行查看
[root@chen ~]# hostname
chen
[root@chen ~]# 
```



![1650037990875](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1650037990875.png)

![1650038161208](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1650038161208.png)

### 4.1.3、配置网卡信息

**在 Linux 系统中， 一切都是文件，因此配置网络服务的工作其实就是在编辑网卡配置文件** 

**在CentOS7中网卡配置文件的前缀为ifcfg开始**

- 第1步：首先切换到/etc/sysconfig/network-scripts 目录中（存放着网卡的配置文件） 
- 第2步： 编辑器修改网卡文件 ifcfg-en33，逐项写入下面的配置参数并 保存退出。 
  - 我之前配置过了所以我的直接显示了IP地址
  - 学习Linux时建议大家配置静态IP，这样我们就可以用finalshell进行连接了

```shell
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=static
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=ens33
UUID=cc396254-ae13-43c7-aaec-23b8866d98a1
DEVICE=ens33
ONBOOT=yes
IPADDR=192.168.200.130
GATEWAY=192.168.200.2
DNS1=192.168.200.2
# 设备类型：TYPE=Ethernet 
# 地址分配模式：BOOTPROTO=static 
# 网卡名称：NAME=en33
# 是否启动：ONBOOT=yes 
# IP 地址：IPADDR=192.168.200.130
# 网关地址：GATEWAY=192.168.200.2
# DNS 地址：DNS1=192.168.200.2
```



![1650275629807](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1650275629807.png)

![1650274907448](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1650274907448.png)

- 第3步：重启网络服务并测试网络是否联通。由于在 Linux 系统中 ping 命令不会自动终止，因此需要手动按下 Ctrl-c 键来强行结束进程 。

```shell
[root@chen network-scripts]# systemctl restart network
[root@chen network-scripts]# ping 192.168.200.130
PING 192.168.200.130 (192.168.200.130) 56(84) bytes of data.
64 bytes from 192.168.200.130: icmp_seq=1 ttl=64 time=0.042 ms
64 bytes from 192.168.200.130: icmp_seq=2 ttl=64 time=0.072 ms
64 bytes from 192.168.200.130: icmp_seq=3 ttl=64 time=0.057 ms
64 bytes from 192.168.200.130: icmp_seq=4 ttl=64 time=0.074 ms
64 bytes from 192.168.200.130: icmp_seq=5 ttl=64 time=0.073 ms
^C
--- 192.168.200.130 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 3999ms
rtt min/avg/max/mdev = 0.042/0.063/0.074/0.015 ms
[root@chen network-scripts]# 
```



![1650275749126](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1650275749126.png)

### 4.1.4、配置Yum软件仓库

**Yum 软件仓库的作用是为了进一步简化 RPM 管理软件的难度以及自动分析 所需软件包及其依赖关系的技术 ，要使用 Yum 软件仓库，就要先把它搭建起来，然后将其配置规则确定好才行**

- 第1步：进入到/etc/yum.repos.d/目录中（该目录存放着 Yum 软件仓库的配置文件）。 
- 第2步：使用 Vim 编辑器创建一个名为 rhel7.repo 的新配置文件（文件名称可随意，但后 缀必须为.repo），逐项写入下面加粗的配置参数并保存退出（不要写后面的中文注释）。 
  - [rhel-media] ：Yum 软件仓库唯一标识符，避免与其他仓库冲突。
  - name=linuxchen：Yum 软件仓库的名称描述，易于识别仓库用处。 
  - baseurl=file:///media/cdrom：提供的方式包括 FTP（ftp://..）、HTTP（http://..）、本地（file:///..）。 
  - enabled=1：设置此源是否可用；1 为可用，0 为禁用。 
  - gpgcheck=1：设置此源是否校验文件；1 为校验，0 为不校验。
  - gpgkey=file:///media/cdrom/RPM-GPG-KEY-redhat-release：若上面参数开启校 验，那么请指定公钥文件地址。 
- 第3步：按配置参数的路径挂载光盘，并把光盘挂载信息写入到/etc/fstab 文件中。 第4步：使用“yum install httpd -y”命令检查 Yum 软件仓库是否已经可用。 

```shell
[root@chen network-scripts]# cd /etc/yum.repos.d/
[root@chen yum.repos.d]# vim centos7.repo
[root@chen yum.repos.d]# vim centos7.repo

[centos7]
name=centos7
baseurl=file:///media/cdrom
enabled=1
gpgcheck=0
```



![1650276984986](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1650276984986.png)

- 创建挂载点后进行挂载操作，并设置成开机自动挂载 (第六章会讲解)尝试使用 Yum 软 件仓库来安装 Web 服务，出现 **Complete！**则代表配置正确 ：

```shell
[root@chen yum.repos.d]# mkdir -p /media/cdrom
[root@chen yum.repos.d]# mount /dev/cdrom /media/cdrom
mount: /dev/sr0 写保护，将以只读方式挂载
[root@chen yum.repos.d]# vim /etc/fstab
[root@chen yum.repos.d]# vim /etc/fstab
[root@chen yum.repos.d]# yum install httpd
已安装:
  httpd.x86_64 0:2.4.6-97.el7.centos.5                                                       
作为依赖被安装:
  httpd-tools.x86_64 0:2.4.6-97.el7.centos.5                 mailcap.noarch 0:2.1.41-2.el7   完毕！
```



![1650277323165](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1650277323165.png)

![1650277398348](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1650277398348.png)

![1650277429712](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1650277429712.png)

## 4.2、编写Shell脚本

可以将 Shell 终端解释器当作人与计算机硬件之间的“翻译官”，它作为用户与 Linux 系 统内部的通信媒介，除了能够支持各种变量与参数外，还提供了诸如循环、分支等高级编程 语言才有的控制结构特性。要想正确使用 Shell 中的这些功能特性，准确下达命令尤为重要。 Shell 脚本命令的工作方式有两种：交互式和批处理。

​	**交互式（Interactive）：用户每输入一条命令就立即执行。**

​	**批处理（Batch）：由用户事先编写好一个完整的 Shell 脚本，Shell 会一次性执行脚本 中诸多的命令。** 

在 Shell 脚本中不仅会用到前面学习过的很多 Linux 命令以及正则表达式、管道符、数据 流重定向等语法规则，还需要把内部功能模块化后通过逻辑语句进行处理，最终形成日常所 见的 Shell 脚本 。

### 4.2.1、编写简单的脚本

使用Vim 编辑器把 Linux 命令按照顺序依次写入到一个 文件中，这就是一个简单的脚本了 

```shell
# 如果想查看当前所在工作路径并列出当前目录下所有的文件及属性信息，实现这个功能的脚本应该类似于下面这样
[root@chen ~]# vim chen.sh
#!/bin/bash
# For Ex BY chen
pwd
ls -al
[root@chen ~]# bash chen.sh
/root
总用量 120
dr-xr-x---. 17 root root 4096 4月  18 18:31 .
dr-xr-xr-x. 18 root root 4096 3月  25 03:11 ..
-rw-------.  1 root root 1476 4月  15 23:54 .bash_history
-rw-r--r--.  1 root root   18 12月 29 2013 .bash_logout
-rw-r--r--.  1 root root  176 12月 29 2013 .bash_profile
-rw-r--r--.  1 root root  176 12月 29 2013 .bashrc
drwx------. 13 root root 4096 3月  25 03:27 .cache
# 第二种运行脚本程序的方法是通过输入完整路径的方式来执行
# 但默认会因为权限不足而提示报错信息，此时只需要为脚本文件增加执行权限即可
[root@chen ~]# ./chen.sh
-bash: ./chen.sh: 权限不够
[root@chen ~]# chmod u+x chen.sh
[root@chen ~]# ./chen.sh
/root
总用量 120
dr-xr-x---. 17 root root 4096 4月  18 18:31 .
dr-xr-xr-x. 18 root root 4096 3月  25 03:11 ..
-rw-------.  1 root root 1476 4月  15 23:54 .bash_history
-rw-r--r--.  1 root root   18 12月 29 2013 .bash_logout
-rw-r--r--.  1 root root  176 12月 29 2013 .bash_profile
-rw-r--r--.  1 root root  176 12月 29 2013 .bashrc
drwx------. 13 root root 4096 3月  25 03:27 .cache
```



![1650277844511](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1650277844511.png)

![1650277922313](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1650277922313.png)

![1650278102785](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1650278102785.png)

### 4.2.2、接收用户的参数

```shell
#Linux 系统中的 Shell 脚本语言 已经内设了用于接收参数的变量，变量之间可以使用空格间隔。
# 例如$0 对应的是当前 Shell 脚本程序的名称，
# $#对应的是总共 有几个参数，
# $*对应的是所有位置的参数值，
# #?对应的是显示上一次命令的执行返回值，而 $1、$2、$3……则分别对应着第 N 个位置的参数值
[root@chen ~]# vim chen.sh
#!/bin/bash
# For Ex BY chen
echo "当前脚本名称为$0" 
echo "总共有$#个参数，分别是$*。" 
echo "第 1 个参数为$1，第 5 个为$5。"
[root@chen ~]# sh chen.sh one two three four five six
当前脚本名称为chen.sh
总共有6个参数，分别是one two three four five six。
第 1 个参数为one，第 5 个为five。
[root@chen ~]# 
```



![1650278614147](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1650278614147.png)

### 4.2.3、判断用户的参数

系统在执行 mkdir 命令时会判断用户输入的信息，即判断用户 指定的文件夹名称是否已经存在，如果存在则提示报错；反之则自动创建。Shell 脚本中的条 件测试语法可以判断表达式是否成立，若条件成立则返回数字 0，否则便返回其他随机数值 。

```shell
# 测试语句表达式: [ 条件表达式 ]([]两边均应有一个空格)
```

条件测试语句可以分为 4 种： 

- 文件测试语句；
- 逻辑测试语句； 
- 整数值比较语句；
- 字符串比较语句。 

**文件测试所用的参数**

| 运算符 |            作用            |
| :----: | :------------------------: |
|   -d   |   测试文件是否为目录类型   |
|   -e   |      测试文件是否存在      |
|   -f   |     判断是否为一般文件     |
|   -r   | 测试当前用户是否有权限读取 |
|   -w   | 测试当前用户是否有权限写入 |
|   -x   | 测试当前用户是否有权限执行 |

```shell
#使用文件测试语句来判断/etc/fstab 是否为一个目录类型的文件，然后通过 Shell 解释器的内设$?变量显示上一条命令执行后的返回值。如果返回值为 0，则目录存在；如果返回值为非零的值，则意味着目录不存在
[root@chen ~]# [ -d /etc/fstab ]
[root@chen ~]# echo $?
1
# 使用文件测试语句来判断/etc/fstab 是否为一般文件，如果返回值为 0，则代表文件存在，且为一般文件
[root@chen ~]# [ -f /etc/fstab ]
[root@chen ~]# echo $?
0
[root@chen ~]# 
# 此可以用来判断/dev/cdrom 文件是否存在，若存在则输出 Exist 字样;逻辑“与”的运算符号是&&
[root@chen ~]# [ -e /dev/cdrom ] && echo "Exist"
Exist
[root@chen ~]# echo $USER
root
# 逻辑“或”,运算符号为||
[root@chen ~]# [ $USER = root ] || echo "user"
[root@chen ~]# su - chen
上一次登录：四 4月 14 23:07:57 CST 2022pts/2 上
[chen@chen ~]$ [ $USER = root ] || echo "user"
user
[chen@chen ~]$ exit
登出
# 逻辑“非”的运算符号叹号(!)
[root@chen ~]# [ ! $USER  = root ] || echo "Administrator"
Administrator
[root@chen ~]# 

```



![1650286601070](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1650286601070.png)



**可用的整数比较运算符**

| 运算符 | 作用           |
| :----- | -------------- |
| -eq    | 是否等于       |
| -ne    | 是否不等于     |
| -gt    | 是否大于       |
| -lt    | 是否小于       |
| -le    | 是否等于或小于 |
| -ge    | 是否大于或等于 |

 ```shell
[root@chen ~]# [ 10 -gt 10 ]
[root@chen ~]# echo $?
1
[root@chen ~]# [ 10 -eq 10 ]
[root@chen ~]# echo $?
0
[root@chen ~]# 
 ```



![1650286959081](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1650286959081.png)

**常见的字符串比较运算符**

| 运算符 |          作用          |
| :----: | :--------------------: |
|   =    | 比较字符串内容是否相同 |
|   !=   | 比较字符串内容是否不同 |
|   -z   | 判断字符串内容是否为空 |

```shell
# 通过判断 String 变量是否为空值，进而判断是否定义了这个变量
[root@chen ~]# [ -z $String ]
[root@chen ~]# echo $?
0
[root@chen ~]# [ -z $String "chen" ]
[root@chen ~]# echo $?
1
[root@chen ~]# echo $LANG
zh_CN.UTF-8
[root@chen ~]# [ $LANG != "en.US" ] && echo "Not en.US"
Not en.US
[root@chen ~]# 
```

 

![1650287354516](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1650287354516.png)

## 4.3、流程控制语句

使用Linux命令、管道符、重定向以及条件测试语句来编写最基本的shell脚本，但是不适合生产环境，接下来我们将学习if、for、while、case四种流程语句来学习编写更强大、功能更强的脚本。

### 4.3.1、if条件测试语句

+ if 条件测试语句可以让脚本根据实际情况自动执行相应的命令。从技术角度来讲，if 语 句分为单分支结构、双分支结构、多分支结构；
+ **if 条件语句的单分支结构由 if、then、fi 关键词组成 相当于口语的“如果……那么……”**  

![1650371103107](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1650371103107.png)

```shell
# 下面使用单分支的 if 条件语句来判断/chen/study 目录是否存在，若存在就结束条件判断和整个 Shell 脚本，反之则去创建这个目录
[root@chen ~]# vim mkstudy.sh

#!/bin/bash
DIR="/chen/study"
if [ ! -e $DIR ]
then
mkdir -p $DIR
fi
# 执行脚本
[root@chen ~]# bash mkstudy.sh 
[root@chen ~]# ls -d /chen/study
/chen/study
```



![1650370414289](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1650370414289.png)

![1650370501418](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1650370501418.png)

- **if 条件语句的双分支结构由 if、then、else、fi 关键词组成** 

![1650371089387](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1650371089387.png)

```shell
# 使用双分支的 if 条件语句来验证某台主机是否在线，然后根据返回值的结果，要么 显示主机在线信息，要么显示主机不在线信息
[root@chen ~]# vim chkhost.sh

#!/bin/bash
ping -c 3 -i 0.2 -W 3 $1 &> /dev/null
if [ $? -eq 0 ]
then
echo "Host $1 id On-line."
else
echo "Host $1 is Off-line."
fi
# 执行脚本
[root@chen ~]# vim chkhost.sh
[root@chen ~]# bash chkhost.sh 192.168.200.130
Host 192.168.200.130 id On-line.
[root@chen ~]# bash chkhost.sh 192.168.200.131
Host 192.168.200.131 is Off-line.
[root@chen ~]# 

```



![1650370930759](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1650370930759.png)

- **if 条件语句的多分支结构由 if、then、else、elif、fi 关键词组成相当于口语的“如 果……那么……如果……那么……”** 

![1650371074841](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1650371074841.png)

```shell
[root@chen ~]# vim chkcore.sh

#!/bin/bash
read -p "Enter your score (0-100):" GRADE
if [ $GRADE -ge 85 ] && [ $GRADE -le 100 ] ; then
echo "$GRADE is Excellent"
elif [ $GRADE -ge 70 ] && [ $GRADE -le 84 ] ; then
echo "$GRADE is PASS"
elif [ $GRADE -ge 0 ] && [ $GRADE -le 69 ] ; then
echo "$GRADE is Fail"
else
echo "Plase enter legal score"
fi
# 执行脚本
[root@chen ~]# vim chkcore.sh
[root@chen ~]# bash chkcore.sh 
Enter your score (0-100):88
88 is Excellent
[root@chen ~]# bash chkcore.sh 200
Enter your score (0-100):200   
Plase enter legal score

```



![1650372101348](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1650372101348.png)

### 4.3.2、for条件循环语句

**for 循环语句允许脚本一次性读取多个信息，然后逐一对信息进行操作处理，当要处理的数据 有范围时，使用 for 循环语句再适合不过了**

![1650457349528](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1650457349528.png)

```shell
# 使用 for 循环语句从列表文件中读取多个用户名，然后为其逐一创建用户账户并设置密码
# 1.首先创建用户名称的列表文件 users.txt，每个用户名称单独一行
[root@chen ~]# vim user.txt
chen
zj
ldz
wxy
# 2.接下来编写 Shell 脚本 chen.sh
# /dev/null 是一个被称作 Linux 黑洞的文件，把输出信息重定向到这个文件等同于删除数据（类似于没有回收功能的垃圾箱），可以让用户的屏幕窗口保持简洁
[root@chen ~]# vim chen.sh

#!/bin/bash
# For Ex BY chen
read -p "Enter The Users Password : " PASSWD
for UNAME in `cat user.txt`
do 
        id $UNAME &> /dev/null
        if [ $? -eq 0 ]
        then
                echo "$UNAME , Already exists"
        else 
                useradd $UNAME &> /dev/null
                echo "$PASSWD" | passwd --stdin $UNAME &> /dev/null
                echo "$UNAME , Create success"
        fi
done
# 执行批量创建用户的 Shell 脚本 chen.sh，在输入为账户设定的密码后将由脚本自动检查并创建这些账户。
[root@chen ~]# vim user.txt
[root@chen ~]# vim chen.sh
[root@chen ~]# vim chen.sh
[root@chen ~]# bash chen.sh
Enter The Users Password : linux
chen , Already exists
zj , Create success
ldz , Create success
wxy , Create success
# 如果想确认这个脚本是否成功创建了用户账户，可以打开这个文件，看其中是否有这些新创建的用户信息 
[root@chen ~]# tail -6 /etc/passwd
tcpdump:x:72:72::/:/sbin/nologin
chen:x:1000:1000:chen:/home/chen:/bin/bash
apache:x:48:48:Apache:/usr/share/httpd:/sbin/nologin
zj:x:1001:1001::/home/zj:/bin/bash
ldz:x:1002:1002::/home/ldz:/bin/bash
wxy:x:1003:1003::/home/wxy:/bin/bash
[root@chen ~]# 
```

 

![1650459092940](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1650459092940.png)

```shell
# 让脚本从文本中自动读取主机列表，然后自动逐个测试这些主机是否在线
[root@chen ~]# vim ipadds.txt

192.168.200.130
192.168.200.131
192.168.200.132
# 双分支 if 条件语句与 for 循环语句相结合，让脚本从主机列表文件 ipadds.txt中自动读取 IP 地址（用来表示主机）并将其赋值给 HLIST 变量，从而通过判断 ping 命令执行后的返回值来逐个测试主机是否在线
[root@chen ~]# vim CheckHosts.sh

#!/bin/bash
HLIST=$(cat ~/ipadds.txt)
for IP in $HLIST
do
        ping -c 3 -i 0.2 -W 3 $IP &> /dev/null
        if [ $? -eq 0 ]
        then
                echo "Host $IP is On-line."
        else
                echo "Host $IP is Off-line."
        fi
done
[root@chen ~]# ./CheckHosts.sh
-bash: ./CheckHosts.sh: 权限不够
[root@chen ~]# chmod u+x CheckHosts.sh 
[root@chen ~]# ./CheckHosts.sh
Host 192.168.200.130 is On-line.
Host 192.168.200.131 is Off-line.
Host 192.168.200.132 is Off-line.
[root@chen ~]#
```



![1650459625882](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1650459625882.png)



### 4.3.3、while条件循环语句

**while 条件循环语句是一种让脚本根据某些条件来重复执行while 循环语句通过判断条件测试的真假来决定是否继续执行命令，若条件为真就继续执行， 为假就结束循环 命令的语句** 

![1650464536418](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1650464536418.png)



```shell
# 接下来结合使用多分支的 if 条件测试语句与 while 条件循环语句，编写一个用来猜测数值大小的脚本 Guess.sh
[root@chen ~]# vim Guess.sh
[root@chen ~]# vim Guess.sh

#!/bin/bash
PRICE=$(expr $RANDOM % 100)
TIMES=0
echo "商品的价格在0-99之间，猜猜看是多少？"
while true
do
        read -p "请输入你猜的价格数目：" INT
let TIMES++
if [ $INT -eq $PRICE ] ; then
        echo "恭喜你答对了，商品的实际价格 $PRICE."
        echo "你共猜测了 $TIMES 次"
        exit 0
        elif [ $INT -gt $PRICE ] ; then
                echo "太高了！" 
        else
                echo "太低了！" 
        fi
done
# 执行脚本
[root@chen ~]# bash Guess.sh 
商品的价格在0-99之间，猜猜看是多少？
请输入你猜的价格数目：90
太高了！
请输入你猜的价格数目：85
太高了！
请输入你猜的价格数目：60
太高了！
请输入你猜的价格数目：25
太低了！
请输入你猜的价格数目：45
太高了！
请输入你猜的价格数目：42
太高了！
请输入你猜的价格数目：39
太高了！
请输入你猜的价格数目：30
太低了！
请输入你猜的价格数目：36
太低了！
请输入你猜的价格数目：38
恭喜你答对了，商品的实际价格 38.
你共猜测了 10 次
[root@chen ~]# 

```



![1650465258956](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1650465258956.png)



### 4.3.4、case条件测试语句

**case 语句是在多个范围内匹配 数据，若匹配成功则执行相关命令并结束整个条件测试；而如果数据不在所列出的范围内， 则会去执行星号（*）中所定义的默认命令** 

![1650465367631](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1650465367631.png)

```shell
# 我们编写脚本 Checkkeys.sh，提示用户输入一个字符并将其赋值给变量 KEY， 然后根据变量 KEY 的值向用户显示其值是字母、数字还是其他字符
[root@chen ~]# vim CheckKeys.sh

#!/bin/bash
read -p "请输入一个字符，并按Enter键确认：" KEY
case "$KEY" in
        [a-z]|[A-Z])
                echo "您输入的是 字母。"
                ;;
        [0-9])
                echo "您输入的是 数字。"
                ;;
        *)
                echo "您输入的是空格 、功能键或其他控制字符。"
esac
# 保存并执行程序
[root@chen ~]# vim CheckKeys.sh
[root@chen ~]# bash CheckKeys.sh 
请输入一个字符，并按Enter键确认：Z
您输入的是 字母。
[root@chen ~]# bash CheckKeys.sh 
请输入一个字符，并按Enter键确认：3
您输入的是 数字。
[root@chen ~]# bash CheckKeys.sh 
请输入一个字符，并按Enter键确认： 
您输入的是空格 、功能键或其他控制字符。
[root@chen ~]# 
```

## 4.4、计划任务服务程序

1. **计划任务服务程序就是可以`使得 Linux 在无需人为介入的情况下，在指定的时间段自动 启用或停止某些服务或命令，从而实现运维的自动化`** 
2. **计划任务分为一次性计划任务与长期性计划任务，大家可以按照如下方式理解。 **
3. 一次性计划任务：今晚 11 点 30 分开启网站服务。 
4. 长期性计划任务：每周一的凌晨 3 点 25 分把/home/wwwroot 目录打包备份为 backup.tar.gz

```shell
# “at 时间”：一次性计划任务
# “at -l”命令：查看已设置好但还未执行的一次性计划任务
# “atrm 任务序号”：删除任务
[root@chen ~]# at 23:20
at> systemctl restart network  
at> 此处请同时按下 Ctrl + D 组合键来结束编写计划任务
job 2 at Wed Apr 20 23:20:00 2022
[root@chen ~]# at -l
2	Wed Apr 20 23:20:00 2022 a root
[root@chen ~]# atrm 2
[root@chen ~]# at -l
# at 命令接收前面 echo 命令的输出信息，以达到通过非交互式的方式创建计划一次性任务的目的
[root@chen ~]# echo "systemctl restart network" | at 23:30
job 3 at Wed Apr 20 23:30:00 2022
[root@chen ~]# at -l
3	Wed Apr 20 23:30:00 2022 a root
[root@chen ~]# 
```



![1650468441867](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1650468441867.png)

5. **如果我们希望 Linux 系统能够周期性地、有规律地执行某些具体的任务，那么 Linux 系统 中默认启用的 crond 服务简直再适合不过了** 

- crontab -e : 创建、编辑计划任务 
- crontab -l : 查看当前计划任务
- crontab -r : 删除某条计划任务 
- crontab -u: 编辑他人的计划任务 

6. **在正式部署计划任务前，请先跟刘遄老师念一下口诀“分、时、日、月、星期 命令”需要注意的是，如果有些字段 没有设置，则需要使用星号（*）占位**

![1650467966779](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1650467966779.png)



**使用 crond 设置任务的参数字段说明** 

| 字段 |                  说明                   |
| :--: | :-------------------------------------: |
|  分  |            取值为0~59的整数             |
|  时  |          取值为0~23的任意整数           |
|  日  |          取值为1~31的任意整数           |
|  月  |          取值为1~12的任意整数           |
| 星期 | 取值为0-7的任意整数，其中0与7均为星期日 |
| 命令 |         要执行的命令或程序脚本          |

```shell
# 假设在每周一、三、五的凌晨 3 点 25 分，都需要使用 tar 命令把某个网站的数据目录进行打包处理，使其作为一个备份文件
# crontab -e 命令来创建计划任务
[root@chen ~]# crontab -e

25 3 * * 1,3,5 /usr/bin/tar -czvf backup.tar.gz /home/wwwroot
[root@chen ~]# crontab -e
no crontab for root - using an empty one 
crontab: installing new crontab
[root@chen ~]# crontab -l
25 3 * * 1,3,5 /usr/bin/tar -czvf backup.tar.gz /home/wwwroot 
[root@chen ~]# 

```



**注意：**`需要说明的是，除了用逗号（,）来分别表示多个时间段，例如“8,9,12”表示 8 月、9 月 和 12 月。还可以用减号（-）来表示一段连续的时间周期（例如字段“日”的取值为“12-15”， 则表示每月的 12～15 日）。以及用除号（/）表示执行任务的间隔时间（例如“/2”表示每隔 2 分钟执行一次任务）之外 `

![1650468714928](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1650468714928.png)

**在 crond 服务中需要同时包含多条计划任务的命令语句，应每行仅写一条。**

```shell
[root@chen ~]# whereis rm
rm: /usr/bin/rm /usr/share/man/man1/rm.1.gz /usr/share/man/man1p/rm.1p.gz
[root@chen ~]# 
# 我们再 添加一条计划任务，它的功能是每周一至周五的凌晨 1 点钟自动清空/tmp 目录内的所有文件
[root@chen ~]# crontab -e
25 3 * * 1,3,5 /usr/bin/tar -czvf backup.tar.gz /home/wwwroot
0 1 * * 1-5 /usr/bin/rm -rf /tmp/*
[root@chen ~]# crontab -l
25 3 * * 1,3,5 /usr/bin/tar -czvf backup.tar.gz /home/wwwroot 
0 1 * * 1-5 /usr/bin/rm -rf /tmp/*
[root@chen ~]# 
```

 

![1650469000846](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1650469000846.png)

### 注意事项：

- **在 crond 服务的配置参数中，可以像 Shell 脚本那样以#号开头写上注释信息，这样 在日后回顾这段命令代码时可以快速了解其功能、需求以及编写人员等重要信息。**
- **计划任务中的“分”字段必须有数值，绝对不能为空或是*号，而“日”和“星期” 字段不能同时使用，否则就会发生冲突** 





## 5、用户身份与权限

### 5.1、用户身份与能力

管理员 UID 为 0：系统的管理员用户。

系统用户 UID 为 1～999： Linux 系统为了避免因某个服务程序出现漏洞而被黑客提 权至整台服务器，默认服务程序会有独立的系统用户负责运行，进而有效控制被破坏 范围。

普通用户 UID 从 1000 开始：是由管理员创建的用于日常工作的用户。 需要注意的是，UID 是不能冲突的，而且管理员创建的普通用户的 UID 默认是从 1000 开始的（即使前面有闲置的号码） 

#### 5.1.1、id命令

- id命令用于显示用户的详细信息，语法格式为“id 用户名”

```shell
[root@chen home]# id chen
uid=1000(chen) gid=1000(chen) groups=1000(chen)
[root@chen home]# 
```



#### 5.1.2、useradd命令

- **useradd 命令用于创建新的用户，格式为“useradd [选项] 用户名”** 
- **使用useradd命令创建用户时，默认的家目录会被存放在/home目录中**

**useradd 命令中的用户参数以及作用** 

| 参数 |                  作用                  |
| :--: | :------------------------------------: |
|  -d  | 指定用户的家目录(默认为/home/username) |
|  -e  |    账户的到期时间，格式为YYYY-MM-DD    |
|  -u  |          指定该用户的默认UID           |
|  -g  |    指定一个初始的用户基本组(已存在)    |
|  -G  |        指定一个或多个扩展用户组        |
|  -N  |      不创建与用户同名的基本用户组      |
|  -s  |      指定该用户默认的Shell解释器       |

- 使用useradd创建一个名称为zj的用户，并使用id命令确认信息

```shell
[root@chen home]# useradd zj
[root@chen home]# id zj
uid=1001(zj) gid=1001(zj) groups=1001(zj)
[root@chen home]# 
```

- 创建一个普通用户并指定家目录、用户的UID以及Shell解释器
- /sbin/nologin解释器一旦被设置，则代表该用户不能登录到系统中

```shell
[root@chen home]# useradd -d /home/linuxcool -u 8888 -s /sbin/nologin linux
[root@chen home]# id linux
uid=8888(linux) gid=8888(linux) groups=8888(linux)
[root@chen home]# 
```

#### 5.1.3、groupadd命令

- groupadd命令用于创建新的用户组，语法格式为"groupadd[参数] 群组名"
  - -g: 指定新建工作组的id
  - -r: 创建系统工作组，系统工作组的组ID小于500
  - -k: 覆盖配置文件"/etc/login.defs"
  - -o: 允许添加ID号不唯一的工作组
- 为了更加高效的指派系统中各个用户的权限，在工作中常常会把几个用户加入到同一组里面，这样便可以针对一类用户统一安排权限

```shell
[root@chen ~]# groupadd ronny
[root@chen ~]# 
```

#### 5.1.4、usermod命令

- usermod命令用于修改用户的属性，英文全称为“user modify“，语法格式为”usermod [参数] 用户名“
- 用户信息保存在/etc/passwd文件中，可以直接用文本编辑器来修改配置

**usermod命令中的参数以及作用**

|     参数     |                             作用                             |
| :----------: | :----------------------------------------------------------: |
| -d<登入目录> |                     修改用户登入时的目录                     |
| -e<有效期限> |                      修改账号的有效期限                      |
| -f<缓冲天数> |     修改在密码过期后多少天即关闭该账号格式为：YYYY-MM-DD     |
|   -g<群组>   |                      修改用户所属的群组                      |
|   -G<群组>   |                    修改用户所属的附加群组                    |
| -l<账号名称> |                       修改用户账号名称                       |
|      -L      |                   锁定用户密码，使密码无效                   |
|  -s<shell>   |                 修改用户登入后所使用的shell                  |
|   -u<uid>    |                          修改用户ID                          |
|      -U      |                         解除密码锁定                         |
|    -d -m     | 参数-m与参数-d连用，可重新指定用户家目录并自动把就的数据转移过去 |
|   -c<备注>   |                    修改用户账号的备注文字                    |



```shell
# 查看linux用户的信息
[root@chen ~]# id linux
uid=8888(linux) gid=8888(linux) groups=8888(linux)

# 将用户linux加入到ronny用户组中去
[root@chen ~]# usermod -G ronny linux
[root@chen ~]# id linux
uid=8888(linux) gid=8888(linux) groups=8888(linux),8889(ronny)

# 修改linux用户的UID号码值
[root@chen ~]# usermod -u 8889 linux
[root@chen ~]# id linux
uid=8889(linux) gid=8888(linux) groups=8888(linux),8889(ronny)

# 把用户的解释器终端由默认的/bin/bash修改为/sbin/nologin
[root@chen ~]# usermod -s /sbin/nologin linux
usermod: no changes
[root@chen ~]# su - linux
This account is currently not available.
```

#### 5.1.5、passwd命令

- passwd命令用于修改用户的密码、过期时间等信息，英文全称为"password",语法格式为"passwd [参数] 用户名"
- 普通用户只能使用passwd命令修改自己的系统密码，root管理员则有权限修改其他所有人的密码

**passwd命令中的参数以及作用**

|  参数   |                             作用                             |
| :-----: | :----------------------------------------------------------: |
|   -l    |               锁定用户密码，无法被用户自行修改               |
|   -u    |             解开已锁定用户密码，允许用户自行修改             |
|   -e    |              密码立即过期，下次登陆强制修改密码              |
|   -k    |             保留即将过期的用户在期满后能仍能使用             |
|   -S    |         查询密码状态，以及密码所采用的加密算法的名称         |
|   -d    |                  使该用户可用空密码登录系统                  |
| --stdin | 通过标准输入修改用户密码，如echo"NewPassword" \| passwd --stdin Username |

```shell
# 修改自己的密码
[root@chen ~]# passwd
Changing password for user root.
New password: 此处输入密码值
BAD PASSWORD: The password is shorter than 7 characters
Retype new password: 再次输入进行确认
passwd: all authentication tokens updated successfully.

# 修改其他人的密码，需要先判断是否为root用户
[root@chen ~]# passwd linux
Changing password for user linux.
New password: 此处输入密码值
BAD PASSWORD: The password is shorter than 8 characters
Retype new password: 再次输入进行确认
passwd: all authentication tokens updated successfully.
[root@chen ~]# 
# 假设您有位同事正在度假，而且假期很长，那么可以使用 passwd 命令禁止该用户登录系统，等假期结束回归工作岗位时，再使用该命令允许用户登录系统，而不是将其删除
[root@chen ~]# passwd -l linux
Locking password for user linux.
passwd: Success
[root@chen ~]# passwd -S linux
linux LK 2022-04-23 0 99999 7 -1 (Password locked.)

# 在解锁时也要使用管理员身份
[root@chen ~]# passwd -u linux
Unlocking password for user linux.
passwd: Success
[root@chen ~]# passwd -S linux
linux PS 2022-04-23 0 99999 7 -1 (Password set, SHA512 crypt.)
```

#### 5.1.6、userdel

- **userdel 命令用于删除用户，格式为“userdel [选项] 用户名”** 

**userdel 命令的参数以及作用** 

| 参数 |              作用              |
| :--: | :----------------------------: |
|  -r  | 删除用户主目录及其中的任何文件 |
|  -h  |       显示命令的帮助信息       |
|  -f  |          强制删除用户          |

```shell
# 在删除一个用户时，一般不建议删除他的家目录，使用userdel不加参数就行
[root@chen ~]# userdel linux
[root@chen ~]# id linux
id: linux: no such user
# 现在用户虽被删除了，但家目录数据会继续存放在/home目录中
[root@chen ~]# cd /home
[root@chen home]# ls
chen  linux  linuxcool  readme.txt  workdir  zj
```



### 5.2、文件权限与归属

尽管在 Linux 系统中一切都是文件，但是每个文件的类型不尽相同，因此 Linux 系统使 用了不同的字符来加以区分，常见的字符如下所示。

- -：普通文件。 
- d：目录文件。
- l：链接文件。
- b：块设备文件。
- c：字符设备文件。
- p：管道文件。



在 Linux 系统中，每个文件都有所属的所有者和所有组，并且规定了文件的所有者、 所有组以及其他人对文件所拥有的可读（r）、可写（w）、可执行（x）等权限。 

这里给大家详细讲解一下目录文件的权限设置。对目录文件来说，“可读”表 示能够读取目录内的文件列表；“可写”表示能够在目录内新增、删除、重命名文件；而“可 执行”则表示能够进入该目录。 



**可读、可写、可执行权限对应的命令在文件和目录上是有区别的** 

![第5章 用户身份与文件权限第5章 用户身份与文件权限](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md%E8%AF%BB%E5%86%99%E6%89%A7%E8%A1%8C%E6%9D%83%E9%99%90%E5%AF%B9%E4%BA%8E%E6%96%87%E4%BB%B6%E4%B8%8E%E7%9B%AE%E5%BD%95%E7%9A%84%E4%BD%9C%E7%94%A8-1024x168.png) 

**文件的可读、可写、可执行权限的英文全称分别是read、write、execute，可以简写为r、w、x，亦可分别用数字4、2、1来表示，文件所有者、文件所属组及其他用户权限之间无关联** 

![第5章 用户身份与文件权限第5章 用户身份与文件权限](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md%E6%96%87%E4%BB%B6%E6%9D%83%E9%99%90%E7%9A%84%E5%AD%97%E7%AC%A6%E4%B8%8E%E6%95%B0%E5%AD%97%E8%A1%A8%E7%A4%BA-1024x201.png) 

**这里以rw-r-x-w-权限为例来介绍如何将字符表示的权限转换为数字表示的权限。首先，要将各个位上的字符替换为数字** 

**数字6是由4+2得到的，不可能是4+1+1（因为每个权限只会出现一次，不可能同时有两个x执行权限）；数字5则是4+1得到的；数字2是本身，没有权限即是空值0。接下来按照上表所示的格式进行书写，得到420401020这样一串数字。有了这些信息就好办了，就可以把这串数字转换成字母了，如图5-2所示。** 

![第5章 用户身份与文件权限第5章 用户身份与文件权限](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md%E6%9D%83%E9%99%90%E8%BD%AC%E6%8D%A2-300x136.jpg)        ![第5章 用户身份与文件权限第5章 用户身份与文件权限](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md%E6%95%B0%E5%AD%97%E8%BD%AC%E6%8D%A2-300x136.jpg) 

图5-1 字符与数字权限转换示意图 				图5-2 数字与字符权限转换示意图 

**大家一定要心中牢记，文件的所有者、所属组和其他用户的权限之间无关联。一定不要写成rrwwx----的样子，一定要把rwx权限位对应到正确的位置，写成rw-r-x-w-** 

## 5.3、文件的特殊权限

**在复杂多变的生产环境中，单纯设置文件的rwx权限无法满足我们对安全和灵活性的需求，因此便有了SUID、SGID与SBIT的特殊权限位。这是一种对文件权限进行设置的特殊功能，可以与一般权限同时使用，以弥补一般权限不能实现的功能。**

**下面具体解释这3个特殊权限位的功能以及用法**

### 5.3.1、SUID

- **SUID是一种对二进制程序进行设置的特殊权限，能够让二进制程序的执行者临时拥有所有者的权限（仅对拥有执行权限的二进制程序有效）** 

```shell
[root@chen ~]# ls -l /etc/shadow
----------. 1 root root 1307 Apr 24 17:19 /etc/shadow
[root@chen ~]# ls -l /bin/passwd
-rwsr-xr-x. 1 root root 27856 Apr  1  2020 /bin/passwd
[root@chen ~]# 
```

**注意：**

​	**/bin/passwd用来告诫用户一定要小心这个权限，因为一旦某个命令文件被设置了SUID权限，就意味着凡是执行该文件的人都可以临时获取到文件所有者所对应的更高权限。因此，千万不要将SUID权限设置到vim、cat、rm等命令上面！！！** 

### 5.3.2、SGID

- **SGID特殊权限有两种应用场景：当对二进制程序进行设置时，能够让执行者临时获取文件所属组的权限；当对目录进行设置时，则是让目录内新创建的文件自动继承该目录原有用户组的名称** 

**SGID的第一种功能是参考SUID而设计的，不同点在于执行程序的用户获取的不再是文件所有者的临时权限，而是获取到文件所属组的权限**

```shell
#用到的就是SGID的第二个功能，即在某个目录中创建的文件自动继承该目录的用户组（只可以对目录进行设置）
[root@chen ~]# su - chenhao
[chenhao@chen ~]$ cd /tmp
[chenhao@chen tmp]$ mkdir testdir
[chenhao@chen tmp]$ ls -ald testdir
drwxrwxr-x. 2 chenhao chenhao 4096 Apr 25 20:16 testdir
[chenhao@chen tmp]$ chmod -R 777 testdir
[chenhao@chen tmp]$ ls -ald testdir
drwxrwxrwx. 2 chenhao chenhao 4096 Apr 25 20:16 testdir
[chenhao@chen tmp]$ 
# 在使用上述命令设置好目录的777权限（确保普通用户可以向其中写入文件），并为该目录设置了SGID特殊权限位后，就可以切换至一个普通用户，然后尝试在该目录中创建文件，并查看新创建的文件是否会继承新创建的文件所在的目录的所属组名称
[chenhao@chen ~]$ cd /tmp/testdir/
[chenhao@chen testdir]$ echo "linux.com" > test
[chenhao@chen testdir]$ ls -al test
-rw-rw-r--. 1 chenhao chenhao 10 Apr 25 20:19 test
[chenhao@chen testdir]$ 

```

- **再介绍两个与本节内容相关的命令：chmod和chown** 
- **chmod命令用于设置文件的一般权限及特殊权限**

**语法格式：** chmod [参数  文件] 

- **常用参数：** 

| -c   | 若该文件权限确实已经更改，才显示其更改动作                   |
| ---- | ------------------------------------------------------------ |
| -f   | 若该文件权限无法被更改也不显示错误讯息                       |
| -v   | 显示权限变更的详细资料                                       |
| -R   | 对目前目录下的所有文件与子目录进行相同的权限变更(即以递回的方式逐个变更) |

**参考实例**

将档案 file1.txt 设为所有人皆可读取：

```shell
[root@chen ~]# mkdir file.txt
[root@chen ~]# chmod a+r file.txt  
```

将目前目录下的所有文件与子目录皆设为任何人可读取 :

```shell
[root@chen ~]# chmod -R a+r *   
```

将 file.txt 设定为只有该文件拥有者可以执行：

```shell
[root@chen ~]# chmod u+x file.txt
```

- **chown命令 – 改变文件或目录用户和用户组**

**语法格式：**chown [参数]

**常用参数：**

| -R        | 对目前目录下的所有文件与子目录进行相同的拥有者变更   |
| --------- | ---------------------------------------------------- |
| -c        | 若该文件拥有者确实已经更改，才显示其更改动作         |
| -f        | 若该文件拥有者无法被更改也不要显示错误讯息           |
| -h        | 只对于连结(link)进行变更，而非该 link 真正指向的文件 |
| -v        | 显示拥有者变更的详细资料                             |
| --help    | 显示辅助说明                                         |
| --version | 显示版本                                             |

**参考实例**

**将test.txt文件用户组与用户都改为bin：**

```
[root@chen ~]# ll test.txt.bz2 
-rw-r--r-- 1 root root 56 Jul 22 20:17 test.txt.bz2
[root@chen ~]# chown bin:bin test.txt.bz2    
[root@chen ~]# ll test.txt.bz2
-rw-r--r-- 1 bin bin 56 Jul 22 20:17 test.txt.bz2
```

**显示改动动作：**

```
[root@chen ~]# ll test.txt 
-rw-r--r-- 1 root root 45 Jul 22 21:11 test.txt
[root@chen ~]# chown -c bin:bin test.txt 
changed ownership of `test.txt' to bin:bin
```

**将当前目录下所有文件的拥有者都改为 chen，用户组改为 chengroup:**

```
[root@chen ~]# chown -R chen:chengroup *  
```



 ### 5.3.3、SBIT

SBIT特殊权限位可确保用户只能删除自己的文件，而不能删除其他用户的文件。换句话说，当对某个目录设置了SBIT粘滞位权限后，那么该目录中的文件就只能被其所有者执行删除操作了 

与前面所讲的SUID和SGID权限显示方法不同，当目录被设置SBIT特殊权限位后，文件的其他用户权限部分的x执行权限就会被替换成t或者T—原本有x执行权限则会写成t，原本没有x执行权限则会被写成T 。

**/tmp目录上的SBIT权限默认已经存在，这体现为“其他用户”权限字段的权限变为rwt：**

```shell
[root@chen ~]# ls -ald /tmp
drwxrwxrwt. 75 root root 12288 Apr 25 20:38 /tmp
```

文件能否被删除并不取决于自身的权限，而是看其所在目录是否有写入权限。为了避免现在很多读者不放心，所以下面的命令还是赋予了这个test文件最大的777权限（rwxrwxrwx） :

```shell
[root@chen ~]# cd /tmp
[root@chen tmp]# echo "Welcome to linux" > test
[root@chen tmp]# chmod 777 test
[root@chen tmp]# ls -al test
-rwxrwxrwx. 1 root root 17 Apr 25 20:41 test
[root@chen tmp]# 
```

随后，切换到一个普通用户身份下，尝试删除这个由其他人创建的文件，这时就会发现，即便读、写、执行权限全开，但是由于SBIT特殊权限位的缘故，依然无法删除该文件： 

```shell
[root@chen tmp]# su - linux
[linux@chen ~]$ cd /tmp
[linux@chen tmp]$ rm -f test
rm: cannot remove ‘test’: Operation not permitted
[linux@chen tmp]$ 
```

- SUID、SGID、SBIT特殊权限的设置参数 

| 参数 |     作用     |
| :--: | :----------: |
| u+s  | 设置SUID权限 |
| u-s  | 取消SUID权限 |
| g+s  | 设置SGID权限 |
| g-s  | 取消SGID权限 |
| o+t  | 设置SBIT权限 |
| o-t  | 取消SBIT权限 |

## 5.4、文件的隐藏属性

Linux系统中的文件除了具备一般权限和特殊权限之外，还有一种隐藏权限，即被隐藏起来的权限，默认情况下不能直接被用户发觉 

**既然叫隐藏权限，那么使用常规的ls命令肯定不能看到它的真面目。隐藏权限的专用设置命令是chattr，专用查看命令是lsattr** 

### 5.4.1、chattr命令

- chattr命令用于设置文件的隐藏权限，英文全称为change attributes,语法格式为"chattr [参数] 文件名称"。
- 想要把某个隐藏功能添加到文件上，则需要在命令后面追加“+参数”，如果想要把某个隐藏功能移出文件，则需要追加“-参数”。chattr命令中可供选择的隐藏权限参数非常丰富 



- chattr命令中的参数及其作用 

| 参数 | 作用                                                         |
| ---- | ------------------------------------------------------------ |
| i    | 无法对文件进行修改；若对目录设置了该参数，则仅能修改其中的子文件内容而不能新建或删除文件 |
| a    | 仅允许补充（追加）内容，无法覆盖/删除内容（Append Only）     |
| S    | 文件内容在变更后立即同步到硬盘（sync）                       |
| s    | 彻底从硬盘中删除，不可恢复（用0填充原文件所在硬盘区域）      |
| A    | 不再修改这个文件或目录的最后访问时间（atime）                |
| b    | 不再修改文件或目录的存取时间                                 |
| D    | 检查压缩文件中的错误                                         |
| d    | 使用dump命令备份时忽略本文件/目录                            |
| c    | 默认将文件或目录进行压缩                                     |
| u    | 当删除该文件后依然保留其在硬盘中的数据，方便日后恢复         |
| t    | 让文件系统支持尾部合并（tail-merging）                       |
| x    | 可以直接访问压缩文件中的内容                                 |

- 我们新建一个普通文件，然后立即尝试删除

```shell
[root@chen ~]# echo "for Test" > test
[root@chen ~]# rm test
rm: remove regular file ‘test’? y
[root@chen ~]# 
```

- 接下来再次新建一个普通文件，并为其设置“不允许删除与覆盖”（+a参数）权限，然后再尝试将这个文件删除 

```shell
[root@chen ~]# echo "for Test" > test
[root@chen ~]# chattr +a test
[root@chen ~]# rm test
rm: remove regular file ‘test’? y
rm: cannot remove ‘test’: Operation not permitted
[root@chen ~]# 
```

### 5.4.2、lsattr命令

- **lsattr命令用于查看文件的隐藏权限，英文全称为“list attributes”，语法格式为“lsattr [参数] 文件名称”**

   

- Linux系统中，文件的隐藏权限必须使用lsattr命令来查看，平时使用的ls之类的命令则看不出 ：

```shell
[root@chen ~]# ls -al test
-rw-r--r--. 1 root root 9 Apr 25 21:05 test
```

- **一旦使用lsattr命令后，文件上被赋予的隐藏权限马上就会原形毕露：**

```shell
[root@chen ~]# lsattr test
-----a-------e-- test
```

- 此时按照显示的隐藏权限的类型（字母），使用chattr命令将其去掉： 

```shell
[root@chen ~]# chattr -a test
[root@chen ~]# lsattr test
-------------e-- test
[root@chen ~]# rm test
rm: remove regular file ‘test’? y
[root@chen ~]# ls
chen  linux  test1  下载  公共  图片  文档  桌面  模板  视频  音乐
[root@chen ~]#
```

## 5.5、文件访问控制列表

不知道大家是否发现，前文讲解的一般权限、特殊权限、隐藏权限其实有一个共性—权限是针对某一类用户设置的，能够对很多人同时生效。如果希望对某个指定的用户进行单独的权限控制，就需要用到文件的访问控制列表（ACL）了。通俗来讲，基于普通文件或目录设置ACL其实就是针对指定的用户或用户组设置文件或目录的操作权限，更加精准地派发权限。另外，如果针对某个目录设置了ACL，则目录中的文件会继承其ACL权限；若针对文件设置了ACL，则文件不再继承其所在目录的ACL权限。

**为了更直观地看到ACL对文件权限控制的强大效果，我们先切换到普通用户，然后尝试进入root管理员的家目录中。在没有针对普通用户为root管理员的家目录设置ACL之前，其执行结果如下所示：**

```shell
[root@chen ~]# su - linux
Last login: Mon Apr 25 20:43:27 CST 2022 on pts/0
[linux@chen ~]$ cd /root
-bash: cd: /root: Permission denied
[linux@chen ~]$ exit
logout
[root@chen ~]# 
```

### 5.5.1、setfacl命令

- **setafacl命令用于管理文件的ACL权限规则，英文名称为"set file ACL",语法格式为"setfacl [参数] 文件名称"**

**setfacl命令中的参数以及作用**

| 参数 | 作用             |
| ---- | ---------------- |
| -m   | 修改权限         |
| -M   | 从文件中读取权限 |
| -x   | 删除某个权限     |
| -b   | 删除全部权限     |
| -R   | 递归子目录       |



```shell 
# 原来我们无法进入/root目录中，下面来设置用户在/root 目录上的权限：
[root@chen ~]# setfacl -Rm u:linux:rwx /root
[root@chen ~]# su - linux
Last login: Fri Apr 29 21:15:34 CST 2022 on pts/0
[linux@chen ~]$ cd /root
[linux@chen root]$ ls
chen  linux  test1  下载  公共  图片  文档  桌面  模板  视频  音乐
[linux@chen root]$ exit
logout
[root@chen ~]# ls -ld /root
dr-xrwx---+ 18 root root 4096 Apr 29 21:15 /root
[root@chen ~]# 
```

**我们如何查看文件是否设置了ACL呢？常用的ls命令时看不到ACL信息的，但是却可以看到文件权限的最后多了一个点(.)变成了加号(+)，这就意味着文件已设置了ACL。**

### 5.5.2、getfacl命令

- getfacl 命令用于显示文件上设置的 ACL 信息，格式为“getfacl 文件名称” 

**下面使用 getfacl 命令显示在 root 管理员家目录上设置的所有 ACL 信息** 

```shell
[root@chen ~]# getfacl /root
getfacl: Removing leading '/' from absolute path names
# file: root
# owner: root
# group: root
user::r-x
user:linux:rwx
group::r-x
mask::rwx
other::---
```

**ACL权限还可以针对某个用户组进行设置**

```shell
# 允许某个组的用户都可以读写/etc/fstab文件
[root@chen ~]# setfacl -m g:linux:rw /etc/fstab
[root@chen ~]# getfacl /etc/fstab 
getfacl: Removing leading '/' from absolute path names
# file: etc/fstab
# owner: root
# group: root
user::rw-
group::r--
group:linux:rw-
mask::rw-
other::r--
```

**设置错了想删除，清空所有的ACL权限，用-b参数；要删除某一条指定的权限按，就用-x参数：**

```shell
[root@chen ~]# clear
[root@chen ~]# setfacl -x g:linux /etc/fstab 
[root@chen ~]# getfacl /etc//fstab 
getfacl: Removing leading '/' from absolute path names
# file: etc//fstab
# owner: root
# group: root
user::rw-
group::r--
mask::r--
other::r--
```

ACL权限的设置都是立即且永久生效的，这也有安全隐患，如果我们不小心设置错了权限，就会覆盖掉文件原始的权限信息，并且找不回来。

**操作前备份一下。**

```shell
# 例如在备份/home目录上的ACL权限时，可使用-R递归参数，这样不仅能够把目录本身的权限进行备份，还能将里面的文件权限也自动备份
[root@chen ~]# cd /
[root@chen /]# getfacl -R home > backup.acl
[root@chen /]# ls -l
total 108
-rw-r--r--.   1 root root 22209 Apr 29 22:53 backup.acl
```

**ACL权限的恢复也简单，使用--restore参数1**

```shell
[root@chen /]# setfacl --restore backup.acl
```



##  5.6、su 命令与 sudo 服务  

- su命令可以解决切换用户的问题

```shell
# 从root管理员切换到普通用户
[root@chen /]# cd ~
[root@chen ~]# su - linux
Last login: Fri Apr 29 21:16:26 CST 2022 on pts/0
[linux@chen ~]$ id 
uid=1002(linux) gid=1002(linux) groups=1002(linux) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023
[linux@chen ~]$

```

**细心的读者一定会发现，上面的 su 命令与用户名之间有一个减号（-），这意味着完全切换到新的用户，即把环境变量信息也变更为新用户的相应信息，而不是保留原始的信息。强烈建议在切换用户身份时添加这个减号（-）**

```shell
# 普通用户切换成 root管理员就需要进行密码验证了
[linux@chen ~]$ su - root
Password: 
Last login: Fri Apr 29 22:29:12 CST 2022 from 192.168.200.1 on pts/1
[root@chen ~]# 
```

- sudo 命令用于给普通用户提供额外的权限来完成原本 root 管理员才能完成的任务，格式 为“sudo [参数] 命令名称” 
- sudo 服务中的可用参数以及作用 

|  参数   |                             作用                             |
| :-----: | :----------------------------------------------------------: |
|   -k    | 强迫使用者在下一次执行 sudo 时问密码（不论有没有超过 N 分钟） |
|   -b    |                  将要执行的指令放在背景执行                  |
|   -p    | prompt 可以更改问密码的提示语，其中 %u 会代换为使用者的帐号名称，%h 会显示主机名称 |
|   -s    | 执行环境变数中的SHELL 所指定的shell ，或是 /etc/passwd 里所指定的 shell |
| command |     要以系统管理者身份（或以 -u 更改为其他人）执行的指令     |
|   -v    | 因为 sudo 在第一次执行时或是在 N分钟内没有执行（N 预设为五）会问密码，这个参数是重新做一次确认，如果超过N分钟，也会问密码 |
|   -h    |                         列出帮助信息                         |
|   -l    |                   列出当前用户可执行的命令                   |
|   -u    |           用户名或 UID 值 以指定的用户身份执行命令           |

**总结来说，sudo 命令具有如下功能：** 

- 限制用户执行指定的命令： 
- 记录用户执行的每一条命令； 
- 配置文件（/etc/sudoers）提供集中的用户管理、权限与主机等参数；
- 验证密码的后 5 分钟内（默认值）无须再让用户再次验证密码 

## 6、存储结构和管理硬盘

**Linux 系统中的文件存储结构开始，讲述文件系统层次化标准（FHS，Filesystem Hierarchy Standard）、udev硬件命名规则以及硬盘分区的规划方法。**

### 6.1、一切从"/"开始

在 Linux 系统中，目录、字符设备、块设备、套接字、打印机等都被抽象成了文件，另外，Linux
系统中的文件和目录名称是严格区分大小写的。例如，root、rOOt、Root、rooT 均代表不同的目
录，并且文件名称中不得包含斜杠（/）。Linux 系统中的文件存储结构如图所示。

![1651642416947](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1651642416947.png)

 **前文提到的FHS 是根据以往无数Linux 系统用户和开发者的经验而总结出来的，是用户在Linux 系统中存储文件时需要遵守的规则，用于指导我们应该把文件保存到什么位置，以及告诉用户应该在何处找到所需的文件**

- Linux系统中常见的目录名称以及相应内容
  - **/boot 		      开机所需文件—内核、开机菜单以及所需配置文件等**
  - **/dev    		      以文件形式存放任何设备与接口**
  - **/etc     	              配置文件**
  - **/home                  用户家目录**
  - **/bin                       存放单用户模式下还可以操作的命令**
  - **/lib                         开机时用到的函数库，以及/bin 与/sbin 下面的命令要调用的函数**
  - **/sbin                      开机过程中需要的命令**
  - **/media                  用于挂载设备文件的目录**
  - **/opt                       放置第三方的软件**
  - **/root                      系统管理员的家目录**
  - **/srv                        一些网络服务的数据文件目录**
  - **/tmp                      任何人均可使用的“共享”临时目录**
  - **/proc                      虚拟文件系统，例如系统内核、进程、外部设备及网络状态等**
  - **/usr/local              用户自行安装的软件**
  - **/usr/sbin Linux   系统开机时不会使用到的软件/命令/脚本**
  - **/usr/share           帮助与说明文件，也可放置共享文件**
  - **/var                       主要存放经常变化的文件，如日志**
  - **/lost+found          当文件系统发生错误时，将一些丢失的文件片段存放在这里**

```shel
[root@chen /]# ls -l
总用量 108
lrwxrwxrwx.   1 root root     7 3月  25 03:00 bin -> usr/bin
dr-xr-xr-x.   6 root root  4096 4月  19 21:03 boot
drwxr-xr-x.   3 root root  4096 4月  19 20:13 chen
drwxr-xr-x.  19 root root  3280 5月   4 13:24 dev
drwxr-xr-x. 146 root root 12288 5月   4 13:24 etc
drwxr-xr-x.   8 root root  4096 4月  25 20:15 home
lrwxrwxrwx.   1 root root     7 3月  25 03:00 lib -> usr/lib
lrwxrwxrwx.   1 root root     9 3月  25 03:00 lib64 -> usr/lib64
drwx------.   2 root root 16384 3月  25 03:00 lost+found
drwxr-xr-x.   3 root root  4096 4月  18 18:18 media
drwxr-xr-x.   2 root root  4096 4月  11 2018 mnt
drwxr-xr-x.   3 root root  4096 3月  25 03:05 opt
dr-xr-xr-x. 229 root root     0 5月   4 13:24 proc
dr-xrwx---+  18 root root  4096 4月  29 22:29 root
drwxr-xr-x.  45 root root  1300 5月   4 13:24 run
lrwxrwxrwx.   1 root root     8 3月  25 03:00 sbin -> usr/sbin
drwxr-xr-x.   2 root root  4096 4月  11 2018 srv
dr-xr-xr-x.  13 root root     0 5月   4 13:24 sys
drwxrwxrwt.  38 root root 12288 5月   4 13:40 tmp
drwxr-xr-x.  13 root root  4096 3月  25 03:00 usr
drwxr-xr-x.  22 root root  4096 4月  18 18:23 var
```

在Linux系统中还有另外一个重要的概念—路径。路径是指如何定位到某个文件，分为**相对路径与绝对路径**

绝对路径指的是从根目录（/）开始写起的文件或目录名称，而相对路径则指的是相对于当前路径的写法。

### 6.2、物理设备的命名规则

在 Linux 系统中一切都是文件，硬件设备也不例外。既然是文件，就必须有文件名称。
系统内核中的udev 设备管理器会自动把硬件名称规范起来，目的是让用户通过设备文件的名
字可以猜出设备大致的属性以及分区信息等；这对于陌生的设备来说特别方便。

- 常见的硬件设备及其文件名称
  - IDE设备                      /dev/hd[a-d]
  - SCSI/SATA/U盘          /dev/sd[a-p]
  - 软驱                            /dev/fd[0-1]
  - 打印机                        /dev/lp[0-15]
  - 光驱                            /dev/cdrom
  - 鼠标                            /dev/mouse
  - 磁带机                        /dev/st0 或/dev/ht0

由于现在的 IDE 设备已经很少见了，所以一般的**硬盘设备都会是以“/dev/sd”开头的**。
而一台主机上可以有多块硬盘，因此**系统采用a～p 来代表16 块不同的硬盘（默认从a 开始**
**分配**），而且硬盘的分区编号也很有讲究：

- 主分区或扩展分区的编号从 1 开始，到4 结束；
- 逻辑分区从编号 5 开始。

**/dev 目录中sda 设备之所以是a，并不是由插槽决定的，而是由系统内核的识别顺序来决定的，而恰巧很多主板的插槽顺序就是系统内核的识别顺序，因此才会被命名为/dev/sda**

**sda3 只能表示是编号为3 的分区，而不能判断sda 设备上已经存在了3 个分区**

![1651652076959](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1651652076959.png)

### 6.3、文件系统与数据资料

用户在硬件存储设备中执行的文件建立、写入、读取、修改、转存与控制等操作都是依
靠文件系统来完成的

Linux系统支持数十种的文件系统，而最常见的文件系统如下所示

- **Ext3**：是一款日志文件系统，能够在系统异常宕机时避免文件系统资料丢失，并
  能自动修复数据的不一致与错误。然而，当硬盘容量较大时，所需的修复时间也
  会很长，而且也不能百分之百地保证资料不会丢失。它会把整个磁盘的每个写入
  动作的细节都预先记录下来，以便在发生异常宕机后能回溯追踪到被中断的部分，
  然后尝试进行修复。
- **Ext4**：Ext3 的改进版本，作为RHEL 6 系统中的默认文件管理系统，它支持的存储容
  量高达1EB（1EB=1,073,741,824GB），且能够有无限多的子目录。另外，Ext4 文件系
  统能够批量分配block 块，从而极大地提高了读写效率。
- **XFS**：是一种高性能的日志文件系统，而且是RHEL 7 中默认的文件管理系统，它的
  优势在发生意外宕机后尤其明显，即可以快速地恢复可能被破坏的文件，而且强大的
  日志功能只用花费极低的计算和存储性能。并且它最大可支持的存储容量为18EB，
  这几乎满足了所有需求。

日常需要保存在硬盘中的数据实在太多了，因此Linux系统中有一个名为super block的“硬盘地图”。Linux并不是把文件内容直接写入到这个“硬盘地图”里面，而是在里面记录着整个文件系统的信息。因为如果把所有的文件内容都写入到这里面，它的体积将变得非常大，而且文件内容的查询与写入速度也会变得很慢。Linux只是把每个文件的权限与属性记录在inode中，而且每个文件占用一个独立的inode表格，该表格的大小默认为128字节，里面记录着如下信息：

> 该文件的访问权限（read、write、execute）；
>
> 该文件的所有者与所属组（owner、group）；
>
> 该文件的大小（size）；
>
> 该文件的创建或内容修改时间（Ctime）；
>
> 该文件的最后一次访问时间（Atime）；
>
> 该文件的修改时间（Mtime）；
>
> 文件的特殊权限（SUID、SGID、SBIT）；
>
> 该文件的真实数据地址（point）。





![VFS的架构示意图-1](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/MdVFS%E7%9A%84%E6%9E%B6%E6%9E%84%E7%A4%BA%E6%84%8F%E5%9B%BE-1.jpg)

### 6.4.挂载硬件设备

#### 6.4.1、df命令

- **df命令用于查看已挂载的磁盘空间使用情况，英文名称为"disk free"**语法格式为："**df -h**"

```shell
[root@chenstudy ~]\# df -h
Filesystem      Size  Used Avail Use% Mounted on
devtmpfs        895M     0  895M   0% /dev
tmpfs           910M     0  910M   0% /dev/shm
tmpfs           910M   11M  900M   2% /run
tmpfs           910M     0  910M   0% /sys/fs/cgroup
/dev/sda2        37G  6.9G   31G  19% /
/dev/sda1      1014M  181M  834M  18% /boot
tmpfs           182M   12K  182M   1% /run/user/42
tmpfs           182M     0  182M   0% /run/user/0
```

#### 6.4.2、umount命令

- **umount**命令用于卸载设备或系统文件，语法格式为：**umount[设备文件/挂载目录]**

```shell
[root@chenstudy ~]\# umount /dev/sdb2
```

- lsblk命令用于查看已挂载的磁盘空间使用情况

```shell
[root@chenstudy ~]\# lsblk
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda      8:0    0   40G  0 disk 
├─sda1   8:1    0    1G  0 part /boot
├─sda2   8:2    0   37G  0 part /
└─sda3   8:3    0    2G  0 part [SWAP]
sr0     11:0    1  4.4G  0 rom 
```

### 6.5、添加硬件设备

#### 6.5.1、fdisk命令

**fdisk命令用于新建、修改及删除磁盘分区表信息**，语法格式为："fdisk 磁盘名称"

 **fdisk命令中的参数以及作用：**

| 参数 |          作用          |
| :--: | :--------------------: |
|  m   |   查看全部可用的参数   |
|  n   |      添加新的分区      |
|  d   |    删除某个分区信息    |
|  l   | 列出所有可用的分区类型 |
|  t   |   改变某个分区的类型   |
|  p   |     查看分区表信息     |
|  w   |       保存并退出       |
|  q   |     不保存直接退出     |

#### 6.5.2、du命令

- **du命令**用于查看分区或目录所占用的磁盘容量大小，语法格式为："du -sh 目录名称"

```shell
[root@localhost /]# du -sh /*
0	/bin
130M	/boot
0	/dev
29M	/etc
12K	/home
0	/lib
0	/lib64
0	/media
0	/mnt
0	/opt
0	/proc
570M	/pxa270_linux
483M	/root
9.4M	/run
0	/sbin
0	/srv
0	/sys
8.0K	/tmp
3.6G	/usr
139M	/var
```











