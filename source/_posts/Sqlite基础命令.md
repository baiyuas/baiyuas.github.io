
# Sqlite 基础命令

## 打开数据

	$ sqlite3  xxx.db

## 查看数据库表

	sqlite> .table

## 导出sql文件

	sqlite> .open xx.db
	sqlite> .output xxx.sql
	sqlite> .dump （table-name）
	sqlite> .exit

在当前目录下就可以查看导出的sql文件了

或者
	$ sqlite3 xx.db .dump > xxx.sql

逆向
	$ sqlite3 xx.db < xxx.sql

## 插入数据

	INSERT INTO BIS_FLOOR VALUES(5,'娱乐精选','业务办理','home_sjyw','hbyd://ywbl2',9,1,0,0,'',1);

## 更新数据

	UPDATE TABLE BIS_FLOOR SET XXX='XXX' WHERE ID = 5;
	
## 查看表结构

	SELECT × FROM SQLITE_MASTER WHERE TUPE='TABLE' AND NAME='XXXTABLE';


