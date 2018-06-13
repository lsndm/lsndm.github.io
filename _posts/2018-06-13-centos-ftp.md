---
layout: post
title: "centos-ftp"
date: 2018-06-13 10:55:50 +0800
comments: true
categories: [linux, ftp]
---

[ftp安装文档](/public/file/centos-ftp.docx)

首先检查是否安装过： `rpm -qa | grep vsftpd`
如果出现  vsftpd-xxx，那就说明安装了。

没有即进行以下安装。

[下载 FTP][1]

  [1]: http://rpmfind.net/linux/rpm2html/search.php?query=vsftpd(x86-64)

找到对应的版本进行下载即可。

将下载好的包，上传到服务器。

1、 安装vsftpd

　　`rpm -ivh vsftpd-3.0.2-21.el7.x86_64.rpm`

2、 启动ftp服务器

　　[root@localhost ~]# `service vsftpd start`

　　为 vsftpd 启动 vsftpd：[确定]

3、 配置

　　[root@localhost ~]# `whereis vsftpd`

　　vsftpd:  /usr/sbin/vsftpd    /etc/vsftpd    /usr/share/man/man8/vsftpd.8.gz

　　yum安装的主要目录为上述的3个目录，其中配置文件vsftpd.conf在/etc/vsftpd中，下面看下怎么配置vsftpd.conf

　　# 默认配置文件: /etc/vsftpd/vsftpd.conf

　　`cd /etc/vsftpd`

　　备份： `cp vsftpd.conf vsftpd.conf_bak`

　　编辑： `vi vsftpd.conf`

　　关于 vsftpd.conf 的选项及说明，请看   https://pan.baidu.com/s/1kVgJdGV


4、添加ftp防火墙规则：

　　/sbin/iptables -I INPUT -p tcp --dport 21 -j ACCEPT

　　/sbin/iptables -A INPUT -p tcp --dport 6000:7000 -j ACCEPT

　　/etc/rc.d/init.d/iptables save

　　/etc/init.d/iptables restart

5、添加用户（注意，该处添加nologin类型用户ftpuser）：

　　`useradd -d /home/ftp -s /sbin/nologin ftpup`

　　`passwd ftpup`

　　输入用户密码

　　再次输入密码

　　重新启动

　　`service vsftpd stop`

　　`service vsftpd start`

9、 使用 ftp 命令在本机进行测试

　　比如 windows 上的 ftp 功能　　

　　ftp> `open <ip> <端口>`

　　　　输入账号和密码。注意：下面操作需要关闭防火墙，不然会出现好多错误。

　　ftp> `put c:\test.html` （回车）

　　　　当屏幕提示你已经传输完毕，可以键入相关命令查看：

　　ftp> `dir` （回车）

　　ftp> `bye`（回车） 退出 ftp 模式

　　总结一下常用的FTP命令：

　　　　1. open：与服务器相连接；

　　　　2. send(put)：上传文件；

　　　　3. get：下载文件；

　　　　4. mget：下载多个文件；

　　　　5. cd：切换目录；

　　　　6. dir：查看当前目录下的文件；

　　　　7. del：删除文件；

　　　　8. bye：中断与服务器的连接



10、使用 ftp 客户端

　　比如 Xftp

　　如果弹出 “无法显示远程文件夹” 的对话框，则进行以下解决

　　因为 ftp 连接模式有 port模式和 pasv模式。客户端一般默认使用的 pasv(被动模式) 。

　　修改方式  点击属性  ->  选项  ->  把 “使用被动模式” 选项 去掉 即可
　

rpm命令介绍：
========================
　　安装

　　rpm -ivh 软件包全名 //如：`rpm -ivh vsftpd-2.2.2-12.el6_5.1.i686.rpm`

　　查看软件是否有安装

　　rpm -q 软件包名 //如：`rpm -q vsftpd`

　　卸载软件:`rpm -e 软件包名`

