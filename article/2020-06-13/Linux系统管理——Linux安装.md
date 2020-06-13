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
p/13111880.html
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

10.输入用户名密码登录系统，操作系统安装完成。

![Linux_install_13](https://cdn.jsdelivr.net/gh/ylighgh/BlogParkCDN@master/images/Linux_install_13.png)

# 远程登录管理工具