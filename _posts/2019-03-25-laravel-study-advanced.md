---
title: Laravel学习-高级篇
categories:
- note
tags:
- laravel
---

> 主要介绍composer和Artisan控制台的使用

## composer快速入门

> 对于现代语言而言，包管理器基本时标配，比如java的Maven，NodeJs的NPM，Objective-C的Cocoapods，PHP的时PEAR，但是PHP的PEAR有一些缺点，如依赖处理容易出现问题，配置非常复杂，难用的命令行接口等，所以就有了Composer的出现。

### Composer简介

* Composer是PHP的一个依赖（dependency）管理工具，不是一个包管理器。它涉及“packages"和"libraries"。
* 在项目中声明所依赖的外部工具库（libraries）Composer 会自动安装这些工具库及依赖的库文件。
* 比如你需要用到A库，但是A库的安装需要安装B库和C库，使用Compser的时候只需要声明使用A库，就能自动安装B库和C库了。

### 安装Composer

* 安装方式
  * Composr-Setup.exe

    > WIn操作系统，需翻墙

  * Composer.phar

    > 通用安装方式，不需要翻墙

* Composer.phar安装方式
  * 直接下载

    > https://getcomposer.org/download/

  * 命令行下载

  ```shell
  php -r "readfile(‘https://getcomposer.org/installer’);" | php
  ```

  * 局部安装

    > 将composer.phar文件复制到任意目录（比如项目根目录下），然后通过 php composer.phar 指令即可使用Composer了

  * 全局安装
    > 全局安装，就只需要用composer这个命令就能调用compser了

    * Mac或者Linux系统

    ```shell
    sudo mv composer.phar /usr/local/bin/composer
    ```
    接下来就试试composer这个命令，遇到权限问题，就需要将composer改成775权限

    * Windows系统

      > 将composer.phar拷贝到php.exe同级目录，新建composer.bat文件，并将下面代码保存到该文件中，执行`@php"%~dpOcomposer.phar" %*`


### 使用Composer中国全量镜像

* **为什么要用Composer中国镜像？**
  * 安装包的数据是从github.com上下载的，安装包的元数据是从packagist.org上下载的

  * 国外的网站连接速度很慢，并且随时可能被“墙”

  * Composer 中国全量镜像所做的就是缓存所有安装包和元数据到国内的机房并通过国内的CDN进行加速，这样就不必再去向国外的网站发起请求

* **如何配置？**

  > [Packagist / Composer 中国全量镜像](https://pkg.phpcomposer.com/)

  * **查看当前镜像地址**

  ```shell
  composer config -gl
  ```

  * **系统全局配置**

  ```shell
  composer config -g repo.packagist composer https://packagist.phpcomposer.com
  ```

  * **单个项目配置**

  打开命令行窗口（windows用户）或控制台（Linux、Mac 用户），进入你的项目的根目录（也就是 composer.json 文件所在目录），执行如下命令：

  ```shell
  composer config repo.packagist composer https://packagist.phpcomposer.com
  ```

  上述命令将会在当前项目中的 composer.json 文件的末尾自动添加镜像的配置信息（你也可以自己手工添加）：

  ```php
  "repositories": {
    		"packagist": {
        		"type": "composer",
        		"url": "https://packagist.phpcomposer.com"
    		}
  }
  ```

  * **解除镜像**

  如果需要解除镜像并恢复到 packagist 官方源，请执行以下命令：

  ```shell
  composer config -g --unset repos.packagist
  ```

### 使用composer

* **创建composer.json文件**
    ```shell
    composer init
    ```
* **查看库是否存在以及相关信息**
   ```shell
   composer search `库名`
   ```
* **查看库的版本**
    ```shell
    composer show --all `库名`
    ```
    找到需要安装的版本，如monolog/monolog 1.21.0版本
* **安装库**
    编辑`composer.json`
    ```json
    {
    "name": "iaoe/test",
    "description": "test",
    "type": "library",
    "require": {
        "monolog/monolog":"1.21.*"
        }
    }
    ```
    使用以下命令来完成安装
    ```
    composer install
    ```

 * **使用require来安装依赖**
    ```shell
    composer require symfony/http-foundation
    ```

* **修改依赖**
    首先修改`composer.json`文件，删掉想要删除的依赖，再要以下命令完成修改
    ```shell
    composer update
    ```
### 使用Composer安装laravel框架
> [安装 | 《Laravel 5.6 中文文档》](https://learnku.com/docs/laravel/5.6/installation/1352)
> 其实laravel也是composer的一个包，所以可以用composer安装它

* **通过Composer Create-Project安装**
    ```shell
    composer create-project --prefer-dist laravel/laravel   项目名
    ```

* **通过Laravel安装器安装**
    1. 首先安装Laravel安装器
        ```shell
        composer global require laravel/installer
        ```
    2. 接下来要将laravel配置到环境变量里
        > 这里展示Linux的配置方法，配置全局环境变量的方法

        先修改`/etc/profile`文件,再下面添加
        ```
        export PATH="~/.config/composer/vendor/bin:$PATH"
        ```
        最后输入命令`source /etc/profile`就可以了
    3. 新建laravel项目
        ```shell
        laravel new 项目名
        ```

## Artisan控制台
> Artisan 是Laravel中自带的命令行工具的名称, 由强大的Symfony Console组件驱动的, 提供了一些对应用开发有帮助的命令

### Artisan使用帮助

> [Artisan 命令行 | 《Laravel 5.8 中文文档》](https://learnku.com/docs/laravel/5.8/artisan/3913)
> Artisan 是 Laravel 自带的命令行接口，他提供了许多使用的命令来帮助你构建 Laravel 应用 ，需要先进入laravel的项目根目录中才能使用

* **查看所有可用的Artisan的命令（list）**
    ```shell
    php artisan
    php artisan list
    ```
* **查看命令的帮助信息（help）**
    ```
    php artisan help migrate
    ```

### Artisan基本使用

* 创建控制器
    ```shell
    php artisan make:controller 控制器名称
    ```
* 创建模型
    ```shell
    php artisan make:model 模型名称
    ```
* 创建中间件
    ```shell
    php artisan make:middleware 中间件名称
    ```
    这样就能快速创建这些之前的组件了

### Artisan创建用户登录注册功能—Auth

>这里通过几个简单的命令行，就能实现快速创建用户登录注册，忘记密码的功能

1. 生成Auth所需的文件

    ```shell
    php artisan make:auth
    ```
2. 接下来在浏览器访问/home，可以发现有登录注册表

3. 迁移
    ```shell
    php artisan migrate
    ```
    迁移成功后就会发现数据库有新的表了