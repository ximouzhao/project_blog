---
title: 安装zip格式的mysql_5.7
date: 2017-11-04 12:19:32
tags: mysql
---
一、到官网下载zip文件。
二、解压到目录，并配置path环境变量，如D:\common\mysql\mysql-5.7.20-winx64\bin
三、在mysql目录新建或修改my.ini
添加以下内容
<pre>
[mysqld] 
basedir=D:\common\mysql\mysql-5.7.20-winx64
datadir=D:\common\mysql\mysql-5.7.20-winx64\data
port=3306
max_connections=200
character-set-server=utf8
default-storage-engine=innodb
[mysql]
default-character-set=utf8
</pre>

四、在mysql文件下新建对应的datadir，比如在mysql-5.7.20-winx64下新建data文件夹
五、执行
```shell
mysqld -install
mysqld --initialize /*一定要执行这个命令*/
```
安装并初始化mysql
六、执行
``net start mysql``
启动mysql

七、windows环境下修改root用户密码
在mysql_5.6以后，root密码默认不为空，是一个随机的密码，没找到这个密码保存的地方，所以安装好之后我们需要重置一下mysql的root用户密码
1、运行``net stop mysql``停止mysql服务
2、运行``mysqld --skip-grant-tables``启动mysqld 服务，注意是mysqld不是mysql
3、运行``mysql -u root ``登录到mysql
4、运行``update mysql.user set authentication_string=password('123qwe') where user='root' and Host = 'localhost';``新版mysql数据库下已经没有password字段了
5、运行``flush privileges;``重新装载权限
6、运行``quit;``退出mysql
7、在任务管理器中杀掉mysqld进程
8、运行``net start mysql``正常启动mysql服务
9、运行``mysql -u root -p ``输入刚才修改的密码登陆到mysql
10、系统会提示需要修改密码，这时候再执行``set password for root@localhost = password('123qwe');``即可



八、错误处理

执行net start mysql 提示mysql启动失败，没有输出任何的错误
是因为我们没有执行mysqld --initialize命令就启动服务导致。
我们只需要清空data目录，重新执行mysqld --initialize命令即可

