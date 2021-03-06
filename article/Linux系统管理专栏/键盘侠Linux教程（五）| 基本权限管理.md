# 基本权限管理

## 权限的介绍

### 权限位的含义

前面讲解ls命令时，我们已经知道长格式显示的第一列就是文件的权限，例如：

```
[root@es ~]# ls -l anaconda-ks.cfg
-rw-------. 1 root root 1573 May 18 23:28 anaconda-ks.cfg
```
> 第一位为文件类型

|文件类型标识|文件类型|
|:--:                     |            :--:|
|-	                       |普通文件|
|d	                      |目录文件|
|l  	                   |软链接文件|
|s（伪文件）   |	套接字文件|
|b（伪文件）   |	块设备文件|
|c（伪文件）   |	字符设备文件|
|p（伪文件）   |	管道符文件|

第一列的权限位如果不计算最后的 "." （这个点的含义我们在后面解释），则共有10位，这10位权限位的含义如图所示。

![权限_01](https://ylighgh.gitee.io/blogparkcdn/images/权限_01.jpeg)

## 基本权限介绍

修改权限的命令 `chmod` ，基本信息如下

### 命令格式

```
[root@es ~]#chmod [选项] 权限模式 文件名

选项：

-R ： 递归设置权限，也就是给子目录中的所有文件设定权限
```

### 权限模式

chmod 命令的权限模式的格式时"[ugoa][[+-=][perms]]"，也就是“[用户身份][[赋予方式][权限]]”的格式

- 用户身份：

    - u：代表所有者(user)

    - g：代表所属组(group)

    - o：代表其他人(other)

    - a：代表全部身份(all)

*  赋予方式：

    - +：加入权限

    - -：减去权限

    - =：设置权限

* 权限：

    - -r：读取权限(read)

   - -w：写权限(write)

    - -x：执行权限(execute)

### 数字权限

数字权限的赋予方式是最简单的，但是不如之前的字母权限好记，直观。我们来看看这些数字权限的含义。

* 4：代表"r"权限

* 2：代表"w"权限

* 1：代表"x"权限

### 常用权限

数字权限的赋予方式更加简单，但是需要用户对这几个数字更加熟悉。其实常用权限也并不多，只有如下几个。

* 644：这是文件的基本权限，代表所有者拥有读，写权限，而所属组和其他人拥有只读权限。

* 755：这是文件的执行权限和目录的基本权限，代表所有者拥有读，写和执行权限，而所属组和其他人拥有读和执行权限。

* 777：这时最大权限。在实际的生产服务器中，要尽力避免给文件或目录赋予这样的权限，这会造成一定的安全隐患。

## 基本权限的作用

### 权限含义的解释

首先，读，写，执行权限对文件和目录的作用是不同的。

- 权限对文件的作用

     - 读(r)：对文件有读(r)权限，代表可以读取文件中的数据。如果把权限对应到命令上，那么一旦对文件有读(r)权限，就可以对文件执行cat,more,less,head,tail等文件查看命令。

    - 写(w)：对文件有写(w)权限，代表可以修改文件中的数据。如果把权限对应到命令上，那么一旦对文件有写(w)权限，就可以对文件执行vim,echo等修改文件数据的命令。**注意：对文件有写权限，是不能删除文件本身的，只能修改文件中的数据。如果要想删除文件，则需要对文件的上级目录拥有写权限。**

    - 执行(x)：对文件有执行(x)权限，代表文件拥有了执行权限，可以运行。在Linux中，只要文件有执行(x)权限，这个文件就是执行文件了。只是这个文件到底能不能正确执行，不仅需要执行(x)权限，还要看文件中代码是不是正确的语言代码。文件夹来说，执行(x)权限是最高权限。

- 权限对目录的作用

    - 读(r)：对目录有读(r)权限，代表可以查看目录下的内容，也就是可以查看目录下有哪些子文件和子目录。如果把权限对应到命令上，那么一旦对目录拥有了读(r)权限，就可以在目录下执行**ls命令**，查看目录下的内容

    - 写(w)：对目录有写(r)权限，代表可以修改目录下的数据，也就是可以在目录中新建，删除，复制，剪切子文件或子目录。如果把权限对应到命令上，那么一旦目录拥有了写(w)权限，就可以在目录下执行**touch,rm,cp,mv命令**。对目录来说写(w)权限是最高权限。

    - 执行(x)：目录是不能运行的，那么对目录拥有执行(x)权限，代表可以进入到目录。如果把权限对应到命令上，那么一旦对目录拥有了执行(x)权限，就可以对目录执行**cd命令**，进入目录。

### 目录的可用权限

目录的可用权限其实有以下几个

* 0：任何权限都不赋予

* 5：基本的目录浏览和进入权限

* 7：完全权限 

## 所有者和所属组命令

### chown命令

修改权限的命令 `chown` ，基本信息如下
```
[root@es ~]#chown [选项] 所有者:所属组 文件或目录

选项：

-R ： 递归设置权限，也就是给子目录中的所有文件设定权限
```
>普通用户不能修改文件的所有者，哪怕自己是这个文件的所有者也不行，只有超级用户才能修改
>普通用户可以修改**所有者是自己**的文件的**权限**=

## umask 默认权限

### 查看系统的umask权限

```
#用八进制数值显示umask权限
[root@centos7 ~]# umask
0022

#用字母表示文件和目录的初始权限
[root@centos7 ~]# umask -S
u=rwx,g=rx,o=rx
```

### umask权限的计算方法

我们需要先了解一下新建文件和目录的默认最大权限

* 对文件来讲，新建文件的默认最大权限是666，没有执行(x)权限。这时因为执行权限对文件来讲比较危险，不能在新建文件的时候默认赋予，而必须通过用户手工赋予。

* 文件的默认权限最大只能是666，而umask的值是022
"-rw-rw-rw-"减去"-----w--w-" 等于"-rw-r--r--"

* 对目录来讲，新建文件的默认最大权限是777。这时因为对目录而言，执行(x)权限仅仅代表进入目录，所以即使建立新文件时直接默认赋予，也没有什么危险。

* 目录的权人权限最大只能是777，而umask的值是022

"drwxrwxrwx" 减去 "d----w--w-" 等于 "drwx-r-xr-x"

# 写在最后

如果文档对你有帮助的话，留个赞再走吧 ，你的点击是我的最大动力。

我是键盘侠，现实中我唯唯诺诺，网络上我重拳出击，关注我，持续更新Linux干货教程。

更多键盘侠Linux系列教程：[链接地址](https://www.cnblogs.com/MrKeyboard/category/1786086.html)

更多Linux干货教程请扫:

![wechatmansearch](https://ylighgh.gitee.io/blogparkcdn/images/wechatmansearch.jpg)

创作不易，打赏请扫:

微信：

![wechatpay](https://ylighgh.gitee.io/blogparkcdn/images/wechatpay.png)

支付宝：

![alipay](https://ylighgh.gitee.io/blogparkcdn/images/alipay.png)