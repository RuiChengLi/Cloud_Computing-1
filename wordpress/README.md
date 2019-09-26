# 腾讯云CentOS 7上搭建WordPress

## 实验环境

- 腾讯云 CentOS 7.6 64位

## 实验要求

- HTTP服务器：Apache Web 服务器；
- 数据库：MySQL；
- 建站工具：WordPress（基于PHP）。

## 步骤

---------------------



#### 安装Apache Web服务器

> yum install httpd //直接用root用户登录,普通用户需要加sudo命令

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/wordpress/wdprs_imgs/1.jpg)

#### 启动apache服务器

> systemctl start httpd.service

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/wordpress/wdprs_imgs/2.jpg)

#### 检验服务器是否成功开启

- 在本地浏览器输入公网IP地址,出现testing界面

  ![](https://github.com/sonettofighting/Cloud_Computing/blob/master/wordpress/wdprs_imgs/3.jpg)

  ---------------

  

####  安装数据库

> yum install mariadb-server mariadb

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/wordpress/wdprs_imgs/4.jpg)

#### 启动数据库及交互脚本

> systemctl start mariadb

> mysql_secure_installation

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/wordpress/wdprs_imgs/5.jpg)

#### 设置数据库

> systemctl enable mariadb.service //根据个人需要，follow提示选择y/n就OK,为了避免出错我选择了没有设置密码,虽然这样很不安全

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/wordpress/wdprs_imgs/6.jpg)

-------------------

#### 启用PHP仓库

> yum install epel-release yum utils
>
> yum install http://rpms.remirepo.net/enterprise/remi-release-7.rpm

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/wordpress/wdprs_imgs/7.jpg)

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/wordpress/wdprs_imgs/8.jpg)

> yum-config-manager --enable remi-php72

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/wordpress/wdprs_imgs/9.jpg)

#### 安装PHP以及php-mysql

> yum install php php-mysql

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/wordpress/wdprs_imgs/10.jpg)

#### 安装成功后查看PHP的版本

> php -v

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/wordpress/wdprs_imgs/11.jpg)

#### 重启apache服务器

> systemctl restart httpd.service

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/wordpress/wdprs_imgs/12.jpg)

#### 安装PHP模块

> search php-

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/wordpress/wdprs_imgs/13.jpg)

> yum install php -fpm php-gd

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/wordpress/wdprs_imgs/14.jpg)

#### 重启apache服务

> service httpd restart

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/wordpress/wdprs_imgs/16-restart.jpg)

#### 测试:创建info.php并将其置于Web服务的根目录下

> vi /var/www/html/info_test.php
>
> <?php phpinfo(); ?>

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/wordpress/wdprs_imgs/16_5.jpg)

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/wordpress/wdprs_imgs/17.jpg)

#### 在本地浏览器中输入公网IP/info_test.php查看是否成功

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/wordpress/wdprs_imgs/18.jpg)

----------------------

#### 为wordpress创建数据库

> mysql -u root -p

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/wordpress/wdprs_imgs/19.jpg)

- 创建数据库wordpress

- 创建用户u1

- 授予用户u1root权限

- 刷新u1权限

- 退出

  > create database wordpress;
  >
  > create user u1@localhost identified by  '';
  >
  > grant all privileges on wordpress to u1localhost identified by '';
  >
  > flush privileges;
  >
  > exit

  ![](https://github.com/sonettofighting/Cloud_Computing/blob/master/wordpress/wdprs_imgs/20.jpg)

#### 安装wordpress

> cd ~
>
> wget http://wordpress.org/latest.tar.gz

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/wordpress/wdprs_imgs/21.jpg)

#### 解压

> tar xzvf latest.tar.gz

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/wordpress/wdprs_imgs/22.jpg)

#### 将主目录下的wordpress文件夹与apache服务器根目录同步

（使得wordpress的内容能够被访问）

>  rsync -avP ~/wordpress/ /var/www/html/

#### 在apache服务器根目录下创建一个文件夹保存wordpress上传的文件

> mkdir /var/www/html/wp-content/uploads

#### 对apache服务器目录及wordpress（之前同步的和新创建的）文件夹设置权限

> chown -R apache:apache /var/www/html/*

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/wordpress/wdprs_imgs/24.jpg)

--------------------------

#### 配置wordpress

> cd /var/www/html //apache服务器
>
> cp wp-config-sample.php wp-config.php

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/wordpress/wdprs_imgs/26.jpg)

#### 在wordpress配置文件中修改数据库信息

> nano wp-config.php

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/wordpress/wdprs_imgs/27.jpg)

#### 在本地浏览器登录wordpress

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/wordpress/wdprs_imgs/28.jpg)

#### 配置结束后登录

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/wordpress/wdprs_imgs/29.jpg)

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/wordpress/wdprs_imgs/30.jpg)

### 做到这一步，配置结束

继续探索ing

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/wordpress/wdprs_imgs/33.jpg)