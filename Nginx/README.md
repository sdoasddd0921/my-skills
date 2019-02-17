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

