# 如何在Ubuntu上快速部署个人网页

### 前言

​	**NGINX** 是增长最快、最受欢迎的 Web 服务器。NGINX 是一款功能强大的 Web 服务器、反向代理和负载均衡器，以其高性能、稳定性和可扩展性而闻名。它通常用于提供 Web 内容、处理传入流量并将其分发到多个服务器。

​	**PHP** 是一种广泛使用的开源脚本语言，专为 Web 开发而设计。从创建动态网页开始，PHP现在用于开发桌面应用程序。PHP 以其易用性、灵活性和对不同操作系统和 Web 服务器的广泛支持而闻名。(简单易用)

​	**MySQL** 是采用最广泛的开源关系数据库，是许多流行网站、应用程序和商业产品的主要关系数据存储。

​	**WordPress** 用于创建网站、博客，甚至一些 Web 应用程序。它已成为一种流行且功能强大的内容管理系统 （CMS）

​	**FTP**（文件传输协议 英语：**F**ile **T**ransfer **P**rotocol，缩写**FTP**）是在计算机网络的客户端和服务器间传输文件的应用层协议网络传输协议)。在这里用来解决wordpress文件上传的问题,同时将功能模块化

​	本篇博客分享如何用WordPress快速搭建一个网页

### 环境:

​	Ubuntu22.04LTS(发行版间差别不大)

### 一.更新系统包

```bash
sudo apt update
sudo apt upgrade
```

### 二.安装Nginx

```bash
sudo apt install nginx
sudo systemctl status nginx			#检查nginx状态,若处于active状态,则安装成功
sudo systemctl start nginx			#启动nginx服务器
sudo systemctl enable nginx			#nginx开机自启动
curl http://localhost				#返回页面信息
```

### 三.安装MySql

```bash
sudo apt install -y mysql-server	#-y参数意思是安装时全选yes
sudo systemctl status mysql
#sudo mysql_secure_installation)	#可选
```

在`mysql_secure_installation`过程中，会提示进行一些安全设置，如设置MySQL root密码、删除匿名用户、禁止root远程登录等。根据提示完成配置即可。

### 四.创建WordPress数据库和用户

##### 方法一:

```bash
sudo mysql -u root -p				#进入mysql命令行
进入后显示 mysql>
```

在MySql命令行中执行以下SQL命令

```mysql
CREATE DATABASE <数据库名>;			#创建一个名为<xxx>的数据库
CREATE USER <用户名>@localhost IDENTIFIED BY <密码>;		#创建一个名为<xxx>的用户,localhost指定该用户只能从本地主机进行连接
GRANT ALL PRIVILEGES ON <数据库名>.* TO <用户名>@localhost;	#给予<xxx>用户操作指定数据库下所有数据的权限
EXIT;
```

##### 方法二:

```bash
sudo mysqladmin create <数据库名>;		#创建名为<xxx>的数据库
CREATE USER <用户名>@localhost IDENTIFIED BY <密码>;
GRANT ALL PRIVILEGES ON <数据库名>.* TO <用户名>@localhost;
EXIT;
```

##### 在安装PHP前,先对MySql进行测试

```bash
mysql -u <用户名> -p <数据库名>
提示输入密码,验证通过进入mysql命令行,说明配置成功
mysql>
```

```bash
如果不成功,显示ERROR 1045(28000),可重新键入
mysql -u <用户名> -p <数据库名>		或
SET PASSWORD for <用户名>@localhost
```

### 五.安装PHP

```bash
sudo apt install -y php-fpm php-mysql php-curl php-mbstring php-imagick php-xml php-zip
```

##### WordPress 需要多个 PHP 模块才能正常运行:

- MySQL	用于连接到MySQL数据库。
- cURL	用于发出远程请求的 cURL。
- Mbstring	处理多字节字符串。
- ImageMagick	执行图像大小调整等操作。
- XML	提供 XML 支持。
- Zip 	以解压缩插件、主题和 WordPress 更新包。

```bash
sudo systemctl status php8.1-fpm.service		#检查fpm服务器运行状态
```

### 六.下载并配置WordPress

```bash
cd /var/www/html		#进入Nginx默认的web目录
sudo wget https://wordpress.org/latest.tar.gz		#下载wordpress的最新版本
sudo tar -xzvf latest.tar.gz						#解压下载的压缩包
sudo mv wordpress/* .
sudo rm -rf wordpress latest.tar.gz
```

(这里没有建立多个网站的需求,所以直接将压缩包内容放到了/var/www/html目录下)

##### 配置WordPress文件权限

```bash
sudo chown -R www-data:www-data /var/www/html
sudo chmod -R 755 /var/www/html
```

