# Docker——基于Docker安装Drupal博客系统

1. 向脚本文件追加内容
```
cat << EOF >> build.sh
#设置主机名
hostnamectl set-hostname docker &&
#CentOS7- 配置阿里镜像源
curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo &&
yum clean all &&
yum makecache &&
#Uninstall old versions
yum -y remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine &&
#SET UP THE REPOSITORY
yum -y install yum-utils &&
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo &&
#INSTALL DOCKER ENGINE
yum -y install docker-ce docker-ce-cli containerd.io
systemctl start docker  && 
systemctl enable docker  &&
#配置镜像加速
mkdir -p /etc/docker &&
echo "{" > /etc/docker/daemon.json &&
echo   '  "registry-mirrors": ["https://ueeg7jo6.mirror.aliyuncs.com"]  '   >> /etc/docker/daemon.json &&
echo "}" >> /etc/docker/daemon.json  &&
#重载Docker，使配置生效
systemctl daemon-reload &&
systemctl restart docker &&
#创建drupal网段
docker network create drupal
#启动容器（drupal mysql）
docker run --name drupal_host -p 80:80 -d  --network drupal drupal &&
docker run -d --name drupal_mysql -p 3306:3306 --network drupal \
    -e MYSQL_DATABASE=drupal \
    -e MYSQL_USER=drupal \
    -e MYSQL_PASSWORD=drupal_password \
    -e MYSQL_ROOT_PASSWORD=000000 \
mysql:5.7 
EOF
```
---

2. 给脚本授予执行权限
```
chmod +x build.sh
```
3. 执行脚本
```
sh build.sh 
```
-----
访问http://服务器IP地址:80进入drupal安装界面

**drupal数据库相关信息**

* 数据库服务器：drupal_mysql

* 数据库端口：3306

* 数据库名称：drupal

* 数据库用户名：drupal

* 数据库密码：drupal_password

* 在安装博客系统选择数据库系统的时候Database host为: `drupal_mysql` 
----
**写在最后**

* 我是键盘侠，现实中我唯唯诺诺，网络上我重拳出击，关注我，持续更新Linux干货教程
