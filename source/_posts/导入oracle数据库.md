---
title: 导入oracle数据库
date: 2017-11-21 23:50:59
tags:
---
忘记数据库连接用户名处理如下
```
cmd中敲入命令sqlplus
输入用户名/as sysdba,以管理员身份连接到oracle；
查看用户名
select username from dba_users 
```
创建数据库用户命令如下：
``` sql
CREATE USER nc65demo IDENTIFIED BY 1 DEFAULT TABLESPACE NNC_DATA01 TEMPORARY TABLESPACE temp;
```
授予管理员权限
```sql
GRANT connect,dba to nc65demo
```
普通导入方式
```sh
imp v65demo/1@orcl fromuser=v65demo touser=v65demo file=v65demo20160111.dmp buffer=100000000 log=I:\v65demo20160111\log.log
```
数据泵导入方式：
1、查询路径信息
查看已有的路径信息
```sql
SELECT * FROM dba_directories;
```
2、创建路径
创建路径需要sys权限，需要有create any directory权限才可以创建路径。
　　选项：DIRECTORY=directory_object
　　Directory_object用于指定目录对象名称。需要注意，目录对象是使用CREATE DIRECTORY语句建立的对象，而不是OS目录。
```sql
eg: CREATE OR REPLACEdirectory backup_path AS 'D:\APP\ORADATA\db_backup'; --创建路径名为dackup_path的路径，并指向硬盘的指定位置
```
对新创建的路径进行授权操作：
```sql
eg：grant read,write on directory backup_path to orcldev; --将对路径的读写权限分配各orcldev用户。
```
3.执行导入命令
```sh
impdp GJNC65/1@orcl2 DIRECTORY=backup_path dumpfile=GJNC65_20171009_2.DMP remap_schema=GJNC65:GJNC65  EXCLUDE=TABLE:\"IN ('SM_BUSILOG_DEFAULT')\"
```
这个命令排除了日志表SM_BUSILOG_DEFAULT
其定义如下,导入完成后手动执行即可
```sql
create table SM_BUSILOG_DEFAULT
(
  busiobjcode    VARCHAR2(200),
  busiobjname    VARCHAR2(300),
  client         VARCHAR2(50),
  dr             NUMBER(10) default 0,
  logdate        CHAR(19),
  logmsg         BLOB,
  operateresult  VARCHAR2(50),
  orgpk_busiobj  VARCHAR2(36),
  pk_busilog     CHAR(20) not null,
  pk_busiobj     VARCHAR2(36),
  pk_group       VARCHAR2(36),
  pk_operation   VARCHAR2(36),
  pk_user        VARCHAR2(36),
  ts             CHAR(19) default to_char(sysdate,'yyyy-mm-dd hh24:mi:ss'),
  typepk_busiobj VARCHAR2(36)
)
tablespace NNC_DATA01
  pctfree 10
  initrans 1
  maxtrans 255
  storage
  (
    initial 55552K
    next 256K
    minextents 1
    maxextents unlimited
  );
create index INDEX_DEFAULTLOG_TYPEPKBUSIOBJ on SM_BUSILOG_DEFAULT (TYPEPK_BUSIOBJ)
  tablespace NNC_INDEX01
  pctfree 10
  initrans 2
  maxtrans 255
  storage
  (
    initial 6M
    next 128K
    minextents 1
    maxextents unlimited
  );
alter table SM_BUSILOG_DEFAULT
  add constraint PK_BUSILOG_DEFAULT primary key (PK_BUSILOG)
  using index 
  tablespace NNC_INDEX01
  pctfree 10
  initrans 2
  maxtrans 255
  storage
  (
    initial 4M
    next 128K
    minextents 1
    maxextents unlimited
  );
```