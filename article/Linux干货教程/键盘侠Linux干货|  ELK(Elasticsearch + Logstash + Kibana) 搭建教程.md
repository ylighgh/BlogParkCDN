# 前言

Elasticsearch + Logstash + Kibana（ELK）是一套开源的日志管理方案，分析网站的访问情况时我们一般会借助 Google / 百度 / CNZZ 等方式嵌入 JS 做数据统计，但是当网站访问异常或者被攻击时我们需要在后台分析如 Nginx 的具体日志，而 Nginx 日志分割 / GoAccess/Awstats 都是相对简单的单节点解决方案，针对分布式集群或者数据量级较大时会显得心有余而力不足，而 ELK 的出现可以使我们从容面对新的挑战。

Logstash：负责日志的收集，处理和储存

Elasticsearch：负责日志检索和分析

Kibana：负责日志的可视化

>ELK(Elasticsearch + Logstash + Kibana)

![ELK_01](https://ylighgh.gitee.io/blogparkcdn/images/ELK_01.png)

# 搭建过程

## 环境准备

*下载RPM包*

**P.S：** 由于ES的官网的速度对我们国内很不友好，我已经将安装包传至国内网站上，直接下载即可

[kibana-7.8.0-x86_64.rpm](https://download.csdn.net/download/ylighgh/12540844)

[elasticsearch-7.8.0-x86_64.rpm](https://download.csdn.net/download/ylighgh/12540839)

[logstash-7.8.0.rpm](https://download.csdn.net/download/ylighgh/12540832)

>windows:先下载在主机上，通过软件上传至服务器端
>Linux:通过SCP命令上传至服务器端

![ELK_02](https://ylighgh.gitee.io/blogparkcdn/images/ELK_02.png)

## 安装Elasticsearch

```
yum -y install elasticsearch-7.8.0-x86_64.rpm

sed -i '17s/#cluster.name: my-application/cluster.name: elk/' /etc/elasticsearch/elasticsearch.yml

sed -i '23s/#node.name: node-1/node.name: node-1/' /etc/elasticsearch/elasticsearch.yml

sed -i '55s/#network.host: 192.168.0.1/network.host: 127.0.0.1/' /etc/elasticsearch/elasticsearch.yml

systemctl daemon-reload
systemctl enable elasticsearch.service
systemctl start elasticsearch

```
## 安装Kibana

```
yum -y install kibana-7.8.0-x86_64.rpm

sed -i '7s/#server.host: "localhost"/server.host: "0.0.0.0"/' /etc/kibana/kibana.yml

sed -i '28s/#elasticsearch.hosts: .*/elasticsearch.hosts: ["http:\/\/127.0.0.1:9200"]/' /etc/kibana/kibana.yml

#清空防火墙规则
iptables -F 
service  iptables save

systemctl enable kibana
systemctl start kibana

```

## 安装Logstash

```
yum -y install logstash-7.8.0.rpm 
```

# 验证

访问 http://服务器地址:5601/

![ELK_03](https://ylighgh.gitee.io/blogparkcdn/images/ELK_03.png)

至此，ELK(Elasticsearch + Logstash + Kibana) 搭建教程结束

- 如果时阿里云ECS需要添加安全组规则放行5601端口

# 写在最后

如果文档对你有帮助的话，留个赞再走吧 ，你的点击是我的最大动力。

我是键盘侠，现实中我唯唯诺诺，网络上我重拳出击，关注我，持续更新Linux干货教程。

更多键盘侠Linux系列教程：[链接地址](https://www.cnblogs.com/MrKeyboard/category/1786086.html)

更多Linux干货教程请扫:（回复 `干货`）

![wechatmansearch](https://ylighgh.gitee.io/blogparkcdn/images/wechatmansearch.jpg)