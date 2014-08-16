README.md
## Jog
这是一个用Nancy框架写的博客引擎，主要是自用。
演示：[just1n.net](http://just1n.net)

###在Ubuntu上安装方法
1、Ubuntu安装mono。

    sudo apt-get install software-properties-common
    sudo add-apt-repository ppa:inizan-yannick/mono
    sudo apt-get update
    sudo apt-get install mono-devel
2、在本机安装mono。

3、修改源码的配置文件appSettings节点下内容。

4、本机编译源码。

    "C:\Program Files (x86)\Mono-3.0.2\bin\xbuild.bat" Jog.sln
把编译好的源码上传到Ubuntu上，假设路径是`/mono/app/jog`

5、Ubuntu安装Nginx。

    sudo apt-get install nginx
新建配置文件` /etc/nginx/sites-available/jog`，内容如下：

    server {
        listen       80;
        server_name  yourdomainname.com;
        root /mono/app/jog;
    
        location /Content/ {
            alias /mono/app/jog/Content/;
            location ~*  \.(jpg|jpeg|png|gif|ico|css|js|ttf)$ {
                expires 365d;
            }
        }
    
        location / {
                proxy_pass http://localhost:3579;
        }
    }
link一下：

    sudo ln -s /etc/nginx/sites-available/jog /etc/nginx/sites-enabled/jog
    sudo /etc/init.d/nginx reload
6、安装supervisor。

    apt-get install supervisor
新建配置文件`/etc/supervisor/conf.d/jog.conf`

    [program:jog]
    command=mono /mono/app/jog/Jog.exe -d
    user=youruser
    stderr_logfile = /var/log/supervisor/jog-err.log
    stdout_logfile = /var/log/supervisor/jog-stdout.log
    directory=/mono/app/jog/
`sudo supervisorctl reload`

That's all.

  [1]: http://just1n.net/2014/08/about-jog

