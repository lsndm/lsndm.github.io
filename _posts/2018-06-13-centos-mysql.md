---
layout: post
title: "centos-mysql"
date: 2018-06-13 11:22:50 +0800
comments: true
categories: [linux, mysql]
---

JDK下载地址：http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html

MySQL5.7下载地址及依赖下载地址：http://repo.mysql.com/yum/mysql-5.7-community/el/7/x86_64/

centos 7相关软件下载地址：http://mirror.centos.org/centos-7/7/os/x86_64/Packages/

此安装方法适用于centos 7以下版本
===================================
mysql数据库服务器安装

	1.查看是否有安装MySQL数据库：rpm -qa | grep mysql
	2.没有则安装，上传MySQL的安装包到/home/softwares
	3.解压MySQL的.tar.gz：tar -xzvf mysql-5.7.19-linux-glibc2.12-x86_64.tar.gz
	4.修改解压后的文件夹名字为mysql-5.7.19
	5.创建/usr/local/mysql文件夹，并把mysql-5.7.19移动到/usr/local/mysql
	6.创建MySQL用户组及用户
		groupadd mysql
		useradd -s /sbin/nologin -r -g mysql mysql
	7.修改/usr/local/mysql目录所属组及所属用户
		chown -R mysql:mysql /usr/local/mysql/
	8.把MySQL的服务添加系统服务
		cp /etc/local/mysql/mysql-5.7.19/support-files/mysql.server /etc/init.d/mysqld
	9.创建MySQL数据库数据存储目录及日志保存目录
		mkdir /mysql
		mkdir /mysql/data
		mkdir /mysql/log
	10.修改/mysql文件夹的所属组及所属者
		chown -R mysql:mysql /mysql
	11.创建MySQL的配置文件my.cnf
		touch /etc/my.cnf
		vi /etc/my.cnf
		添加以下内容：
		[mysqld]
		basedir=/usr/local/mysql/mysql-5.7.19
		datadir=/mysql/data
		character-set-server=utf8
		port=3306
		default-storage-engine=INNODB
		user=mysqluser
		#skip-grant-tables
		[mysql]
		default-character-set=utf8
		[client]
		default-character-set=utf8
	12.初始化数据库
		/usr/local/mysql/mysql-5.7.19/bin/mysqld --initialize
	13.启动MySQL服务
		service mysqld start
	14.创建MySQL软连接
		ln -fs /usr/local/mysql/mysql-5.7.19/bin/mysql /usr/bin
	15.登陆MySQL同下