　　查看系统里边全部rpm方式安装的软件(query all)：`rpm -qa`

　　模糊查找已经安装了包名含有ftp的软件：`rpm -qa | grep ftp`

ftp客户端安装
=================================

　　离线安装:`rpm -ivh ftp-0.17-54.el6.x86_64.rpm`

　　在线安装:`rpm -Uvh http://mirror.centos.org/centos/6/os/x86_64/Packages/ftp-0.17-54.el6.x86_64.rpm`

　
linux下面怎样让给一个用户添加对指定文件夹写的权力
===============================================

　　`chown -R usr:usergroup /usr/local/bin`

　　`chmod u+w,-x,o-w-r /usr/local/bin`

　　usr为你的普通用户

　　usergroup为这个普通用户的组

　　u+w表示你的普通用户拥有写权限

　　-x表示所有用户都没有执行权限

　　o-w-r表示除了普通用户，其他用户都没有写和读权限

***

CentOS 7.0默认使用的是firewall作为防火墙
=========================================

1、关闭firewall：

　　`systemctl stop firewalld.service #停止firewall`

　　`systemctl disable firewalld.service` #禁止firewall开机启动

　　`firewall-cmd --state` #查看默认防火墙状态（关闭后显示notrunning，开启后显示running）

2、防火墙规则添加

　　修改配置文件/etc/firewalld/zones/public.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<zone>
  <short>Public</short>
  <description>For use in public areas.</description>
  <rule family="ipv4">
    <source address="122.10.70.234"/>
    <port protocol="udp" port="514"/>
    <accept/>
  </rule>
  <rule family="ipv4">
    <source address="123.60.255.14"/>
    <port protocol="tcp" port="10050-10051"/>
    <accept/>
  </rule>
 <rule family="ipv4">
    <source address="192.249.87.114"/> 放通指定ip，指定端口、协议
    <port protocol="tcp" port="80"/>
    <accept/>
  </rule>
<rule family="ipv4"> 放通任意ip访问服务器的9527端口
    <port protocol="tcp" port="9527"/>
    <accept/>
  </rule>
</zone>
```
***

firewall常用命令
===========================
　　`service firewalld restart` 重启

　　`service firewalld start` 开启

　　`service firewalld stop` 关闭

　　`systemctl status firewall` 查看firewall服务状态

　　`firewall-cmd --state` 查看firewall的状态

　　`firewall-cmd --list-all` 查看防火墙规则

***

CentOS切换为iptables防火墙
=====================================

1、关闭firewall：

　　`service firewalld stop`
　　`systemctl disable firewalld.service` #禁止firewall开机启动

2、安装iptables防火墙

　　`yum install iptables-services` #安装

3、编辑iptables防火墙配置

　　`vi /etc/sysconfig/iptables` #编辑防火墙配置文件

下边是一个完整的配置文件：

    Firewall configuration written by system-config-firewall

    Manual customization of this file is not recommended.

    *filter

    :INPUT ACCEPT [0:0]

    :FORWARD ACCEPT [0:0]

    :OUTPUT ACCEPT [0:0]

    -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

    -A INPUT -p icmp -j ACCEPT

    -A INPUT -i lo -j ACCEPT

    -A INPUT -m state --state NEW -m tcp -p tcp --dport 22 -j ACCEPT

    -A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT

    -A INPUT -m state --state NEW -m tcp -p tcp --dport 3306 -j ACCEPT

    -A INPUT -j REJECT --reject-with icmp-host-prohibited

    -A FORWARD -j REJECT --reject-with icmp-host-prohibited

    COMMIT

　　:wq! #保存退出

　　`service iptables start` #开启

　　`systemctl enable iptables.service` #设置防火墙开机启动


centos各种安装包下载地址
==============================

http://mirror.centos.org

centos 6.9版本安装包下载地址
=================================

http://mirror.centos.org/centos-6/6.9/os/x86_64/Packages/
