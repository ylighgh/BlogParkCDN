# CentOS7初始化
```
#禁止关闭显示器 archlinux wiki 提及的方法
echo -ne "\033[9;0]" >> /etc/issue
# 重启，cat /sys/module/kernel/parameters/consoleblank 为空表示生效

yum install -y iproute rsync epel-release vim-enhanced wget curl screen lbzip2 tcpdump unzip

# PHP 7
yum install centos-release-scl

#查看主机名
hostnamectl status

#修改主机名
hostnamectl set-hostname 主机名

#删除ipv6的localhost配置
#::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
echo "127.0.0.1 localhost localhost.localdomain localhost4 localhost4.localdomain4 $(hostname)" > /etc/hosts

rm -f ~/anaconda-ks.cfg  ~/install.log  ~/install.log.syslog

#禁用SELINUX，必须重启才能生效
echo SELINUX=disabled>/etc/selinux/config
echo SELINUXTYPE=targeted>>/etc/selinux/config

#如果你想使用自己的 iptables 静态防火墙规则, 那么请安装 iptables-services 并且禁用 firewalld ，启用 iptables
yum install -y iptables-services
systemctl  stop  firewalld
systemctl mask firewalld.service
systemctl enable iptables.service
iptables -F
iptables-save >/etc/sysconfig/iptables


systemctl mask NetworkManager


#最大可以打开的文件
echo "*               soft   nofile            65535" >> /etc/security/limits.conf
echo "*               hard   nofile            65535" >> /etc/security/limits.conf

# ssh登录时，登录ip被会反向解析为域名，导致ssh登录缓慢
sed -i "s/#UseDNS yes/UseDNS no/" /etc/ssh/sshd_config
sed -i "s/GSSAPIAuthentication yes/GSSAPIAuthentication no/" /etc/ssh/sshd_config
sed -i "s/GSSAPICleanupCredentials yes/GSSAPICleanupCredentials no/" /etc/ssh/sshd_config
sed -i "s/#MaxAuthTries 6/MaxAuthTries 10/" /etc/ssh/sshd_config
# server每隔30秒发送一次请求给client，然后client响应，从而保持连接
sed -i "s/#ClientAliveInterval 0/ClientAliveInterval 30/" /etc/ssh/sshd_config
# server发出请求后，客户端没有响应得次数达到3，就自动断开连接，正常情况下，client不会不响应
sed -i "s/#ClientAliveCountMax 3/ClientAliveCountMax 10/" /etc/ssh/sshd_config

#支持gbk文件显示
echo "set fencs=utf-8,gbk" >> /etc/vimrc

#设定系统时区
yes|cp /usr/share/zoneinfo/Asia/Chongqing /etc/localtime

#时间同步
yum install -y chrony
systemctl enable chronyd
systemctl start chronyd
# 或者
systemctl enable systemd-timesyncd
systemctl start systemd-timesyncd

#如果是x86_64系统，排除32位包
echo "exclude=*.i386 *.i586 *.i686" >> /etc/yum.conf

#disable IPv6
echo "net.ipv6.conf.all.disable_ipv6 = 1" >>  /etc/sysctl.conf
echo "net.ipv6.conf.default.disable_ipv6 = 1" >>  /etc/sysctl.conf

firewall-cmd --zone=public --add-port=28529/tcp --permanent
firewall-cmd --reload
```