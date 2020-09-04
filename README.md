# web_strategy

## 如何搭建个人网站

环境：腾讯云服务器Ubuntu Server 18.04.1 TLS 64位

### 1 WordPress安装

#### 1.1 配置LAMP环境

LAMP = Linux + Apache + MySQL + PHP

（1）安装Apache2

```shell
sudo apt-get install apache2 -y
```

安装完成后，输入IP地址或配置过DNS解析的域名，即可看到 **Apache2 Ubuntu Default Page** 页面，说明apache安装成功。

（2）安装PHP组件

```shell
sudo apt-get install php -y
sudo apt-get install libapache2-mod-php
```

（3）安装MySQL服务

```shell
sudo apt-get install mysql-server -y
sudo apt-get install php-mysql
```

在安装过程中，会出现配置页面，如果不熟悉可以直接选择yes进行默认配置。

此外，还需要输入两次MySQL密码，该密码需妥善保存。

（4）安装phpmyadmin

```shell
sudo apt-get install phpmyadmin -y

#建立软连接
sudo ln -s /usr/share/phpmyadmin /var/www/html/phpmyadmin
```



重启MySQL服务和Apache服务，完成环境配置。

```shell
sudo service mysql restart
sudo systemctl restart apache2.service
```



#### 1.2 安装配置WordPress

（1）下载安装Wordpress

```shell
wget https://cn.wordpress.org/latest-zh_CN.tar.gz

##解压
sudo tar -zxvf latest-zh_CN.tar.gz
```

解压完成后，即可在Wordpress文件夹中看到Wordpress源码。

（2）移动Wordpress

我们将Wordpress文件夹源码移动到网站的根目录下，这里我们将网站的根目录设置为/var/www/html/example.com。

```shell
##移动wordpress源码
sudo mkdir /var/www/html/example.com
sudo mv wordpress /var/www/html/example.com/wordpress_5.4.2

##配置网站根目录
cd /etc/apache2/sites-available
sudo vim 000-default.conf

##修改DocumentRoot
DocumentRoot /var/www/html/yiweizzz.com/wordpress_5.4.2
```

（3）配置Wordpress数据库

进入mysql，创建一个wordpress数据库，为wordpress数据库设置一个用户以及用户密码，并为该用户配置数据库访问权限。

```shell
sudo mysql -u root -p

## 进入mysql
mysql> CREATE DATABASE wordpress;
mysql> CREATE USER 'user' IDENTIFIED BY 'password';
mysql> GRANT ALL PRIVILEGES ON wordpress.* TO 'user';
mysql> FLUSH PRIVILEGES; #生效这些配置

## 退出mysql
mysql> exit;

```

（4）配置Wordpress

在浏览器中访问IP或域名，按步骤进行操作。

用户名和密码即在MySQL中设置的用户名和密码。

```
Database Name: wordpress
Username: user
Password: password
Database Host: localhost
Table Prefix: dm_
```

（5）配置FTP服务

在需要安装wordpress模板时，会要求输入FTP用户名和密码，我们可以进行如下设置，即可无需输入用户名密码，就可以进行模板安装。

```
cd /var/www/html/example.com/wordpress_5.4.2
sudo vim wp-config.php
```

在wp-config.php文件中添加如下指令。

```
define("FS_METHOD", "direct");
define("FS_CHMOD_DIR", 0777);
define("FS_CHMOD_FILE", 0777);
```

即可成功使用Wordpress！！


