# 实验软件包下载地址

## VirtualBox下载地址
VirtualBox：[下载地址](https://download.virtualbox.org/virtualbox/6.1.10/VirtualBox-6.1.10-138449-Win.exe)

## CentOS7镜像下载地址

CentOS7：[下载地址](https://mirrors.tuna.tsinghua.edu.cn/centos/7/isos/x86_64/CentOS-7-x86_64-Everything-2003.iso)

## 远程登录管理工具下载地址

MobaXterm：[下载地址](https://mobaxterm.mobatek.net/download.html)

# VirtualBox 虚拟机安装与使用

## VirtualBox 简介

VirtualBox是一个虚拟PC的软件，可以在现有的操作系统上虚拟出一个新的硬件环境，相当于模拟出新的一台PC，以此来实现在一台机器上真正同时运行两个独立的操作系统。

## VirtualBox 配置

1.新建虚拟机

![virtualbox_01](https://cdn.jsdelivr.net/gh/ylighgh/BlogParkCDN@master/images/virtualbox_01.png)

2.设置虚拟机名称和保存位置

![virtualbox_02](https://cdn.jsdelivr.net/gh/ylighgh/BlogParkCDN@master/images/virtualbox_02.png)

3.设置虚拟机内存大小

![virtualbox_03](https://cdn.jsdelivr.net/gh/ylighgh/BlogParkCDN@master/images/virtualbox_03.png)

4.创建虚拟磁盘

![virtualbox_04](https://cdn.jsdelivr.net/gh/ylighgh/BlogParkCDN@master/images/virtualbox_04.png)

5.选择虚拟硬盘文件类型

![virtualbox_05](https://cdn.jsdelivr.net/gh/ylighgh/BlogParkCDN@master/images/virtualbox_05.png)

![virtualbox_06](https://cdn.jsdelivr.net/gh/ylighgh/BlogParkCDN@master/images/virtualbox_06.png)

7.设置虚拟机的虚拟硬盘文件大小和保存位置

![virtualbox_07](https://cdn.jsdelivr.net/gh/ylighgh/BlogParkCDN@master/images/virtualbox_07.png)

8.配置虚拟机

![virtualbox_08](https://cdn.jsdelivr.net/gh/ylighgh/BlogParkCDN@master/images/virtualbox_08.png)

9.选择下载的ISO文件

![virtualbox_09](https://cdn.jsdelivr.net/gh/ylighgh/BlogParkCDN@master/images/virtualbox_09.png)

10.选择网络连接方式，这里我们选择桥接网卡
>桥接网卡是将虚拟机的网卡直接连接到我们的物理网卡上

![virtualbox_10](https://cdn.jsdelivr.net/gh/ylighgh/BlogParkCDN@master/images/virtualbox_10.png)

11.选择系统启动的顺序
>将硬盘放在第一启动位，之后使用光驱安装系统，是安装在硬盘上

![virtualbox_11](https://cdn.jsdelivr.net/gh/ylighgh/BlogParkCDN@master/images/virtualbox_11.png)


## VirtualBox 网络连接方法

| 连接方式 | 连接网卡 |是否能连接本机 |是否能连接局域网 |是否能连接公网 |
| :----:|:----: | :----: | :----: |  :----: | 
| 桥接 | 本地真实网卡 |可以 |可以 |可以 |
| NAT | VMnet8 |可以 |不能 |可以 |
| 仅主机 | VMnet1|可以 |不能 |不能 |

# 系统分区

## 磁盘分区

磁盘分区是使用分区编辑器（partition editor）在磁盘上划分几个逻辑部分。碟片一旦划分成数个分区（Partition），不同类的目录与文件可以存储进不同的分区。
 
### 分区类型（MBR）

- 主分区：最多只能有4个
- 扩展分区
    - 最多只能有1个
    - 主分区加扩展分区最多有4个
    - 不能写入数据，只能包含逻辑分区
- 逻辑分区

### 两种分区表形式

* MBR分区表：最大支持2.1TB硬盘，最多支持4个分区
* GPT分区表（全局唯一标示分区表）：GPT支持9.4ZB硬盘（1ZB=1024PB，1PB=1024EB，1EB=1024TB）。理论上支持的分区数没有限制，但windows限制128个主分区。
 
## 格式化

格式化（高级格式化）又称逻辑格式化，它是指根据用户选定的文件系统（如FAT16、FAT32、NTFS、EXT2、EXT3、EXT4等），在磁盘的特定区域写入特定数据，在分区中划出一片用于存放文件分配表、目录表等用于文件管理的磁盘空间

## 硬件设备文件名

![Linuxharddev_01](https://cdn.jsdelivr.net/gh/ylighgh/BlogParkCDN@master/images/Linuxharddev_01.png)

- 设备文件名
    - /dev/hda1（IDE接口）
    - /dev/sda1 （SCSI硬盘接口、SATA硬盘接口）

> /dev/sdb5 第二块SATA接口硬盘的第一个逻辑分区

## 挂载
挂载点：（使用已经存在的空目录作为挂载点）

- 必须分区
    - /（根分区）
    - swap分区（交换内存）
        - 如果真实内存小于4GB，swap位内存的两倍
        - 如果真实内存大于4GB，swap和内存一致
        - 实验环境，不大于2GB
- 推荐分区
    - /boot （启动分区，1GB）
- 常用分区
    - /home（用户文件服务器）
    - /www （用于Web服务器）


# Linux系统安装

Linux最小化安装，安装时需要按照一下步骤进行：

1.安装时请选择英文界面，然后单击右下角  `Continue` 按钮。

![Linux_install_01](https://cdn.jsdelivr.net/gh/ylighgh/BlogParkCDN@master/images/Linux_install_01.png)

2.单击 `DATA & TIME` 设置系统时区为 `Asia Shanghai`，设置完成单击左上角` Done `按钮。

![Linux_install_02](https://cdn.jsdelivr.net/gh/ylighgh/BlogParkCDN@master/images/Linux_install_02.png)

![Linux_install_03](https://cdn.jsdelivr.net/gh/ylighgh/BlogParkCDN@master/images/Linux_install_03.png)

3.单击 `INSTALLATION DESTINATION` 按钮进行分区。

![Linux_install_04](https://cdn.jsdelivr.net/gh/ylighgh/BlogParkCDN@master/images/Linux_install_04.png)

4.选择磁盘并选中` I will configure partioning `单选按钮，单击左上角 `Done` 按钮，进行手动分区。

![Linux_install_05](https://cdn.jsdelivr.net/gh/ylighgh/BlogParkCDN@master/images/Linux_install_05.png)

5.单击 `Click here to create them automatically `按钮自动创建分区，分区完成单击左上角 `Done` 按钮。

![Linux_install_06](https://cdn.jsdelivr.net/gh/ylighgh/BlogParkCDN@master/images/Linux_install_06.png)


![Linux_install_07](https://cdn.jsdelivr.net/gh/ylighgh/BlogParkCDN@master/images/Linux_install_07.png)


6.单击 `Accept Changes` 按钮保存修改。

![Linux_install_08](https://cdn.jsdelivr.net/gh/ylighgh/BlogParkCDN@master/images/Linux_install_08.png)

7.单击 `Begin Installation`按钮开始安装。

![Linux_install_09](https://cdn.jsdelivr.net/gh/ylighgh/BlogParkCDN@master/images/Linux_install_09.png)

8.单击 `ROOT PASSWORD` 按钮设置root密码，设置密码为000000。单击两次 `Done` 按钮保存退出。

![Linux_install_10](https://cdn.jsdelivr.net/gh/ylighgh/BlogParkCDN@master/images/Linux_install_10.png)

![Linux_install_11](https://cdn.jsdelivr.net/gh/ylighgh/BlogParkCDN@master/images/Linux_install_11.png)

9.安装完成后单击右下角 `Reboot `按钮重启系统。

![Linux_install_12](https://cdn.jsdelivr.net/gh/ylighgh/BlogParkCDN@master/images/Linux_install_12.png)

10.输入用户名密码登录系统，操作系统安装完成。（输入密码的时候屏幕上不会显示，输入完成回车即可）

![Linux_install_13](https://cdn.jsdelivr.net/gh/ylighgh/BlogParkCDN@master/images/Linux_install_13.png)

# 远程登录管理工具

## 为什么要使用远程登录管理工具

事实上在工作过程中间我们是无法真实的看到服务器，如果是购买云主机就更加看不到服务器，有些云服务器是在香港、国外。比如:阿里云、腾讯云、亚马逊云。在这种情况下，只能通过远程连接的方式管理Linux 系统。因此，在安装好Linux 系统后，学习Linux 运维的第一步应该是配置好客户端软件远程连接Linux 系统进行管理。

## Linux虚拟机IP地址的配置

在我们使用远程连接工具连接Linux之前，首先确保两件事情：

* LinuxIP地址配置正确
* 主机能够和Linux通信

刚才我们在前面已经设置了虚拟机的网络模式为桥接模式，接下来我们进入到虚拟机进行网卡配置

1.使用 `ip add` 命令查看当前有几张网卡

![Linux_ip_01](https://cdn.jsdelivr.net/gh/ylighgh/BlogParkCDN@master/images/Linux_ip_01.png)

lo：本地回环网卡

enp0s3:有线网卡

2.编辑网卡配置信息

`vi /etc/sysconfig/network-scripts/ifcfg-enp0s3` 输入完成回车

![Linux_ip_02](https://cdn.jsdelivr.net/gh/ylighgh/BlogParkCDN@master/images/Linux_ip_02.png)

将BOOTPROTO更改为dhcp模式，onboot更改为yes，表示开启网卡，然后按下 `ESC` 键，输入`:wq` 表示保存并退出

3.重启网卡，使配置生效

`systemctl restart network` 

`ip add ` 查看网卡状态 192.168.1.102就是我们虚拟机的IP地址

![Linux_ip_03](https://cdn.jsdelivr.net/gh/ylighgh/BlogParkCDN@master/images/Linux_ip_03.png)

4.虚拟机IP地址配置完成，接下来测试是否能够和主机通信

在windows的 `cmd`里面输入 `ping 192.168.1.102` 如果ping的通，则表示能够和虚拟机通信

![Linux_ip_04](https://cdn.jsdelivr.net/gh/ylighgh/BlogParkCDN@master/images/Linux_ip_04.png)

5.使用远程工具连接虚拟机

![Linux_ip_05](https://cdn.jsdelivr.net/gh/ylighgh/BlogParkCDN@master/images/Linux_ip_05.png)

输入设置的密码（屏幕上不会有显示），输入完毕回车

![Linux_ip_06](https://cdn.jsdelivr.net/gh/ylighgh/BlogParkCDN@master/images/Linux_ip_06.png)

成功连接虚拟机

![Linux_ip_07](https://cdn.jsdelivr.net/gh/ylighgh/BlogParkCDN@master/images/Linux_ip_07.png)

# 写在最后
如果文档对你有帮助的话，请点击一下 `推荐`按钮 ，你的点击是我的最大动力。

我是键盘侠，现实中我唯唯诺诺，网络上我重拳出击，关注我，持续更新Linux干货教程。