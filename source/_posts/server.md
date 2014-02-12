title: 服务器解决emoji表情
date: 2014-02-12 18:12:43
tags: [服务器]
categories: [服务器]
---
坑爹的android 和iphone 需要是用emoji ，前端需要处理，后端也需要处理。
亲测可用，自己修改了下内容，来自http://blog.csdn.net/shootyou/article/details/8236886 

使用系统CentOS 6.2本来已经系统自带安装了mysql 5.1，但是奈何5.1不支持utf8mb4字符集，无法支持emoji，只能想办法将Mysql升级到5.5。

这果然是一次蛋疼的升级过程。

完整步骤：
### 1.首先备份数据，虽说成功的升级数据不会丢失，但是保险起见备份下。

	mysqldump -u xxx -h xxx -P 3306 -p --all-databases > databases.sql  

最好连数据文件和配置文件也备份一份。

	cp -R /data/mysql mysql-5.1-data  
	cp /etc/my.cnf my.cnf-5.1  

备份完之后停止mysql服务。

	service mysqld stop  

好了，开始进入正题。

### 2.卸载旧版本的Mysql

	yum remove mysql mysql-*  

执行之后再看看是不是残余一些mysql-libs之类的

	yum list installed | grep mysql  

如果有，并确认没用之后也可以删除。

	yum remove mysql-libs  

注意删除mysql-libs可能会对一些依赖软件产生影响，这里我们不讨论。
好了，卸载的动作基本结束。

### 3.安装Mysql5.5

这里使用yum安装的过程。

在走了N多弯路之后我发现需要首先安装一些新的repo。

	rpm -Uvh http://dl.fedoraproject.org/pub/epel/6/i386/epel-release-6-8.noarch.rpm  
	rpm -Uvh http://mirrors.neusoft.edu.cn/epel/6/i386/epel-release-6-8.noarch.rpm  
	rpm -Uvh http://packages.sw.be/rpmforge-release/rpmforge-release-0.5.2-2.el6.rf.x86_64.rpm  
	rpm -Uvh http://dl.iuscommunity.org/pub/ius/stable/Redhat/6/x86_64/epel-release-6-5.noarch.rpm  
	rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-6.rpm  
	
这个时候你再运行：

	yum --enablerepo=remi,remi-test info mysql mysql-server  
就会发现mysql的版本已经是5.5.x了。毫不犹豫安装之。

	yum --enablerepo=remi,remi-test install mysql mysql-server  
	
安装到此结束。接下来是启动和升级。
### 4.启动和升级
这个时候你想直接启动十有八九会报错，主要的问题两块：

一是配置文件，5.5相比5.1有些配置改名了，这个需要你对照启动错误日志一点点改进。

二是没有执行mysql_upgrade。

在确保配置文件没问题之后运行:

	mysql_upgrade -u root -p  

等他全部ok。
再试试运行mysql。

	service mysqld start  

保佑你看到的是绿色的[ok]。


