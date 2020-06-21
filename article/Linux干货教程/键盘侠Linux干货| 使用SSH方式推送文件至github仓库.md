# 前言

作为一名优秀的计算机从业人员，相信大家github应该都有知道吧。（优秀的代码托管工具）

但是由于每次git push 时都需要输入帐号密码才能将我们修改的文件推送至远程仓库非常的不方便，由于mk是比较懒的人，不想每次都敲键盘输密码。

![git_01](https://ylighgh.gitee.io/blogparkcdn/images/git_01.jpg)

所以，我想配置一下github的无密码推送文件（也可以称为SSH证书推送）。

# 配置SSH

## 生成SSH密钥对

```
ssh-keygen -t rsa -C "youremail@example.com"
```
如下图所示，会生成两个文件

私钥文件：`/ylighgh/.ssh/id_rsa`

公钥文件： `/ylighgh/.ssh/id_rsa.pub`

![git_02](https://ylighgh.gitee.io/blogparkcdn/images/git_02.jpg)

## 创建配置文件

```
touch ~/.ssh/config
```

## 配置SSH客户端私钥

```
cat << EOF >> ~/.ssh/config
IdentityFile ~/.ssh/id_rsa
EOF
```
## 查看公钥

这里的公钥我们等会用到的，先粘贴在我们的剪切板上

```
cat ~/.ssh/id_rsa.pub
```
![git_03](https://ylighgh.gitee.io/blogparkcdn/images/git_03.jpg)

## 在github上配置SSH

![git_04](https://ylighgh.gitee.io/blogparkcdn/images/git_04.jpg)

![git_05](https://ylighgh.gitee.io/blogparkcdn/images/git_05.jpg)

![git_06](https://ylighgh.gitee.io/blogparkcdn/images/git_06.jpg)

添加完成之后点击 `Add SSH key` ，Github和主机之间的SSH配对完成

添加完成可在终端执行 ` ssh git@github.com` 验证是不是添加成功

![git_07](https://ylighgh.gitee.io/blogparkcdn/images/git_07.jpg)


#  github远程仓库文件拉取至本地

## 克隆仓库到本地

使用 `git clone`克隆仓库到本地

**P.S：这里使用SSH方法**

![git_08](https://ylighgh.gitee.io/blogparkcdn/images/git_08.jpg)

![git_09](https://ylighgh.gitee.io/blogparkcdn/images/git_09.jpg)

# 推送文件至github远程仓库

## 创建文件

初始化git仓库 `git init` (进入到项目目录中执行)

创建一个a.test的文件 `touch a.test`

提交到暂缓区 `git add . && git commit -m 'test'`

![git_10](https://ylighgh.gitee.io/blogparkcdn/images/git_10.jpg)

## 推送文件到远程仓库

使用`git push`命令

![git_11](https://ylighgh.gitee.io/blogparkcdn/images/git_11.jpg)

由于github服务器在国外，我这里使用了一个代理方式推送文件，但我们可以看到，使用SSH方式推送文件是不需要我们输入用户名密码，这就达到了我的目的，偷懒~~。

# 写在最后
如果文档对你有帮助的话，留个赞再走吧 ，你的点击是我的最大动力。

我是键盘侠，现实中我唯唯诺诺，网络上我重拳出击，关注我，持续更新Linux干货教程。

更多键盘侠Linux系列教程：[链接地址](https://www.cnblogs.com/MrKeyboard/category/1786086.html)

更多Linux干货教程请扫:

![wechatmansearch](https://ylighgh.gitee.io/blogparkcdn/images/wechatmansearch.jpg)