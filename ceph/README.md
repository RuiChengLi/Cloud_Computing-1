# CEPH分布式存储集群在VMware上的实现

新年快乐！

## Step 1 - Configure VMWare Machines

以minimal模式创建一台虚拟机

![最小化安装](https://github.com/sonettofighting/Cloud_Computing/blob/master/ceph/imgs/图片1.png)

以root用户登录后，创建需要的CEPH用户

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/ceph/imgs/图片2.png)

赋予CEPH用户root的权限

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/ceph/imgs/root.jpg)

安装NTP，同步所有节点的时间

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/ceph/imgs/QQ截图20191212201457.jpg)

安装open-vm-tool

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/ceph/imgs/QQ截图20191212201526.jpg)

关闭SELinux服务 (Security-EnhancedLinux)

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/ceph/imgs/selinux.jpg)

打开network-scripts/ifcfg，设置onboot="on"，确保网络通信

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/ceph/imgs/ifcfg.jpg)

重启网络服务，使配置生效

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/ceph/imgs/QQ截图20191212201255.jpg)

开启防火墙（其实因为在本地，此步可以跳过）

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/ceph/imgs/云计算二号-2019-12-13-22-43-20.png)

此时，可以复制虚拟机了。

以当前虚拟机为模板，复制三台：mon1(管理节点)、osd1、osd2（存储节点）

选择创建完整克隆

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/ceph/imgs/QQ截图20191212201938.jpg)

创建完克隆之后，同时开启四台虚拟机，并用ip addr命令查看各自ip。

在这里我的IP是：

```
admin	192.168.154.129

monitor	192.168.154.130

osd1	192.168.154.131

osd2	192.168.154.132
```

在四台虚拟机上，都要编辑/etc/hosts文件
![](https://github.com/sonettofighting/Cloud_Computing/blob/master/ceph/imgs/ip.png)

完成后，四台虚拟机应该可以互相2ping通

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/ceph/imgs/ping.png)

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/ceph/imgs/ping2.png)

## Step 2 - Configure the SSH Server

安装ssh server

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/ceph/imgs/云计算二号-2019-12-13-08-48-29.png)

登录cephuser，生成ssh key并修改config文件

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/ceph/imgs/ssh.png)

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/ceph/imgs/config.png)

copy ssh id

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/ceph/imgs/ssh2.png)

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/ceph/imgs/copyid.png)

做到此步，可以在admin机上 直接通过ssh + username 命令免密码登录所有从节点

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/ceph/imgs/ssh3.png)

## Step 3 - Build the Ceph Cluster

在admin节点机上，下载ceph deployment tool '**ceph-deploy**'

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/ceph/imgs/rpm.png)

创建一个目录，用于分派、部署、存储配置信息。

（本来我用的目录名是cluster，之后由于有错，重新创建了目录“new cluster”）

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/ceph/imgs/mkdir.png)

设置管理节点为mon1

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/ceph/imgs/模拟.png)

配置./ceph.config文件 

注意defaultpool最小应该有2个节点，否则不能达到activate+clean的health状态

这算个坑吧，反正我跳进去了

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/ceph/imgs/1.png)

admin上，执行install命令，在所有节点上安装ceph

```
ceph-deploy install ceph-admin mon1 osd1 osd2
```

epel-release可能会出错，只要执行删除操作即可

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/ceph/imgs/云计算二号-2019-12-13-23-00-02.png)

登录osd1、osd2，创建供分发的目录，修改权限并返回admin用户，进行ceph-deploy prepare。

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/ceph/imgs/云计算二号-2019-12-14-09-09-39.png)

激活节点：

```
ceph-deploy osd activate osd1:/osd1 osd2:/osd2
```

得到结果应该最后会是这样

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/ceph/imgs/云计算二号-2019-12-14-09-04-48.png)

部署管理节点

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/ceph/imgs/云计算二号-2019-12-13-14-30-29.png)

得到keyrings:

```
ceph-deploy gatherkeys mon1
```

为所有节点部署management-key

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/ceph/imgs/云计算二号-2019-12-14-09-14-45.png)

为keyrings添加可读权限

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/ceph/imgs/云计算二号-2019-12-14-09-15-27.png)

## Step 4 - Testing the Ceph setup

登录mon1管理节点，查看health状态

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/ceph/imgs/云计算二号-2019-12-14-09-20-02.png)

![](https://github.com/sonettofighting/Cloud_Computing/blob/master/ceph/imgs/结果.png)

