1、下载http://dev.mysql.com/downloads/mysql/；

2、Community > MySQL Community Server；

3、Other Downloads: > Windows (x86, 32-bit), ZIP Archive；

4、解压mysql-8.0.15-winx64.zip，解压在 D: oolsmysql-8.0.15-winx64；

5、在D: oolsmysql-8.0.15-winx64目录下新建my.ini 文件，设置代码如下：

[mysql]

# 设置mysql客户端默认字符集

default-character-set=utf8

[mysqld]

#skip-grant-tables

#设置3306端口

port = 3306

# 设置mysql的安装目录

basedir=D:\tools\mysql-8.0.15-winx64

# 设置mysql数据库的数据的存放目录

datadir=D:\tools\mysql-8.0.15-winx64\sqldata

# 允许最大连接数

max_connections=200

# 服务端使用的字符集默认为8比特编码的latin1字符集

character-set-server=utf8

# 创建新表时将使用的默认存储引擎

default-storage-engine=INNODB

6、安装mysql服务，以管理员身份执行cmd；

cd到D: oolsmysql-8.0.15-winx64in目录，然后执行mysqld install,然后会出现成功返回值细腻些；

7、启动服务 net start MYSQL(大写) 本地计算机上的服务启动；

如果执行 net start mysql 如下报错：

D: oolsmysql-8.0.15-winx64in>net start MYSQL

MySQL 服务正在启动 .

MySQL 服务无法启动。

服务没有报告任何错误。

请键入 NET HELPMSG 3534 以获得更多的帮助。

需要执行下：mysqld --initialize

8、设置环境变量：D: oolsmysql-8.0.15-winx64in（不做详细说明）;

9、然后输入：mysql -u root -p ，会出现：ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: YES)

坑一出现：

mysql5.6之前可以通过在mysql.ini里设置跳过密码即可：skip-grant-tables，但是5.7之后密码是在sqldata/**.err的文件中。

2019-02-26T06:21:23.767449Z 5 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: KwwQsL,),0ug





找到密码之后，通过mysql -uroot -pKwwQsL,),0ug 进入。

坑二出现：

执行use mysql命令，报错：ERROR 1820 (HY000): You must reset your password using ALTER USER statement before executing this statement.

如果你想要设置一个简单的测试密码的话，比如设置为123456，会提示这个错误，报错的意思就是你的密码不符合要求。

然后执行命令更改密码：alter user 'root'@'localhost' identified by 'Pass1234';

然后就可以了。

坑三出现：

用客户端连接本地连接，报错：navicat for mysql 链接时报错：1251-Client does not support authentication protocol requested by server

主要原因是mysql服务器要求的认证插件版本与客户端不一致造成的。

打开mysql命令行输入如下命令查看，系统用户对应的认证插件：





可以看到root用户使用的plugin是caching_sha2_password，mysql官方网站有如下说明：





意思是说caching_sha2_password是8.0默认的认证插件，必须使用支持此插件的客户端版本。

解决方法是将root的plugin改成mysql_native_password。相当于降了一级。

ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'Pass1234';

然后就成功了。

