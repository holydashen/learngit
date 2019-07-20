# centos7 下mysql5.7修改默认编码格式为UTF-8



**方法步骤：**

**1、用SSH登录mysql，使用命令查看mysql的默认编码。**

​    **命令：show variables like 'character_set_%';**

**2、进入配置文件修改配置内容，执行命令：vi /etc/my.cnf**

**3、修改配置文件的内容，在[mysqld]结束位置添加：character_set_server=utf8**

**4、重新启动mysql服务。**

​    **停止命令：systemctl stop mysqld.service**

​    **启动命令：systemctl start mysqld.service**