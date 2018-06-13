---
layout: post
title: "centos-jdk-tomcat"
date: 2018-06-13 12:08:50 +0800
comments: true
categories: [linux, jdk, tomcat]
---


一.jdk安装
====================
		(1)查看是否有安装jdk:
			shell> rpm -qa | grep jdk
		(2)没有则安装,上传jdk的安装包到/home/softwares（没有则创建：shell> mkdir /home/softwares）目录下
		(3)rpm方式安装：
			shell> cd /home/softwares
			shell> rpm -ivh jdk-8u151-linux-x64.rpm
		(4)查询jdk安装路径：
			shell> which -a java
			显示：/usr/bin/java
			shell> ls -lrt /usr/bin/java
			显示：lrwxrwxrwx 1 root root 22 Nov 15 09:27 /usr/bin/java -> /etc/alternatives/java
			shell> ls -lrt /etc/alternatives/java
			显示：lrwxrwxrwx 1 root root 35 Nov 15 09:27 /etc/alternatives/java -> /usr/java/jdk1.8.0_151/jre/bin/java
		(4)配置环境变量：
			shell> vi /etc/profile
			在文件末尾添加如下内容
			export JAVA_HOME=/usr/java/jdk1.8.0_151
			export JRE_HOME=$JAVA_HOME/jre
			export CLASSPATH=.:$JAVA_HOME/lib:$JRE_HOME/lib
			export PATH=$JAVA_HOME/bin:$PATH
		(6)重新载入系统配置文件使其生效：
			shell> source /etc/profile
		(7)查看配置路径是否正确：
			shell> echo $JAVA_HOME
			shell> echo $CLASSPATH
			shell> echo $JRE_HOME
			shell> echo $PATH
		(8)查看是否安装成功：
			shell> java -version

***

二.tomcat安装
==================================
		(1)创建/tomcat目录shell> mkdir /tomcat
		(2)上传tomcat的.tar.gz到服务器并解压到/tomcat目录
			shell> tar -xzvf apache-tomcat-7.0.79.tar.gz -C /tomcat
		(3)修改tomcat的配置文件server.xml:
			shell> vi /tomcat/apache-tomcat-7.0.79/conf/server.xml
			<Connector port="80" protocol="HTTP/1.1"
	               connectionTimeout="20000"
	               redirectPort="8443" URIEncoding="UTF-8" />
	    (4)启动tomcat:
	    	shell> /tomcat/apache-tomcat-7.0.79/bin/startup.sh
	    (5)在浏览器输入http://ip测试tomcat是否可以成功访问

***

[jdk-tomcat文档下载](/public/file/centos-jdk-tomcat.docx)
