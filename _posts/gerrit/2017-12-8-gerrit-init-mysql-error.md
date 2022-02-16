---
layout: post
title: "Centos6.8 install gerrit Mysql error"
subtitle:   "Caused by: com.mysql.jdbc.exceptions.jdbc4.MySQLSyntaxErrorException: Invalid default value for 'added_on'"
date:       2017-12-8 0:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-05.jpg"
---
 5.7.20 MySQL Community Server
 
~~~bash
Exception in thread "main" com.google.gwtorm.server.OrmException: Cannot apply SQL
CREATE TABLE account_group_by_id_aud (
added_by INT DEFAULT 0 NOT NULL,
removed_by INT,
removed_on TIMESTAMP NULL DEFAULT NULL,
group_id INT DEFAULT 0 NOT NULL,
include_uuid VARCHAR(255) BINARY DEFAULT '' NOT NULL,
added_on TIMESTAMP NOT NULL
,PRIMARY KEY(group_id,include_uuid,added_on)
)
    at com.google.gwtorm.jdbc.JdbcExecutor.execute(JdbcExecutor.java:44)
    at com.google.gwtorm.jdbc.JdbcSchema.createRelations(JdbcSchema.java:119)
    at com.google.gwtorm.jdbc.JdbcSchema.updateSchema(JdbcSchema.java:89)
    at com.google.gerrit.server.schema.SchemaCreator.create(SchemaCreator.java:77)
    at com.google.gerrit.server.schema.SchemaUpdater.update(SchemaUpdater.java:99)
    at com.google.gerrit.pgm.init.BaseInit$SiteRun.upgradeSchema(BaseInit.java:341)
    at com.google.gerrit.pgm.init.BaseInit.run(BaseInit.java:121)
    at com.google.gerrit.pgm.util.AbstractProgram.main(AbstractProgram.java:64)
    at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
    at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
    at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
    at java.lang.reflect.Method.invoke(Method.java:498)
    at com.google.gerrit.launcher.GerritLauncher.invokeProgram(GerritLauncher.java:166)
    at com.google.gerrit.launcher.GerritLauncher.mainImpl(GerritLauncher.java:93)
    at com.google.gerrit.launcher.GerritLauncher.main(GerritLauncher.java:50)
    at Main.main(Main.java:25)
Caused by: com.mysql.jdbc.exceptions.jdbc4.MySQLSyntaxErrorException: Invalid default value for 'added_on'
    at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
    at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:62)
    at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
    at java.lang.reflect.Constructor.newInstance(Constructor.java:423)
    at com.mysql.jdbc.Util.handleNewInstance(Util.java:411)
    at com.mysql.jdbc.Util.getInstance(Util.java:386)
    at com.mysql.jdbc.SQLError.createSQLException(SQLError.java:1053)
    at com.mysql.jdbc.MysqlIO.checkErrorPacket(MysqlIO.java:4074)
    at com.mysql.jdbc.MysqlIO.checkErrorPacket(MysqlIO.java:4006)
    at com.mysql.jdbc.MysqlIO.sendCommand(MysqlIO.java:2468)
    at com.mysql.jdbc.MysqlIO.sqlQueryDirect(MysqlIO.java:2629)
    at com.mysql.jdbc.ConnectionImpl.execSQL(ConnectionImpl.java:2713)
    at com.mysql.jdbc.ConnectionImpl.execSQL(ConnectionImpl.java:2663)
    at com.mysql.jdbc.StatementImpl.execute(StatementImpl.java:888)
    at com.mysql.jdbc.StatementImpl.execute(StatementImpl.java:730)
    at com.google.gwtorm.jdbc.JdbcExecutor.execute(JdbcExecutor.java:42)
    ... 15 more
 ~~~
 
[该问题解决方案](http://www.jianshu.com/p/154960f71d11)<br>
使用MySQL root用户登录 设置<br>
set global explicit_defaults_for_timestamp=1; 重新安装gerrit即可 新版本的mysql timestamp默认值的问题需要手动配置一下
