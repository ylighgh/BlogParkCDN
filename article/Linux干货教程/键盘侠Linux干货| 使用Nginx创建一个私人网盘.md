# 前言

使用Nginx搭建一个私人网盘

# 安装Nginx
## *增加 Nginx 官方源*
```
cat << EOF > /etc/yum.repos.d/nginx.repo
[nginx-stable]
name=nginx stable repo
baseurl=http://nginx.org/packages/centos/\$releasever/\$basearch/
gpgcheck=1
enabled=1
gpgkey=https://nginx.org/keys/nginx_signing.key
module_hotfixes=true

[nginx-mainline]
name=nginx mainline repo
baseurl=http://nginx.org/packages/mainline/centos/\$releasever/\$basearch/
gpgcheck=1
enabled=0
gpgkey=https://nginx.org/keys/nginx_signing.key
module_hotfixes=true
EOF
```
> EPEL 源中的 nginx.service 由于 KILL 参数问题，启动后无法停止，不建议使用。

## *安装Nginx*
```
yum install -y nginx
```

## *备份Nginx配置文件*
```
echo y|cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.default
```

## *修改 nginx.conf*
```
cat << EOF > /etc/nginx/nginx.conf
# For more information on configuration, see:
#   * Official English Documentation: http://nginx.org/en/docs/
#   * Official Russian Documentation: http://nginx.org/ru/docs/

user nginx;
worker_processes auto;
error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;

worker_rlimit_nofile 65535;

events {
    worker_connections 65535;
}

http {
    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;

    log_format  main  '\$host \$server_port \$remote_addr - \$remote_user [\$time_local] "\$request" '
                      '\$status \$request_time \$body_bytes_sent "\$http_referer" '
                      '"\$http_user_agent" "\$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile            on;
    tcp_nopush          on;
    tcp_nodelay         on;
    keepalive_timeout   65;
    types_hash_max_size 2048;

    server_names_hash_bucket_size 128;
    server_name_in_redirect off;
    client_header_buffer_size 32k;
    large_client_header_buffers 4 32k;

    client_header_timeout  3m;
    client_body_timeout    3m;
    client_max_body_size 50m;
    client_body_buffer_size 256k;
    send_timeout           3m;

    gzip  on;
    gzip_min_length  1k;
    gzip_buffers     4 16k;
    gzip_http_version 1.0;
    gzip_comp_level 2;
    gzip_types text/plain application/x-javascript text/css application/xml;
    gzip_vary on;

    proxy_redirect off;
    proxy_set_header Host \$host;
    proxy_set_header X-Real-IP \$remote_addr;
    proxy_set_header REMOTE-HOST \$remote_addr;
    proxy_set_header X-Forwarded-For \$proxy_add_x_forwarded_for;
    proxy_connect_timeout 60;
    proxy_send_timeout 60;
    proxy_read_timeout 60;
    proxy_buffer_size 256k;
    proxy_buffers 4 256k;
    proxy_busy_buffers_size 256k;
    proxy_temp_file_write_size 256k;
    proxy_next_upstream error timeout invalid_header http_500 http_503 http_404;
    proxy_max_temp_file_size 128m;
    #让代理服务端不要主动关闭客户端的连接，协助处理499返回代码问题
    proxy_ignore_client_abort on;

    fastcgi_buffer_size 64k;
    fastcgi_buffers 4 64k;
    fastcgi_busy_buffers_size 128k;

    index index.html index.htm index.php default.html default.htm default.php;

    # Load modular configuration files from the /etc/nginx/conf.d directory.
    # See http://nginx.org/en/docs/ngx_core_module.html#include
    # for more information.
    include /etc/nginx/conf.d/*.conf;
}
EOF
```

## *增加默认Host*
```
mkdir /etc/nginx/conf.d

cat << EOF > /etc/nginx/conf.d/default.conf
server {
    listen       80 default_server;
    listen       [::]:80 default_server;
    server_name  _;
    root         /usr/share/nginx/html;

    # Load configuration files for the default server block.
    include /etc/nginx/default.d/*.conf;

    location / {
    }

    error_page 404 /404.html;
        location = /40x.html {
    }

    error_page 500 502 503 504 /50x.html;
        location = /50x.html {
    }
}
EOF
```

## *启动Nginx*
```
systemctl start nginx
```

## *增加开机启动*
```
systemctl enable nginx
```

## *查看Nginx状态*
```
# systemctl status nginx
● nginx.service - nginx - high performance web server
   Loaded: loaded (/usr/lib/systemd/system/nginx.service; enabled; vendor preset: disabled)
   Active: active (running) since Mon 2020-05-25 05:50:22 EDT; 7s ago
     Docs: http://nginx.org/en/docs/
   CGroup: /system.slice/nginx.service
           ├─1853 nginx: master process /usr/sbin/nginx -c /etc/nginx/nginx.conf
           └─1854 nginx: worker process

May 25 05:50:22 mysql1 systemd[1]: Starting nginx - high performance web server...
May 25 05:50:22 mysql1 systemd[1]: Can't open PID file /var/run/nginx.pid (yet?) after start: No such file or directory
May 25 05:50:22 mysql1 systemd[1]: Started nginx - high performance web server.

# ss -antpl|grep nginx
LISTEN     0      128          *:80                       *:*                   users:(("nginx",pid=1854,fd=6),("nginx",pid=1853,fd=6))
LISTEN     0      128       [::]:80                    [::]:*                   users:(("nginx",pid=1854,fd=7),("nginx",pid=1853,fd=7))
```

# 搭建私有网盘

## 安装密码工具
yum install -y httpd-tools

## 使用密码工具生成密码文件
htpasswd -c /data/.htpasswd geek  (用户名)

## 增加网盘配置文件
```
mkdir -p /data/software

cat << EOF > /etc/nginx/conf.d/download.conf
server {
        listen       9090 default_server;
        server_name  _;
        root         /data/software;
        # 启用HTTP密码验证
        auth_basic "Administrator's password";
        auth_basic_user_file /data/.htpasswd;

        include /etc/nginx/default.d/*.conf;

        location / {
            autoindex on;
            autoindex_localtime on;
            # 设置文件大小显示单位
            autoindex_exact_size off;
        }
}
EOF
```

## 重载Nginx使配置生效
nginx -t && nginx -s reload

# 向网盘里面增加内容

## 增加QQ安装包

```
cd /data/software/
wget https://down.qq.com/qqweb/PCQQ/PCQQ_EXE/PCQQ2020.exe
```

# 访问网盘

http://服务器IP地址:9090

输入刚才配置的用户名密码就可以进行访问了

![下载站_01](https://ylighgh.gitee.io/blogparkcdn/images/下载站_01.png)

![下载站_02](https://ylighgh.gitee.io/blogparkcdn/images/下载站_02.png)


# 注意事项

* 网盘下载文件保存位置:/data/software/

* 如果是阿里云ECS要去添加安全组规则将9090端口放行

# 写在最后

如果文档对你有帮助的话，留个赞再走吧 ，你的点击是我的最大动力。

我是键盘侠，现实中我唯唯诺诺，网络上我重拳出击，关注我，持续更新Linux干货教程。

更多键盘侠Linux系列教程：[链接地址](https://www.cnblogs.com/MrKeyboard/category/1786086.html)

更多Linux干货教程请扫:

![wechatmansearch](https://ylighgh.gitee.io/blogparkcdn/images/wechatmansearch.jpg)

创作不易，打赏请扫:

![wechatpay](https://ylighgh.gitee.io/blogparkcdn/images/wechatpay.png)