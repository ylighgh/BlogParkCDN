# CentOS 7- 配置阿里镜像源
```
#下载CentOS 7的repo文件
curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo

#更新镜像源
yum clean all
yum makecache

```

