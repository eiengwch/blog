---
layout: post
title: Mysql 命令行导出数据库
categories: Mysql
description: 命令行下面导出数据库和表
keywords: Mysql
---

## 导出整个数据库-包括表中的数据

```sh
mysqldump -u db_username -p db_name > db_name.sql    
```

## 导出数据库中的某张表-包含数据

```sh
mysqldump -u db_username -p db_name table_name > table_name.sql     
```

## 导出数据库结构-不含数据

```sh
mysqldump -u db_username -p -d db_name > db_name.sql        
```

## 导出数据库中的某张数据表的表结构-不含数据

```sh
mysqldump -u db_username -p -d db_name table_name > table_name.sql         
```


1. 示例

```sh
mysqldump -uroot -p    test_database   > /Users/eiengwch/nmae.sql
//mysqldump           导出数据库命令
// -uroot       	  登录该数据库 用户名
// -p 		    	  登录该数据库参数
// test_databas 	  要导出到数据库名
// /Users/eiengwch/   导出到数据库文件存储路径
//  bb.sql    		  导出到数据库文件名
```

