# 基本权限管理

## 权限的介绍

权限位的含义

前面讲解ls命令时，我们已经知道长格式显示的第一列就是文件的权限，例如：

```
[root@es ~]# ls -l anaconda-ks.cfg
-rw-------. 1 root root 1573 May 18 23:28 anaconda-ks.cfg
```
第一列的权限位如果不计算最后的 "." （这个点的含义我们在后面解释），则共有10位，这10位权限位的含义如图所示。

![权限_01](https://ylighgh.gitee.io/blogparkcdn/images/权限_01.jpeg)
