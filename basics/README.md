# 云计算开发基础

## 准备工作

- 购买腾讯云服务器 centOS 7.6版本

  [腾讯云学生计划]:( https://cloud.tencent.com/act/campus)

- 安装Git并注册

  [Git]: ( https://git-scm.com/downloads)

- 下载PuTTY端实现远程登录云服务器（老师给的教程使用XShell,但是这个免费,性质似乎都一样）

  [PuTTY]: ( https://www.putty.org/)

  ![1568774507304](https://github.com/sonettofighting/SONETTO/blob/master/imgs/1568774507304.png)根据腾讯云公网IP地址远程登录

  ![1568774562582](https://github.com/sonettofighting/SONETTO/blob/master/imgs/1568774562582.png)

  

## SSH key

- ##### 验证是否有ssh key，在Git Bash客户端输入指令

  > ls -al ~/.ssh

![1568774842039](https://github.com/sonettofighting/SONETTO/blob/master/imgs/1568774842039.png)

如果出现id_rsa和id_rsa.pub文件，则ssh key存在，以下步骤可以跳过。

- ##### 创建新的ssh key

> ssh-keygen -t rsa -b 4096 -C “1013278470@qq.com”

![1568775033256](https://github.com/sonettofighting/SONETTO/blob/master/imgs/1568775033256.png)

复制密钥的内容到GitHub的SSH key中

[GitHub]: https://github.com/settings/keys

![1568775186669](https://github.com/sonettofighting/SONETTO/blob/master/imgs/1568775186669.png)

- ##### 测试SSH Key是否配置成功

  ![1568775317719](https://github.com/sonettofighting/SONETTO/blob/master/imgs/1568775317719.png)

  此时配置成功

  ## 创建GitHub项目并在本地同步

- ##### 新建代码仓库

  ![1568775758250](https://github.com/sonettofighting/SONETTO/blob/master/imgs/1568775758250.png)

此处不初始化README文档，之后用直接touch。

- ##### 创建本地代码仓库

  ![1568775932395](https://github.com/sonettofighting/SONETTO/blob/master/imgs/1568775932395.png)

新建一个文件夹用于存放代码，右键单击bash here。

![1568775985916](https://github.com/sonettofighting/SONETTO/blob/master/imgs/1568775985916.png)

复制先前创建的代码仓url。

> git remote add origin https://github.com/sonettofighting/SONETTO.git

![1568776138442](https://github.com/sonettofighting/SONETTO/blob/master/imgs/1568776138442.png)

由于之前已经添加此代码仓，所以提示已经存在。

> git pull origin master
>
> git touch README.md //由于做本实验的时候已经有这个文件了，在此就用一个test的文本文件替代
>
> git status 

> git add . //添加所有modified文件

![1568776462683](https://github.com/sonettofighting/SONETTO/blob/master/imgs/1568776462683.png)

>  git commit -m "sonettofighting"
>
> git push -u origin master

提交指令

![1568776623176](https://github.com/sonettofighting/SONETTO/blob/master/imgs/1568776623176.png)

- ##### 上传成功

  ![1568776667918](https://github.com/sonettofighting/SONETTO/blob/master/imgs/1568776667918.png)

在代码仓查看到刚刚修改的文件

# 安装VMware和CentOS系统

*此处推荐使用VMware workstation player，在非商业用途时免费* 

[VMware]: ( https://www.vmware.com/products/workstation-player/workstation-player-evaluation.html)

[CentOS]: ( https://www.centos.org/)

- ##### 创建新的虚拟机

  ![1568777036750](https://github.com/sonettofighting/SONETTO/blob/master/imgs/1568777036750.png)

![1568776999617](https://github.com/sonettofighting/SONETTO/blob/master/imgs/1568776999617.png)

语言设置为英语，安装桌面GUI，并设置开启网络，其他选项自行选择。

![1568777533434](https://github.com/sonettofighting/SONETTO/blob/master/imgs/1568777533434.png)

![1568777475021](https://github.com/sonettofighting/SONETTO/blob/master/imgs/1568777475021.png)

*有IP地址出现时，说明可以连接网络。*

此处要注意，网络开启有一个选项最好要打开，不然之后可能会遇到网络打不开需要修改脚本的问题。

![1568777456332](https://github.com/sonettofighting/SONETTO/blob/master/imgs/1568777456332.png)

~~（初学者不会真的会被吐）~~

接下来根据提示，设置root用户、当前用户密码，并同意term信息。

- ##### 成功登录

![1568777888396](https://github.com/sonettofighting/SONETTO/blob/master/imgs/1568777888396.png)

赶紧检查一下网络，可以登录

![1568777946824](https://github.com/sonettofighting/SONETTO/blob/master/imgs/1568777946824.png)

到此结束！！！