##### 命令说明:

这里的chown改变了文件或目录的所有者和所属组;

-R为递归处理参数,对目录及目录内的所有子目录和文件的所有者进行变更;

www-data:www-data 意思是将文件权限给予www-data用户并将它们加入到www-data用户组。前者确保只有web服务器进程可以修改这些文件,提高安全性;后者保证web服务器能够正常访问和处理这些文件。

第二条命令涉及文件的读写和执行权限问题,之后会有单独的博客文章进行解释

### 七.配置Nginx

##### 创建一个新的Nginx服务器块配置文件

```bash
sudo vim /etc/nginx/sites-available/wordpress
```

##### 获取php-fpm服务器版本**(重要)**

```bash
ls /var/run/php
获取php<>-fpm.sock的版本号
```

##### 在文件中添加如下内容

```nginx
server {
    listen 80;
    server_name <替换为自己的域名或ip>;
    root /var/www/html;
    
    index index.php index.html index.htm index.nginx-debian.html;

    location / {
        try_files $uri $uri/ /index.php?$args;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php<版本号>-fpm.sock;
    }

    location ~ /\.ht {
        deny all;
    }
}
```

##### 激活配置文件并禁用默认的配置文件

```bash
sudo ln -s /etc/nginx/sites-available/wordpress /etc/nginx/sites-enabled/
sudo unlink /etc/nginx/sites-enabled/default
```

##### 解释:

Nginx 通常使用以下目录结构来管理站点配置：

- **`/etc/nginx/sites-available/`**：这个目录包含所有可用的站点配置文件。这里的文件不一定被 Nginx 激活。
- **`/etc/nginx/sites-enabled/`**：这个目录包含所有已启用的站点配置文件。Nginx 只会加载这个目录中的配置文件。
- 通过将 `sites-available` 目录中的配置文件链接到 `sites-enabled` 目录，可以方便地启用或禁用站点，而无需复制文件。

通过删除符号链接禁用默认配置文件

- `unlink`：是一个用于删除文件或符号链接的命令。与 `rm` 命令不同，`unlink` 只能删除单个文件或符号链接。
- `/etc/nginx/sites-enabled/default`：需要删除的文件或符号链接的路径。

##### 测试Nginx配置是否正确

```bash
sudo nginx -t
```

返回successful说明配置正确

##### 重启Nginx

```bash
sudo systemctl reload nginx
```

### 八.完成WordPress安装

##### 打开浏览器,访问设置的域名或ip地址,进入wordpress配置面板后按提示完成安装

### 九.wordpress上传文件出现413错误

WordPress上传文件出现413错误是由于上传文件大小超过了服务器的限制,可以通过如下两种方法解决:

1.解除或放缓上传文件大小限制(不推荐)

2.搭建FTP服务器(推荐)

##### 方法一:

- 修改Nginx配置文件

```bash
sudo vim /etc/nginx/nginx.conf
```

在`http`块中添加或修改以下行

```nginx
http {
	client_max_body_size 100M;
}
```

保存并关闭文件后重启Nginx

- 修改PHP配置文件

```bash
sudo vim /etc/php/<版本号>/fpm/php.ini
```

找到并修改以下行

```ini
upload_max_filesize = 100M
post_max_size = 100M
```

保存并关闭文件后,重启PHP-FPM

```bash
sudo systemctl restart php<版本号>-fpm
```

##### 方法二:

(方法一易出错,增加了模块间的耦合度,同时不利于管理,故更推荐搭建FTP服务器)

- 安装vsftpd

```bash
sudo apt update
sudo apt install vsftpd
```

- 配置vsftpd

编辑vsftpd配置文件

```bash
sudo vim /etc/vsftpd.conf
```

修改如下属性

```conf
anonymous_enable=NO
local_enable=YES
write_enable=YES
chroot_local_user=YES
```

- 保存并关闭文件,重启vsftpd

```bash
sudo systemctl restart vsftpd
sudo systemctl enable vsftpd
```

- 创建FTP用户

```bash
sudo adduser <用户名>
```

会提示设置密码,输入即可

- 为该用户设置web目录的访问权限

为了让上传文件夹更好管理,建议创建一个FTP目录

```bash
sudo mkdir /var/www/html/<FTP文件夹名>
sudo chown -R <用户名>:<用户名> /var/www/html/<FTP文件夹名>
```

使用FTP客户端(FileZilla或WinSCP),使用刚创建的FTP用户进行连接。

连接之后可以将文件上传至/var/www/html/<FTP文件夹名>目录,然后使用ssh进行文件管理
