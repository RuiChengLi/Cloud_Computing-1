# 云计算实验三：Docker

注:因为拉了两次centos，所以后面的设备号有些地方有出入，不影响。

### 软件运行环境

###### 腾讯云 CentOS 7.6

## 安装docker

* 更新应用程序库

> yum check-update
>
> ![](F:\dasan\Git\docker\docker\images\图片2.png)

* 获取docker官方脚本下载

  > curl -fsSL  https://get.docker.com/ | sh
  >
  > ![](F:\dasan\Git\docker\docker\images\3.png)

* 用systemctl命令启动docker守护进程(服务器)

  > systemctl start docker
  >
  > //检查docker是否成功安装
  >
  > systemctl status docker
  >
  > ![](F:\dasan\Git\docker\docker\images\4.png)
  >
  > //检查版本
  >
  > docker version
  >
  > ![](F:\dasan\Git\docker\docker\images\6.png)

* 自启动

  > systemctl enable docker
  >
  > ![](F:\dasan\Git\docker\docker\images\5.png)

## 获取CentOS镜像

* 查找centos

  > docker search centos

* 拉到本服务器

  > docker pull centos
  >
  > ![](F:\dasan\Git\docker\docker\images\7.png)

* 尝试跑一下镜像

  > docker run centos
  >
  > //在仓库里检查是不是有这个镜像了
  >
  > ![](F:\dasan\Git\docker\docker\images\8.png)
  >
  > exit
  >
  > docker images
  >
  > ![](F:\dasan\Git\docker\docker\images\9.png)

## 在CentOS镜像中搭建WordPress

### 预处理：遇到的问题

##### 1.由于centos容器里什么都没有，所以使用sysyemctl命令时会报错，需要下载。

##### 2.如果不设置端口映射，原先的wordpress会被覆盖。

#### 解决方案：

* 下载initscripts

> yum install initscripts
>
> ![](F:\dasan\Git\docker\docker\images\32_solution.png)

* 通过init在后台运行docker容器，通过exec的方式进入容器。
* 设置端口映射

> docker run -d -it --priviledged --name wordpress -o 8888:80 -d centos:7 /usr/sbin/init
>
> ![](F:\dasan\Git\docker\docker\images\21.png)

### 进入centos容器

> //85是需要进入的容器的简写
>
> docker exec -it 85 /bin/bash 
>
> ![](F:\dasan\Git\docker\docker\images\22.png)

### 下载并配置apache服务器

> yum install httpd
>
> ![](F:\dasan\Git\docker\docker\images\23.png)
>
> //开启apache服务器
>
> systemctl start httpd
>
> //设置自启动apache服务器（创建从/etc到/usr的link）
>
> // systemctl enable httpd
>
> ![](F:\dasan\Git\docker\docker\images\24.png)

### 数据库配置

> //下载数据库
>
>  yum install mariadb-server mariadb
>
> ![](F:\dasan\Git\docker\docker\images\25.png)
>
> //开启数据库守护进程并设置自启动
>
> systemctl start mariadb
>
> ![](F:\dasan\Git\docker\docker\images\26.png)
>
> systemctl enable mariadb
>
> ![](F:\dasan\Git\docker\docker\images\27.png)
>
> //设置数据库
>
> systemctl enable mariadb.service
>
> ![](F:\dasan\Git\docker\docker\images\27.png)
>
> ### 配置
>
> ![](F:\dasan\Git\docker\docker\images\db1.png)
>
> 
>
> 

### 启用PHP仓库

> //下载相关文件
>
> yum install epel-release yum-tyils
>
> ![](F:\dasan\Git\docker\docker\images\28.png)
>
> yum intstall http://rpms.remirepo.net/enterprise/remi-release-7.rpm
>
> yum-config-manager  --enable remi-php72
>
> ![](F:\dasan\Git\docker\docker\images\30.png)
>
> //安装php的数据库文件
>
> yum install php php-mysql
>
> ![](F:\dasan\Git\docker\docker\images\31.png)

安装成功后，通过查看版本检查php的安装是否成功

> php -v

##### 重启apache服务器

> systemctl restart httpd

#### 配置数据库

> mysql -u root -p
>
> 

### 搭建WordPress

#### 安装

> yum install git
>
> ![](F:\dasan\Git\docker\docker\images\wd1.png)
>
> git clone http://gitee.com/helang_z/wordpress.git4
>
> ![](F:\dasan\Git\docker\docker\images\wd2.png)
>
> cd wordpress
>
> ![](F:\dasan\Git\docker\docker\images\wd3.png)
>
> tar -xzvf wordpress-5.2.4-tar.gz

#### 配置

> cd /var/www/html
>
> cp wp-config-sample.php wp-config.php
>
> //如果不copy的话，后续登录wordpress会让你创建一个新的
>
> //因为是空的容器，所以vim、nano都没有
>
> vi wp-config.php
>
> ![](F:\dasan\Git\docker\docker\images\wd4.png)
>
> //根据之前配置的数据库信息，修改DB_NAME、DB_PASSWORD、DB_HOST
>
> ![](F:\dasan\Git\docker\docker\images\wd5.png)

#### 登录wordpress

> https://106.54.38.228:8888
>
> //根据提示，修改信息，完成搭建。
>
> ![](F:\dasan\Git\docker\docker\images\wd6.png)
>
> ![](F:\dasan\Git\docker\docker\images\wd7.png)

### Docker Hub推送

> docker ps -a 查看容器

![](F:\dasan\Git\docker\docker\images\hub1.png)

> 在本地提交
>
> 格式 :docker commit -m "提交信息" -a "用户名" 容器ID  仓库名:标签 //将容器推送到指定的仓库

![](F:\dasan\Git\docker\docker\images\hb2.png)

> docker images 可以查看到前面前面的信息
>
> 再键入 docker tag IMAGE_ID 用户名/仓库名

![](F:\dasan\Git\docker\docker\images\hb3.png)

> 再次输入docker images 可以查看到tag的仓库

![](F:\dasan\Git\docker\docker\images\hb4.png)

> //登录docker hub
>
> docker login -u sonetto(用户名)
>
> //推送
>
> docker push sonetto/assignment(仓库名)

* 接下来就是漫长的等待~

![](F:\dasan\Git\docker\docker\images\hb5.png)

# 结果 

![](F:\dasan\Git\docker\docker\images\result.jpg)