可以忽略上面的MySQL安装使用下面的安装方式并支持centos 7版本
=====================================================
二、mysql数据库服务器安装

	前置条件：
			服务器
				ip：***
				user：***
				password:***
			工具
				xshell
				xftp
				上传文件：快捷键Ctrl+Alt+F
			软件包
					MySQL安装包及用到的相关包及依赖:
						libaio-0.3.109-13.el7.x86_64.rpm
						mysql-community-common-5.7.19-1.el7.x86_64.rpm
						mysql-community-libs-5.7.19-1.el7.x86_64.rpm
						mysql-community-client-5.7.19-1.el7.x86_64.rpm
						mysql-community-server-5.7.19-1.el7.x86_64.rpm
					数据库文件.sql
	前置工作：
		添加Linux中MySQL用户，可以不添加用户...
			shell> useradd mysql
			shell> passwd mysql    
		查询系统中是否有mariadb数据库，有的话卸载避免和MySQL冲突
			shell> rpm -qa | grep -i mariadb
			shell> rpm -e mariadb-libs-5.5.56-2.el7.x86_64 --nodeps

	1.查看是否有安装MySQL数据库：rpm -qa | grep mysql
	2.没有则安装，上传MySQL的安装包到/home/softwares
	3.rpm安装方式，安装依赖项，顺序安装
		shell> rpm -ivh mysql-community-common-5.7.19-1.el7.x86_64.rpm
		shell> rpm -ivh mysql-community-libs-5.7.19-1.el7.x86_64.rpm
		shell> rpm -ivh mysql-community-client-5.7.19-1.el7.x86_64.rpm
		shell> rpm -ivh mysql-community-server-5.7.19-1.el7.x86_64.rpm
			warning: mysql-community-server-5.7.19-1.el7.x86_64.rpm: Header V3 DSA/SHA1 Signature, key ID 5072e1f5: NOKEY
			error: Failed dependencies:
				libaio.so.1()(64bit) is needed by mysql-community-server-5.7.19-1.el7.x86_64
				libaio.so.1(LIBAIO_0.1)(64bit) is needed by mysql-community-server-5.7.19-1.el7.x86_64
				libaio.so.1(LIBAIO_0.4)(64bit) is needed by mysql-community-server-5.7.19-1.el7.x86_64
				mysql-community-client(x86-64) >= 5.7.9 is needed by mysql-community-server-5.7.19-1.el7.x86_64
				mysql-community-common(x86-64) = 5.7.19-1.el7 is needed by mysql-community-server-5.7.19-1.el7.x86_64
			安装mysql警告 warning: mysql-community-server-5.7.19-1.el6.x86_64.rpm: Header V3 DSA/SHA1 Signature, key ID 5072e1f5: NOKEY，
			这是由于yum安装了旧版本的GPG keys造成的 解决办法：后面加上  --force --nodeps
			error解决办法：shell> rpm -ivh libaio-0.3.109-13.el7.x86_64.rpm
	4.编辑MySQL配置文件my.cnf:
		shell> vi /etc/my.cnf
		添加以下内容：
		[mysqld]
		character-set-server=utf8
		port=3306
		default-storage-engine=INNODB
		#user=mysqluser
		#skip-grant-tables
		[mysql]
		default-character-set=utf8
		[client]
		default-character-set=utf8



	5.启动MySQL服务：shell> systemctl start mysqld.service
	6.查看MySQL的启动状态:shell> systemctl status mysqld.service
	7、开机启动
		shell> systemctl enable mysqld
		shell> systemctl daemon-reload
	8.查询生成的随机密码：shell> grep 'temporary password' /var/log/mysqld.log
		如果能查到，就登陆，否则停止MySQL服务，编辑my.cnf,放开#skip-grant-tables注释，再启动MySQL服务	         
	9.登陆MySQL：shell> mysql -uroot -p
	10.修改MySQL的密码：
		mysql> alter user root@localhost identified by 'rootroot';（注：通过查询生成的随机密码登陆方式）
		mysql> show variables like '%password%';
		mysql> show variables like '%character%';
		mysql> exit

		以下为通过释放skip-grant-tables方式修改
		mysql> use mysql;
		mysql> update mysql.user set authentication_string=password('root'), password_expired='N' where user='root' and host='localhost';
		mysql> flush privileges;
		mysql> exit

	11.编辑my.cnf配置文件，注释掉skip-grant-tables，登陆MySQL然后添加用户并授权

		mysql> grant all privileges on *.* to 'test'@'%' identified by 'testtest' with grant option;
		查看ssl是否开启
		mysql> show global variables like '%ssl%';
		mysql> show databases;
		mysql> show tables;
		mysql> use mysql;
		mysql> desc user;
		mysql> select host, user, password_expired from user;

	查询软件及安装：shell> find / -name mysql*-community*


	导入数据
		创建数据上传.sql文件的路径
		shell> mkdir /home/mysql/data
		使用xftp上传.sql文件到/home/mysql/data路径下
		登陆MySQL数据库
		shell> mysql -uroot -p
		导入sql文件
		mysql> source /home/mysqluser/data/aaa.sql
		查看所有数据库名：
		mysql> show databases;
		使用jixi数据库
		mysql> use aaa;
		查看所有表名
		mysql> show tables;
