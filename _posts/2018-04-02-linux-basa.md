---
layout: post
title: "Linux"
date: 2018-04-02 10:55:50 +0800
comments: true
categories: [linux]
---


centos 6
=========================

操作  |  命令
-------------                 | -------------
格式化查看当前系统时间          |  date + %Y-%m-%d %H:%M:%S
立刻关机                       |  shutdown -h now \| halt
10分钟以后关机                 |  shutdown -h +10
12点整的时候关机               |  shutdown -h 12:00:00
重启                          |  shutdown -r now \| reboot
清屏                          |  clear \| Ctrl + l(快捷键)
退出当前进程                   |  Ctrl + c(快捷键)
查看当前目录下的子节点信息      |  ls \| ll \| ls -al
切换到home目录                 |  cd /home
切换到用户主目录               |  cd ~
回到上次所在目录               |  cd -
ip地址查看                    |  ifconfig
创建相对路径文件夹             |  mkdir aaa
级联创建目录                   |  mddir -p aaa/bbb/ccc
删除空目录                     |  rmdir aaa
删除aaa整个文件夹及其所有子节点  |  rm -r aaa
强制删除aaa                    |  rm -rf aaa
修改文件夹名称                 |  mv aaa bbb
移动文件夹aaa到home目录                                 | mv aaa /home
创建空文件                                              |  touch test.txt
重定向(清空)文件                                        |  echo "I miss You, my baby" > test.txt
向文件追加内容                                          |  echo "hello" \>\> test.txt
编辑文件                                               |  vi test.txt
编辑文件并将光标移至最后一行行首                         |  vi + test.txt
编辑文件并将光标移至第n行行首                            | vi +n test.txt
编辑文件并将光标置于第一个与pattern匹配的串处             | vi +/pattern test.txt
编辑文件第n行并将与pattern匹配的串替换为空                | vi +ns/pattern/ test.txt
跳到文件的首行                                          | gg
跳到文件的末行                                          | G
删除一行                                               |  dd
删除3行                                                | 3dd
复制一行                                               | yy
复制3行                                                | 3yy
粘贴                                                   | p
undo                                                   | u
显示行号                                                | :set nu
查找光标所在行的第一个a，替换成b                          |  :s/a/b
查找文中所有a，替换成b                                   |  :%s/a/b
查找关键字you，并定位到第一个找到的位置，按n可以查找下一个  |  :/you
实时查看文件  |  tail -f test.txt
zip压缩文件  |  zip -r aaa.zip aaa
zip解压文件  |  unzip aaa.zip


centos 6 与 centos 7 与 ubuntu 区别
=====================================

操作  |  centos 6  |  centos 7  |  ubuntu  |
-------------      | -------------           |-------------------         | ---------------|
开启防火墙          |  service iptables start |  systemctl start firewalld |  sudo ufw enable && sudo ufw default deny |
关闭防火墙          |  service iptables stop  |  systemctl stop firewalld  |  sudo ufw disable  |
关闭防火墙自启动     |  chkconfig iptables off |  systemctl disable firewalld |   |
mysql开启自启动     | ~~cp /usr/local/mysql/support-files/mysql.server /etc/init.d/mysqld <br> chmod +x /etc/init.d/mysqld  <br>   chkconfig --add mysql  <br>~~ rpm安装直接 chkconfig mysqld on     | systemctl enable mysqld && systemctl daemon-reload | dpkg安装默认自启动 |
在线安装方式        |  yum install -y 软件名     |  yum install -y 软件名   |  sudo apt-get install 软件名  |
离线安装方式        |  rpm -ivh \*\*\*.rpm      |  rpm -ivh \*\*\*.rpm      |  sudo dpkg -i \*\*\*.deb  |  
查询是否安装        |  rpm -qa \| grep mysql     |  rpm -qa \| grep mysql     |  dpkg -l \| grep mysql   |


Linux上MySQL无法创建进程
===============================

`sudo mkdir /var/run/mysqld`

`sudo chown -R mysql:mysql /var/run/mysqld`

`sudo service mysqld start`
