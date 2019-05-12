---
title: 阿里云简易部署laravel项目
categories:
- 教程
tags:
- laravel
---

> 因为自己在部署Laravel系统上遇到了很多坑，所以写下一篇教程来记录下阿里云部署Laravel的步骤，这里我使用的是阿里云的`轻量服务器`，系统是`ubuntu16.04`，Laravel的版本是`5.8`。
>
> 书写于2019/04/14 16:52

## 使用LNMP自动部署脚本搭建环境

> [Ubuntu 14/16 下的 Laravel LNMP 线上环境自动部署脚本](https://learnku.com/laravel/t/2814/ubuntu-1416-under-the-laravel-lnmp-online-environment-automatically-deploy-scripts)
>
> 这里我使用的`Laravel中文社区`的站长`Summer`的`一键部署脚本`来部署Laravel环境，因为本人是个新手，虽然也手动搭过LNMP环境，但总是有一点小问题，所以直接用这个脚本来搭建，这里你也可以使用其他的一键脚本如`LNMP.org`的一键安装包，或者自己手动搭建环境。

1. 登录你的Linux服务器，将脚本下载下来并运行

    ```shell
    wget -qO- https://raw.githubusercontent.com/summerblue/laravel-ubuntu-init/master/download.sh - | bash
    ```

2. 之后你会看到这样的提示，这里需要稍等片刻

   ```shell
   ===> 开始下载...
   ===> 下载完毕
   
   安装脚本位于： /root/laravel-ubuntu-init
   ===> 正在初始化系统...
   ```

3. 安装完成后会显示`mysql`的密码，这里需要记下这个密码，以便之后的操作

   ```shell
   ===> 开始下载...
   ===> 下载完毕
   
   安装脚本位于： /root/laravel-ubuntu-init
   ===> 正在初始化系统...    [DONE]
   ===> 正在初始化软件源...    [DONE]
   ===> 正在安装基础软件...    [DONE]
   ===> 正在安装 PHP...    [DONE]
   ===> 正在安装 Mysql / Nginx / Redis / Memcached / Beanstalkd / Sqlite3...    [DONE]
   ===> 正在安装 Nodejs / Yarn...    [DONE]
   ===> 正在安装 Composer...    [DONE]
   安装完毕
   Mysql root 密码：MM6aIdOHC0a7Gt6eT8eLH1rtuaSzmE4h
   请手动执行 source ~/.bash_aliases 使 alias 指令生效。
   ```

4. 根据提示，输入命令完成最后一步的安装

   ```shel
   source ~/.bash_aliases
   ```

5. 配置运行环境，添加Nginx站点

   ```shell
   ~/laravel-ubuntu-init/16.04/nginx_add_site.sh
   ```

6. 根据提示输入项目名和域名

   ```shell
   请输入项目名：输入你的项目名，如Laravel等，最好和你要部署的Laravel项目名相同
   请输入站点域名（多个域名用空格隔开）：yun.iaoe.xyz
   域名列表：这里输入你的域名，可以用阿里云的ip代替
   项目名：xcx
   项目目录：/var/www/xcx
   是否确认？ [y/N] y
   配置文件创建成功
   Nginx 重启成功
   ```

7. 项目的地址就位于`var/www/你的项目名`，在这个位置新建一个`public`文件夹，并新建一个`index.php`文件，文件内容为大家喜闻乐见的

   ```php
   <?php
       phpinfo();
   ```

8. 访问你的公网ip(阿里云的ip地址)或者域名，出现php的版本信息页面就算搭建完成了。



## 部署项目到服务器

> 这里的操作相对比较简单，但比较烦的一点是，步骤少一步可能就会访问不了，由于在这方面是个新手，所以在这里耗了很长时间，才想到写一篇文章来记录下过程

1. 上传项目文件到服务器上

   > 这里我使用的是git的方式传输，当然你也可以使用其他方式，值得注意的一点是，就像上面的第7点里面写的，你也大概猜到项目文件大概要怎么放了吧，就是把Laravel项目放到`var/www`这个路径里，要确保你的Laravel项目名和刚才你设置的项目文件名相同

   ```shell
   root@iZwz9bk0r4isk428e4rgb8Z:/var/www# ls
   html  你的项目文件名
   ```

2. 现在`cd`进入你的项目里，依次输入以下命令，来修改你的权限

   ```shell
   chown -R www-data:www-data storage/
   chown -R www-data:www-data bootstrap/cache/
   chmod -R 775 storage/
   chmod -R 775 bootstrap/cache/
   ```

3. 接下来安装依赖，等待片刻

   ```shell
   composer install
   ```

4. 复制`.env.example`文件，并生成key

   ```shell
   cp .env.example .env
   php artisan key:generate
   ```

5. 接下来就可以访问你的域名或者公网ip(阿里云服务器ip)，见到你熟悉的`laravel`页面了





## 参考链接

[Ubuntu 14/16 下的 Laravel LNMP 线上环境自动部署脚本 | Laravel China 社区 - 高品质的 Laravel 开发者社区](https://learnku.com/laravel/t/2814/ubuntu-1416-under-the-laravel-lnmp-online-environment-automatically-deploy-scripts)

[轻松部署 Laravel 应用 | 《02. 一键脚本》 | Laravel China 社区 - 高品质的 Laravel 开发者社区](https://learnku.com/articles/24918)

[使用Log分析Laravel的Internal Server Error（code 500） - 简书](https://www.jianshu.com/p/24138be4d9ed)

[Laravel 出现 No application encryption key has been specified. - Hseven779的博客 - CSDN博客](https://blog.csdn.net/hseven779/article/details/78743713)

[laraval 放到 nginx 服务器上之后，运行报 500 错误 | Laravel China 社区 - 高品质的 Laravel 开发者社区](https://learnku.com/laravel/t/4420/laraval-on-the-nginx-server-running-500-error)

[LNMP 环境部署 Laravel 项目的一些总结 | Laravel China 社区 - 高品质的 Laravel 开发者社区](https://learnku.com/articles/18322)

[php - How to fix Error: laravel.log could not be opened? - Stack Overflow](https://stackoverflow.com/questions/23411520/how-to-fix-error-laravel-log-could-not-be-opened)