# VirtualBox 虚拟机安装与使用

## VirtualBox 简介

VirtualBox是一个虚拟PC的软件，可以在现有的操作系统上虚拟出一个新的硬件环境，相当于模拟出新的一台PC，以此来实现在一台机器上真正同时运行两个独立的操作系统。

## VirtualBox 下载地址
VirtualBox：[下载地址](https://download.virtualbox.org/virtualbox/6.1.10/VirtualBox-6.1.10-138449-Win.exe)

## CentOS7镜像下载地址

CentOS7：[下载地址](https://mirrors.tuna.tsinghua.edu.cn/centos/7/isos/x86_64/CentOS-7-x86_64-Everything-2003.iso)

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

8.设置完毕就可以看见我们的虚拟机已经创建成功了，接下来进行虚拟机的配置

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


# Linux系统安装




# 远程登录管理工具