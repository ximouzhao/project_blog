---
title: 配置linux生产环境
date: 2018-03-11 15:25:59
tags:
---
查看机器版本
```sh
cat /proc/version

cat /etc/redhat-release，这种方法只适合Redhat系的Linux：

[root@S-CentOS home]# cat /etc/redhat-release
CentOS release 6.5 (Final)
```
```sh
free -m //以m为单位显示内存使用情况
fdisk -l //
```
jdk7linux下载地址http://www.pc6.com/softview/SoftView_447834.html
http://7dx.pc6.com/wwb5/jdk7u79linuxx64.tar.gz
官网https://tomcat.apache.org/download-80.cgi
tomcat最终下载地址http://mirrors.hust.edu.cn/apache/tomcat/tomcat-8/v8.0.50/bin/apache-tomcat-8.0.50.tar.gz
命令行浏览器yum  install   links


linuxoracle 下载地址http://download.oracle.com/otn/linux/oracle11g/R2/linux.x64_11gR2_database_1of2.zip?AuthParam=1520773212_487351b546f61d1bfaa7bf6a069ced87
http://download.oracle.com/otn/linux/oracle11g/R2/linux.x64_11gR2_database_2of2.zip?AuthParam=1520773205_1f47cb0a74d8af2b4c33e7debfdabad3
这里有授权


compat-libcap1-1.10-3.el7.x86_64 
记住yum install compat-libcap1


top命令
进程信息区
统计信息区域的下方显示了各个进程的详细信息。首先来认识一下各列的含义。

序号	列名	含义
a	PID	进程id
b	PPID	父进程id
c	RUSER	Real user name
d	UID	进程所有者的用户id
e	USER	进程所有者的用户名
f	GROUP	进程所有者的组名
g	TTY	启动进程的终端名。不是从终端启动的进程则显示为 ?
h	PR	优先级
i	NI	nice值。负值表示高优先级，正值表示低优先级
j	P	最后使用的CPU，仅在多CPU环境下有意义
k	%CPU	上次更新到现在的CPU时间占用百分比
l	TIME	进程使用的CPU时间总计，单位秒
m	TIME+	进程使用的CPU时间总计，单位1/100秒
n	%MEM	进程使用的物理内存百分比
o	VIRT	进程使用的虚拟内存总量，单位kb。VIRT=SWAP+RES
p	SWAP	进程使用的虚拟内存中，被换出的大小，单位kb。
q	RES	进程使用的、未被换出的物理内存大小，单位kb。RES=CODE+DATA
r	CODE	可执行代码占用的物理内存大小，单位kb
s	DATA	可执行代码以外的部分(数据段+栈)占用的物理内存大小，单位kb
t	SHR	共享内存大小，单位kb
u	nFLT	页面错误次数
v	nDRT	最后一次写入到现在，被修改过的页面数。
w	S	进程状态。
D=不可中断的睡眠状态
R=运行
S=睡眠
T=跟踪/停止
Z=僵尸进程
x	COMMAND	命令名/命令行
y	WCHAN	若该进程在睡眠，则显示睡眠中的系统函数名
z	Flags	任务标志，参考 sched.h
默认情况下仅显示比较重要的 PID、USER、PR、NI、VIRT、RES、SHR、S、%CPU、%MEM、TIME+、COMMAND 列。可以通过下面的快捷键来更改显示内容。


dbca -silent -createDatabase -templateName General_Purpose.dbc -gdbName orcl -sysPassword oracle -systemPassword oracle
可以使用

注意排版
遭遇TNS-01150: The address of the specified listener name is incorrect
```bash
SID_LIST_LISTENER = 
  (SID_LIST = 
    (SID_DESC =
      (SID_NAME = PLSExtProc) 
      (ORACLE_HOME = /oracle/product/10.2.0/db_1) 
      (PROGRAM = extproc)
    ) 
    (SID_DESC =
      (GLOBAL_DBNAME = orcl)
      (ORACLE_HOME = /oracle/product/10.2.0/db_1) 
      (SID_NAME = orcl)
    ) 
  ) 
LISTENER = 
  (DESCRIPTION_LIST = 
    (DESCRIPTION =
      (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1)) 
      (ADDRESS = (PROTOCOL = TCP)(HOST = localhost)(PORT = 1521)) 
    )
  )
```

