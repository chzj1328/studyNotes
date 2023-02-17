# CentOS的下载与安装
## CentOS的下载
1. 阿里云镜像地址：[centos-7.9.2009-isos-x86_64安装包下载_开源镜像站-阿里云](https://mirrors.aliyun.com/centos/7.9.2009/isos/x86_64/)
![在这里插入图片描述](https://img-blog.csdnimg.cn/97ec04ac17484eb993e313264ba937f5.png)


## 虚拟机的创建
### 【打开VM：点击创建新的虚拟机】
![在这里插入图片描述](https://img-blog.csdnimg.cn/4a7d441847c54505bde647f9ccb096d8.png)
### 【选择自定义(高级)，点击下一步】
![在这里插入图片描述](https://img-blog.csdnimg.cn/09bf16ad1d41483eacd118c28bc914b5.png)
### 【点击：下一步】
![在这里插入图片描述](https://img-blog.csdnimg.cn/a57087196aab477e99df67717794d7a7.png)
### 【选择稍后安装操作系统，点击下一步】
![在这里插入图片描述](https://img-blog.csdnimg.cn/62889d4629124689a38189c0f700ae97.png)
### 【选择：Linux和CentOS 7 64位】
![在这里插入图片描述](https://img-blog.csdnimg.cn/33b35b1dc03d498c8a4a36e483e74ebb.png)
### 【输入虚拟机名称并选择安装位置，点击下一步】
![在这里插入图片描述](https://img-blog.csdnimg.cn/ce8b6a48fc4e44aa91d9d9ce3116802e.png)
### 【修改处理器，点击下一步】
![在这里插入图片描述](https://img-blog.csdnimg.cn/c0432fc0dc214766b32f01ba61b5f174.png)
### 【修改虚拟机内存，点击下一步】
● 建议选择2GB
![在这里插入图片描述](https://img-blog.csdnimg.cn/78df816fdb08494da00ffcf46b31fc28.png)
### 【选择NAT模式，点击下一步】
● 安装好后再修改为桥连接模式
![在这里插入图片描述](https://img-blog.csdnimg.cn/46b8f999eecf40798576416cf29e5a85.png)
### 【点击下一步】
![在这里插入图片描述](https://img-blog.csdnimg.cn/ded7fa00deda419fbd589eeea4c72f3b.png)
### 【点击下一步】
![在这里插入图片描述](https://img-blog.csdnimg.cn/410a44cad4a047649b9555c40dde9e7e.png)
### 【选择创建新的虚拟机磁盘，点击下一步】
![在这里插入图片描述](https://img-blog.csdnimg.cn/3a7d826344e64e3da3c462a48bab3c23.png)
### 【指定磁盘容量，点击下一步】
● 推荐40GB
![在这里插入图片描述](https://img-blog.csdnimg.cn/f383e0cd6b7e49b2b7548c8313e43455.png)
### 【点击下一步】
![在这里插入图片描述](https://img-blog.csdnimg.cn/442be34774224c7893db0aafa4d2534a.png)


### 【点击自定义硬件】
● 删除打印机和声卡
![在这里插入图片描述](https://img-blog.csdnimg.cn/3a352d2c18b34839954deb928a304c6d.png)

● 选择镜像文件【刚下载的ios文件】
![在这里插入图片描述](https://img-blog.csdnimg.cn/30ed4bb181cf40afa33bf3016c15e0dd.png)
### 【点击完成】
![在这里插入图片描述](https://img-blog.csdnimg.cn/0948569eaaf148e7bbdcb3fe0f720372.png)
## 安装CentOS系统
### 【点击：开启此虚拟机】
![在这里插入图片描述](https://img-blog.csdnimg.cn/49e440c57a734bbda1827c56e7279557.png)
### 【选择：Install Centos 7】
![在这里插入图片描述](https://img-blog.csdnimg.cn/3ab04f471356414390a849e21b4dd0c2.png)
### 【安装中】
![在这里插入图片描述](https://img-blog.csdnimg.cn/b21222a293cd489c972c63cdea7a488a.png)


### 【语言选择：English，点击：Continue】
![在这里插入图片描述](https://img-blog.csdnimg.cn/041a3f3b8cd8480b933b2283b4bc37ee.png)


### 【INSTALLATION SUMMARY 安装总览（这里可以完成centos 7 版本Linux的全部设置）】
（1）首先，设置时区--**DATE & TIME**
找到**Asia--Shanghai**并点击--**Done**
![在这里插入图片描述](https://img-blog.csdnimg.cn/ab9d406ef4a7478088f664b534f1700c.png)
（2）KEYBOARD 键盘就默认是**English（US）**
![在这里插入图片描述](https://img-blog.csdnimg.cn/d830ef6b68b444da8b952d0a1746f3d5.png)


（3）LANGUAGE SUPPORT语言支持
![在这里插入图片描述](https://img-blog.csdnimg.cn/1328f3226f0c4f8e938c422b5bc0e04e.png)


**可以是默认的English 也可以自行添加Chinese简体中文的支持**
![在这里插入图片描述](https://img-blog.csdnimg.cn/a6ec6dfa893946ffb26dcb98164e75bc.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/5e1481f1a6c6418880a2d6157d6d8d20.png)

（4）INSTALLATION SOURCE 安装资源
默认选择--**Local media** 本地媒体文件
![在这里插入图片描述](https://img-blog.csdnimg.cn/ed6b3a4a794245118cff7f42421e0074.png)
（5）SOFTWARE SELECTION软件安装选择
图形界面安装--**Server with GUI** 或者 **GNOME Desktop**
![在这里插入图片描述](https://img-blog.csdnimg.cn/b4d279c1818741c9a9426882aae57223.png)
### 【INSTALLATION DESTINATION 安装位置---即进行系统分区】
（1）首先选中我们在创建虚拟机时候的200G虚拟硬盘
（2）下滑菜单找到Other Storage Options--Partitioning--I will configure partitioning选中 I will configure partitioning  自定义分区 --点击done
![在这里插入图片描述](https://img-blog.csdnimg.cn/bb5cbd2ceb3b4df682dd0f7f78e1aa14.png)
（3）分区
![在这里插入图片描述](https://img-blog.csdnimg.cn/9b5b106e00624ab7872d5ee4757ba2a9.png)


（4）分区完成！
点击Done，点击Accept Changes
![在这里插入图片描述](https://img-blog.csdnimg.cn/db82d3124aa94a82962dbc69f1af3b67.png)


**KDUMP选择默认**
![在这里插入图片描述](https://img-blog.csdnimg.cn/54c6d16520a94cd7bac114c7bd7d832b.png)

**NETWORK & HOST NAME** 设置网络连接和主机名
 在Host name处设置主机名：（例如：chenstudy）
![在这里插入图片描述](https://img-blog.csdnimg.cn/80accd3d1b2c43aabc30f25dae9dd849.png)

完成所有设置，点击Begin Installation
![在这里插入图片描述](https://img-blog.csdnimg.cn/8fed70b11fda4fe3a2c0e5f8104ba501.png)
设置管理员Root Password（务必记住密码！）
![在这里插入图片描述](https://img-blog.csdnimg.cn/d1e02169c86242c092e17636128cfb48.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/57751185ecff461cb4e17daba22f2029.png)



密码设置完成后，点击**Done**
**创建用户**
![在这里插入图片描述](https://img-blog.csdnimg.cn/da8100fb98ea4441954d7cc6dbffe3b9.png)
**点击reboot重启使用**

