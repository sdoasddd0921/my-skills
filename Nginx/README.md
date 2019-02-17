# Nginx

高性能的HTTP和反向代理服务。

## 使用

- **安装**

  使用 `brew`、`yum`、`apt` 等 Linux 包管理程序命令进行安装，或者访问[官网](https://nginx.org)下载安装程序进行安装。

- **常用命令**

  `nginx -V` 查看 Nginx 的版本信息，并带有环境变量等具体信息。

  `nginx -t` 可以用来检查当前的配置是否正确。

  `nginx -c <配置文件路径>` 可以指定配置文件，如果使用相对路径，会使用默认配置的 `--prifix` 前缀地址。

  `nginx -p <prefix path>` 设置 Nginx 的前缀地址，目前已知对 `-c` 使用的相对路径有效，并且在配置文件中 server 节点使用的 root，index 等配置项貌似也有用。

  `nginx -s <stop|quit|reopen|reload>` 控制 Nginx 停止、重启等。Stop 相当于关电源，quit 相当于执行安全关机命令。

- 结束方式

  通过使用 `ps aux | grep nginx` 来查看运行中的 Nginx 的 PID，然后结束进程。

  或者使用 `lsof -i tcp:<prot>` 来查看被占用的端口中是否有 Nginx，然后根据对应的 PID 结束进程。

## 配置文件

Nginx 是基于配置文件运行的。可以通过 `-V` 命令查看默认的配置文件位置，在 linux 和 macOS 以及 windows 中的位置貌似都不一样，参考着编写自定义的配置。

**注意1**，貌似必须要 events 配置。

**注意2**，server 节点里的 server_name 写了其他的网址的时候，需要修改系统的 hosts 文件将改地址指向 `172.0.0.1`，否则访问地址是不会跳转到 Nginx 代理的，并且访问 localhost 只会默认返回第一个 server 节点的代理内容。

**注意3**，http 配置项中 `include` 的文件的位置貌似是配置文件的相对位置。

**注意4**，server 配置项中 location 项里的 root 貌似又是以 Nginx 的 prefix 配置为相对路径的。

贴一下**默认配置**：

```

#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;

    server {
        listen       8080;
        server_name  a.com;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            root   html;
            index  index.html index.htm;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }


    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}


    # HTTPS server
    #
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;

    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;

    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;

    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}
    include servers/*;
}

```