SQL>START file_name
or
SQL>@ file_name

 

1 .sqlplus system/system@srv 
2. sql>@c:\a.sql  (执行此命令前将a.sql  拷到C盘根目录)

sqlplus system/system@srv @a.sql
或者登录sqlplus后执行@a.sql

那怎么开启一个端口呢
添加
firewall-cmd --zone=public --add-port=80/tcp --permanent    （--permanent永久生效，没有此参数重启后失效）
重新载入
firewall-cmd --reload
查看
firewall-cmd --zone= public --query-port=80/tcp
删除
firewall-cmd --zone= public --remove-port=80/tcp --permanent



按以下次序依次安装
rpm -ivh mysql-community-common-5.7.12-1.el6.x86_64.rpm
rpm -ivh mysql-community-libs-5.7.12-1.el6.x86_64.rpm
rpm -ivh mysql-community-devel-5.7.12-1.el6.x86_64.rpm
rpm -ivh mysql-community-client-5.7.12-1.el6.x86_64.rpm
rpm -ivh mysql-community-server-5.7.12-1.el6.x86_64.rpm


systemctl start mysqld

低版本redhat需要解决：
yum install perl
yum install net-tools
卸载依赖rpm -e mariadb-libs-1:5.5.56-2.el7.x86_64 --nodeps

/var/lib/mysql

#vim /etc/my.cnf(注：windows下修改的是my.ini)


///set password=password("qazplm!@#");//不可用 ERROR 1290 (HY000): The MySQL server is running with the --skip-grant-tables option so it cannot exe
cute this statement

#vim /etc/my.cnf(注：windows下修改的是my.ini)

在文档内搜索mysqld定位到[mysqld]文本段：
/mysqld(在vim编辑状态下直接输入该命令可搜索文本内容)

在[mysqld]后面任意一行添加“skip-grant-tables”用来跳过密码验证的过程，如下图所示：
mysql -uroot –p（密码）
use mysql;
5.7 正确更新密码：update mysql.user set authentication_string=password('mimeTGBmima1@#') where user='root';
CREATE USER 'username'@'host' IDENTIFIED BY 'password';
```sql
CREATE USER 'dog'@'localhost' IDENTIFIED BY '123456';
CREATE USER 'pig'@'192.168.1.101_' IDENDIFIED BY '123456';
CREATE USER 'pig'@'%' IDENTIFIED BY '123456';
CREATE USER 'pig'@'%' IDENTIFIED BY '';
CREATE USER 'pig'@'%';
CREATE USER 'zhaoxmf'@'%' IDENTIFIED BY 'mimeTGBmima1@#';
以下报错解决方法
ERROR 1819 (HY000): Your password does not satisfy the current policy requirement.
alter  user 'root'@'localhost' identified by '#20as3SElksds0ew98';
alter  user 'root'@'localhost' identified by 'mimeTGBmima1@#';
```
zhaoxmf mimeTGBmima1@#  umpay

grant all privileges on *.* to 'zhaoxmf'@'%' with grant option;


mysql>GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION
//使修改生效
mysql>FLUSH PRIVILEGES
//退出MySQL服务器
mysql>EXIT


mysql的view不能有子查询语句

linux查看系统编码和修改系统编码的方法
# locale
LANG=en_US.UTF-8
LC_CTYPE="en_US.UTF-8"
LC_NUMERIC="en_US.UTF-8"
LC_TIME="en_US.UTF-8"
LC_COLLATE="en_US.UTF-8"
LC_MONETARY="en_US.UTF-8"
LC_MESSAGES="en_US.UTF-8"
LC_PAPER="en_US.UTF-8"
LC_NAME="en_US.UTF-8"
LC_ADDRESS="en_US.UTF-8"
LC_TELEPHONE="en_US.UTF-8"
LC_MEASUREMENT="en_US.UTF-8"
LC_IDENTIFICATION="en_US.UTF-8"

CREATE DATABASE 库名;
use database;


vi /etc/profile
export JAVA_HOME=/usr/local/jdk1.7.0_71

export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar

export PATH=$JAVA_HOME/bin:$PATH
source /etc/profile
输入 java -version 然后会显示jdk的版本信息等
