# [CentOS7 安装mysql（YUM方式）](https://www.cnblogs.com/caoxb/p/9405323.html)

 **1.下载mysql源安装包**

shell> wget <http://dev.mysql.com/get/mysql57-community-release-el7-8.noarch.rpm>

 

**2.安装mysql源**

shell> yum localinstall mysql57-community-release-el7-8.noarch.rpm 

 

**3.检查mysql源是否安装成功**

shell> yum repolist enabled | grep "mysql.*-community.*"

 

**4.修改 vim /etc/yum.repos.d/mysql-community.repo源** ，改变默认安装的mysql版本。比如要安装5.6版本，将5.7源的enabled=1改成enabled=0。然后再将5.6源的enabled=0改成enabled=1即可。

![img](https://images2018.cnblogs.com/blog/1443749/201808/1443749-20180802095051183-2053425212.png)

 

**5.安装MySQL**

shell> yum install mysql-community-server

 

**6.启动MySQL服务**

shell> systemctl start mysqld

 

**7.开机启动**

shell> systemctl enable mysqld

shell> systemctl daemon-reload

 

**8.修改root本地登录密码**

 1）查看mysql密码

shell> grep 'temporary password' /var/log/mysqld.log

![img](https://images2018.cnblogs.com/blog/1443749/201808/1443749-20180802094841902-1212339549.png)

2）连接mysql

shell> mysql -uroot -p

3）修改密码[注意：后面的分号一定要跟上]

mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass4!';

或者：

mysql> set password for 'root'@'localhost'=password('MyNewPass4!'); 

mysql> show variables like '%password%';



**9.添加远程登录用户**

mysql> GRANT ALL PRIVILEGES ON *.* TO 'caoxiaobo'@'%' IDENTIFIED BY 'Caoxiaobo0917!' WITH GRANT OPTION;