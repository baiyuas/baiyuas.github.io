---
title: Android 开发之GreenDao
date: 2018-09-19 19:50:38
categories: Android
tags: 
    - GreenDao
---

介绍GreenDao 升级和事务
<!-- more -->

## 关于事务

在Greendao中可以使用事务来处理一些sql的执行，一开始我是这样使用的
	
	db.beginTransaction();
	。。。。
	db.endTransaction();

就这样把我搞死了，在初始化时候onCreate初始化的数据总是无法创建，最后找到还缺少一个步骤**db.setTransactionSuccessful();**
正常使用如下：

	db.beginTransaction();
	try {
		
		db.setTransactionSuccessful();
	} catch (Exception e) {

	} finally {
		db.endTransaction();
	}
开始事务，正常执行，然后提交。如果异常则回滚

## 关于升级保留数据

这里需要自己继承**DaoMaster.OpenHelper**
然后重写 
**onCreate(Database db)**
**onUpgrade(Database db, int oldVersion, int newVersion)**
者两个方法，根据自己需求操作。

如果保留数据，基本思路就是

1. 创建临时表
2. 删除旧表
3. 创建新表
4. 根据需求将临时表中数据更新到新表中

然后在初始化GreenD奥使用使用自己定义的Helper
 
	MyOpenHelper openHelper = new MyOpenHelper(context, "xx.db", null);
    Database db = openHelper.getEncryptedWritableDb("cc");
    DaoMaster daoMaster = new DaoMaster(db);

## 关于更新表

默认如果表结构发生变化，在升级时候会先删除所有表，然后在创建，但是这个触发必须修改在build.gradle中配置的**schemaVersion**

        greendao {
            schemaVersion 1 
			daoPackage 'greendao编译生成的包的位置' 一般自己指定一下，要不自动生成在bean所在包下代码显得混乱，而且不建议提交到SVN，不利于协同工作
            targetGenDir 'src/main/java'
        }