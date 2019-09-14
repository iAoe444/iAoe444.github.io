---
title: ubuntu安装owncloud教程
categories:
- tutorial
tags:
- cloud
- geek
- tool
---

> 首先我的环境已经安装上了php7.2，nginx以及apache2，用的是阿里云的轻量服务器，装的系统是ubuntu18.04，用的是自定义安装的方式，官方有快速安装方式，废话不多说，开始安装owncloud教程吧
> 书写于`2019/09/14 20:34`

## 下载owncloud包


可以在owncloud自己的网盘里下载[download2.owncloud.org](https://download.owncloud.org/community/)，选择最新版本就行了，我选择的是`owncloud-10.2.0.tar.bz2`，使用命令

```bash
wget https://download.owncloud.org/community/owncloud-10.2.0.tar.bz2
```

这里下载有点慢，所以我用IDM进行下载，然后再传到ubuntu上

## 安装owncloud

1. 解压owncloud

   ```bash
   sudo tar xjf owncloud-10.2.0.tar.bz2 
   ```

2. 将owncloud放入apache2目录里

   ```bash
   sudo cp -r -v owncloud/ /var/www/html/
   ```

3. 开始创建用户存储目录

   ```bash
   cd /var/www/html/owncloud/
   sudo mkdir data
   ```

4. 更改owncloud目录中的相关权限

   ```bash
   sudo chown -R www-data:www-data data
   sudo chown -R www-data:www-data config
   sudo chown -R www-data:www-data apps
   ```

5. 配置apache2

   ```bash
   nano /etc/apache2/apache2.conf
   ```
   
   ```
   <Directory /var/www/html/owncloud/>  
           Options Indexes FollowSymLinks  
           AllowOverride All  
           Require all granted  
   </Directory>
   ```
   
   ![](https://ws1.sinaimg.cn/large/006bBmqIgy1g6zd22qkdrj30ay0ccjro.jpg)

## 为owncloud创建一个数据库

```bash
sudo mysql -u root -p
```

```mysql
CREATE DATABASE owncloud;
GRANT ALL ON owncloud.* to 'owncloud'@'localhost' IDENTIFIED BY 'test1234';
FLUSH PRIVILEGES;
exit
```

所以现在的新建了一个数据库叫`owncloud`，建了一个只可以操作这个`owncloud`库的用户`owncloud`，密码是`test1234`

## 使用nginx反向代理（没有该需求可以忽略）

> 这里由于使用的是nginx进行反向代理，实现多站点，目前服务器里面跑了三个web服务，有一个是用apache2服务器的，这里我们再建一个virtualhost，将项目跑在8089端口

1. 让apache2监听8089端口

   ```bash
   nano /etc/apache2/ports.conf
   ```

   ```
   # If you just change the port or add more ports here, you will likely also
   # have to change the VirtualHost statement in
   # /etc/apache2/sites-enabled/000-default.conf
   
   Listen 8088 #这个是之前的项目
   Listen 8089	#这里监听8089端口，添加这一段就行
   
   <IfModule ssl_module>
           Listen 443
   </IfModule>
   
   <IfModule mod_gnutls.c>
           Listen 443
   </IfModule>
   
   # vim: syntax=apache ts=4 sw=4 sts=4 sr noet
   ```

2. 新增virtualhost

   ```bash
   nano /etc/apache2/apache2.conf 
   ```

   ```
   ServerAdmin iAoe@localhost
   Servername localhost:80
   ErrorLog /etc/apache2/logs/error.log
   CustomLog /etc/apache2/logs/access.log combined
   <VirtualHost *:8089>
           DocumentRoot /var/www/html/owncloud/
           ServerName 这里填写域名
   </VirtualHost>
   ```

   ![](https://ws1.sinaimg.cn/large/006bBmqIgy1g6zcs39qqjj30eq0ao0t7.jpg)
   
3. 创建nginx配置文件

   ```
   nano /etc/nginx/conf.d/随便起名.conf
   ```

   ```
   server {
           listen       80;
           server_name 自己的域名  localhost;
           access_log  logs/xxxx.com.access.log;
           index index.html index.php index.htm index;
          location /
           {
               proxy_next_upstream http_502 http_504 error timeout invalid_header;
               proxy_set_header Host  $host;
               proxy_set_header X-Real-IP $remote_addr;
               proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
               proxy_pass http://127.0.0.1:8089;#注意修改这个端口
   	}
   }
   ```

## 使用https（没有该需求可以忽略）

这里使用的非常简单的的https工具，[Certbot](https://certbot.eff.org/)，之前使用过，所以直接使用下面的命令就可以了

```bash
sudo certbot --nginx
```

## 重启服务和配置owncloud

```bash
service apache2 restart
service nginx restart
```

现在应该可以通过`http://服务地址/owncloud`，或者如果你做了反向代理，应该可以通过你设置的域名直接访问了，需要做最后一步配置

![](https://ws1.sinaimg.cn/large/006bBmqIly1g6zdbvu8t9j30ee0fqgmi.jpg)

这样就安装完成了，相对来说算是比较简单的

![](https://ws1.sinaimg.cn/large/006bBmqIly1g6zdgq1eerj31hy0r6gmi.jpg)

## 参考链接

[Linux下配置Nginx+Apache+PHP+Tomcat+Java同时运行 - 王瑞的博客 - CSDN博客](https://blog.csdn.net/Axela30W/article/details/78865165)

[在ubuntu16.04上安装owncloud - 简书](https://www.jianshu.com/p/7290d5329321)