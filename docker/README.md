# 云计算实验三：Docker

注:因为拉了两次centos，所以后面的设备号有些地方有出入，不影响。

### 软件运行环境

###### 腾讯云 CentOS 7.6

# 子实验一：安装docker

* 更新应用程序库

> yum check-update
>

* 获取docker官方脚本下载

  > curl -fsSL  https://get.docker.com/ | sh
  >
  > ![图片加载慢](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/3.png)

* 用systemctl命令启动docker守护进程(服务器)

  > systemctl start docker
  >
  > //检查docker是否成功安装
  >
  > systemctl status docker
  >
  > ![图片加载慢](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/4.png)
  >
  > //检查版本
  >
  > docker version
  >
  > ![图片加载慢](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/6.png)

* 自启动

  > systemctl enable docker
  >
  > ![图片加载慢](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/5.png)

### 获取CentOS镜像

* 查找centos

  > docker search centos

* 拉到本服务器

  > docker pull centos
  >
  > ![图片加载慢](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/7.png)

* 尝试跑一下镜像

  > docker run centos
  >
  > //在仓库里检查是不是有这个镜像了
  >
  > ![图片加载慢](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/8.png)
  >
  > exit
  >
  > docker images
  >
  > ![图片加载慢](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/9.png)

# 子实验二：在CentOS镜像中搭建WordPress

### 预处理：遇到的问题

##### 1.由于centos容器里什么都没有，所以使用sysyemctl命令时会报错，需要下载。

##### 2.如果不设置端口映射，原先的wordpress会被覆盖。

#### 解决方案：

* 下载initscripts

> yum install initscripts
>
> ![图片加载慢](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/32_solution.png)

* 通过init在后台运行docker容器，通过exec的方式进入容器。
* 设置端口映射

> docker run -d -it --priviledged --name wordpress -o 8888:80 -d centos:7 /usr/sbin/init
>
> ![图片加载慢](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/21.png)

### 进入centos容器

> //85是需要进入的容器的简写
>
> docker exec -it 85 /bin/bash 
>
> ![图片加载慢](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/22.png)

### 下载并配置apache服务器

> yum install httpd
>
> ![图片加载慢](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/23.png)
>
> //开启apache服务器
>
> systemctl start httpd
>
> //设置自启动apache服务器（创建从/etc到/usr的link）
>
> // systemctl enable httpd
>
> ![图片加载慢](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/24.png)

### 数据库配置

> //下载数据库
>
>  yum install mariadb-server mariadb
>
> ![图片加载慢](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/25.png)
>
> //开启数据库守护进程并设置自启动
>
> systemctl start mariadb
>
> ![图片加载慢](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/26.png)
>
> systemctl enable mariadb
>
> ![图片加载慢](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/27.png)
>
> //设置数据库
>
> systemctl enable mariadb.service
>
> ![图片加载慢](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/27.png)
>
> ### 配置
>
> ![图片加载慢](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/db1.png)
>
> 
>
> 

### 启用PHP仓库

> //下载相关文件
>
> yum install epel-release yum-tyils
>
> ![图片加载慢](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/28.png)
>
> yum intstall http://rpms.remirepo.net/enterprise/remi-release-7.rpm
>
> yum-config-manager  --enable remi-php72
>
> ![图片加载慢](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/30.png)
>
> //安装php的数据库文件
>
> yum install php php-mysql
>
> ![图片加载慢](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/31.png)

安装成功后，通过查看版本检查php的安装是否成功

> php -v

##### 重启apache服务器

> systemctl restart httpd

#### 配置数据库

> mysql -u root -p
>

### 搭建WordPress

#### 安装

> yum install git
>
> ![图片加载慢](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/wd1.png)
>
> git clone http://gitee.com/helang_z/wordpress.git
>
> ![图片加载慢](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/wd2.png)
>
> cd wordpress
>
> ![图片加载慢](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/wd3.png)
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
> ![图片加载慢](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/wd4.png)
>
> //根据之前配置的数据库信息，修改DB_NAME、DB_PASSWORD、DB_HOST
>
> ![图片加载慢](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/wd5.png)

#### 登录wordpress

> //根据提示，修改信息，完成搭建。
>
> ![图片加载慢](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/wd6.png)
>
> ![](F:\dasan\Git\docker\docker\images\wd7.png)

### Docker Hub推送

> docker ps -a 查看容器

![图片加载慢](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/hub1.png)

> 在本地提交
>
> 格式 :docker commit -m "提交信息" -a "用户名" 容器ID  仓库名:标签 //将容器推送到指定的仓库

![图片加载慢](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/hb2.png)

> docker images 可以查看到前面前面的信息
>
> 再键入 docker tag IMAGE_ID 用户名/仓库名

![图片加载慢](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/hb3.png)

> 再次输入docker images 可以查看到tag的仓库

![图片加载慢](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/hb4.png)

> //登录docker hub
>
> docker login -u sonetto(用户名)
>
> //推送
>
> docker push sonetto/assignment(仓库名)

* 接下来就是漫长的等待~

![图片加载慢](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/hb5.png)

### 结果 

![图片加载慢](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/result.jpg)

# 子实验三：Dockfile

##### 准备的内容总共有这些

---------



![1574576736597](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/1574576736597.png)

#### 前期准备文件

-----------



![1574577681484](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/1574577681484.png)

![1574578177059](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\1574578177059.png)

（wordpress从何浪同学的github上下载：git clone http://gitee.com/helang_z/wordpress.git）

### Dockerfile内容

-------



![1574577217186](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/1574577217186.png)

### install.sh

---------



##### -包括apache、php、mysql、wordpress的安装

![1574577352782](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/1574577352782.png)



### mysql的配置

-------------



#### setup.sh

![1574577642829](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/1574577642829.png)

#### server.cnf

![1574577803476](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/1574577803476.png)

#### 数据库文件wordpress.sql

![1574577962980](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/1574577962980.png)

#### Dockerfile最后执行的shell脚本：start_all.sh

--------



![1574578068028](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/1574578068028.png)

之前没有加最后两句，容器创建成功了一直打不会开。

# 验证

#### 搭建image

![1574578449805](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/1574578449805.png)

#### 查看images

![1574578476076](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/1574578476076.png)

#### 创建并查看container

![1574578556972](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/1574578556972.png)

#### 查看container运行状态

![1574578588949](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/1574578588949.png)

##### 正在运行！

#### 查看容器内部文件

![1574578966192](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/1574578966192.png)

##### 说明前面COPY进来的东西都成功拷贝咯

#### 以交互的形式进入container，打开mysql

![1574578857239](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/1574578857239.png)

##### 查看到wordpress.sql中的数据库被成功创建咯

### 打开网页，输入ip及对应映射的端口号

![1574578663330](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/1574578663330.png)

#### 进去了也就说明apache服务器和php搭成了

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/1574578678942.png)

![1574578695124](https://github.com/sonettofighting/Cloud_Computing/blob/master/docker/images/1574578695124.png)

搭建成功



#感谢感谢898989898989989898989！