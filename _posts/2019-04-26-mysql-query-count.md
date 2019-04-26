---
layout: post
title: Mysql 常用的查询统计
categories: Mysql
description: 常用的查询统计 求和 分组 去重等
keywords: Mysql
---


### Mysql 统计查询

### 示例-统计条数

```mysql
SELECT
COUNT(*) as count_id
FROM
stat_live_family_member_daily
WHERE dtime = '2016-12-5'
AND family_id = 11
```

### 示例-去除重复统计

```mysql
SELECT
COUNT(DISTINCT uid) as count_id
FROM
stat_live_family_member_daily
WHERE dtime = '2016-12-5'
```

### 示例-分组统计

```mysql
SELECT
dtime,COUNT(DISTINCT uid) as count
FROM
stat_live_family_member_daily
WHERE dtime >= '2016-11-16'
AND dtime < '2016-12-1'
AND family_id = 11
GROUP BY dtime
ORDER BY dtime ASC
```

### 示例-表达式计算统计

```mysql
SELECT
SUM(income_glamour) as sum_inc,
COUNT(IF(is_valid=1,true,null)) as is_1,
COUNT(IF(is_valid=0,true,null)) as is_0
FROM
stat_live_family_member_daily
WHERE dtime >= '2016-11-16'
AND dtime < '2016-12-1'
ORDER BY dtime ASC
```

### 示例-分组计算统计

```mysql
SELECT
dtime,SUM(income_glamour) as sum_inc,
COUNT(IF(is_valid=1,true,null)) as is_1,
COUNT(IF(is_valid=0,true,null)) as is_0
FROM
stat_live_family_member_daily
WHERE dtime >= '2016-11-16'
AND dtime < '2016-12-1'
GROUP BY dtime
ORDER BY dtime ASC 
```
### 示例-计算一个字段之和

```mysql
SELECT
SUM(fee) as sum_fee
FROM
cj_order
WHERE state=2
AND pay_time >= UNIX_TIMESTAMP('2016-12-6')
ORDER BY fee DESC
```
### 示例-计算多个字段数据之和

```mysql
SELECT
SUM(fee) as sum_fee, SUM(multiple) as multiple_sum
FROM
cj_order
WHERE state=2
AND pay_time >= UNIX_TIMESTAMP('2016-12-6')
ORDER BY fee DESC
```


### 示例-对计算出来的结果添加当天的时间

```mysql
SELECT
SUM(fee) as sum_fee, SUM(multiple) as multiple_sum, NOW() as the_day
FROM
cj_order
WHERE state=2
AND pay_time >= UNIX_TIMESTAMP('2016-12-6')
ORDER BY fee DESC
```

### 示例-关联分组 关联表 统计查询

```mysql
SELECT
 `name`,goods_id,SUM(fee) as sum_fee
FROM
cj_order as o
LEFT JOIN chujiandw.cj_goods as g on o.goods_id = g.id
WHERE state=2
AND pay_time >= UNIX_TIMESTAMP('2016-12-6')
GROUP BY goods_id
ORDER BY sum_fee DESC

```

### 示例-表达式计算求和

```mysql
SELECT
dtime,SUM(income_glamour) as income_sum,
            SUM(income_glamour/100) as money,
            SUM(followers) as sum_foo, 
            SUM(share_num) as sum_shaer,
            SUM(followers+share_num) as sum_all
FROM
stat_live_family_member_daily
WHERE dtime >= '2016-11-16'
AND dtime < '2016-12-1'
AND family_id = 11
GROUP BY dtime
ORDER BY dtime ASC

```
