---
layout: post
title: Mysql 常用的查询
categories: Mysql
description: 常用的关系查询，子查询，连接查询 去重复查询等
keywords: Mysql
---


### Mysql 子查询

### 示例-关系单表子查询

```mysql
SELECT id,uid,family_id,base_pay,cash_base,is_valid
FROM stat_live_family_member_daily WHERE uid in (
	SELECT uid FROM stat_live_family_member_daily WHERE dtime >= '2016-11-01'GROUP BY uid HAVING SUM(is_valid) >= 13)
AND dtime >= '2016-11-01'
AND base_pay >= 1
ORDER BY uid  
```

### 示例-关系多表子查询

```mysql
SELECT * FROM
    (SELECT * FROM `cj_glamour_record` WHERE `uid` = 14368320 AND `type` = 100  UNION
            (SELECT * FROM `cj_glamour_record_like` WHERE `uid` = 14368320 AND `type` = 100)
        UNION
            (SELECT * FROM `cj_glamour_record_live` WHERE `uid` = 14368320 AND `type` = 100 )
    ) tmp
ORDER BY create_time DESC,id DESC
LIMIT 0,50 
```

### 示例-三张表连接查询

```mysql
SELECT g.`name`, o.goods_id, SUM(fee) FROM
cj_order as o
	LEFT JOIN chujiandw.cj_goods as g  on o.goods_id = g.id
WHERE
	o.pay_time > UNIX_TIMESTAMP('2016-12-5 00:00:00')
and o.state=2
GROUP BY o.goods_id
```

### 示例-多张表连接查询

```mysql
SELECT
    sum(diamond) AS sum_diamond
FROM
    chujiandw.cj_user_base  AS t 
LEFT JOIN chujiandw.cj_account_base AS l ON t.uid = l.uid
LEFT JOIN chujiandw.cj_location_base AS a ON l.uid = a.uid
WHERE
a.update_time > 1472206345
AND t.type = 0
```

### 示例-去重复 子查询 时间戳转换日期查询

```mysql
SELECT
id,uid,family_id,FROM_UNIXTIME(create_time, '%Y-%m-%d'),attrs,delete_time
from 
cj_live_family_users 
WHERE
family_id  in (SELECT id from cj_live_family WHERE delete_time =0)
and  uid in (SELECT uid from cj_live_family_users  GROUP BY uid having  count(uid) > 1)
and (delete_time=0 or delete_time > UNIX_TIMESTAMP(NOW()))
and (cancel_time=0 or cancel_time > UNIX_TIMESTAMP(NOW()))
ORDER BY create_time desc  
```

### 示例-子查询 当前时间戳转换

```mysql
SELECT count(id) as sum_id
from  cj_live_family_users WHERE
family_id in (SELECT id from cj_live_family WHERE  delete_time=0)
and (delete_time=0 or delete_time=0 > UNIX_TIMESTAMP(NOW()))
and (cancel_time=0 or cancel_time > UNIX_TIMESTAMP(NOW()))
select count( id) as ids from  cj_live_family_users WHERE
family_id in (SELECT id FROM cj_live_family WHERE delete_time=0)
and cancel_time  < UNIX_TIMESTAMP(NOW()) 
and cancel_time >0 
```

### 示例-日期转换时间戳查询

```mysql
SELECT `oid`,sum(glamour_take) AS `glamour_take` FROM `cj_live_gift_record`
WHERE `oid` IN (14841990)
AND `create_time` >= UNIX_TIMESTAMP('2016-11-11')
AND create_time <= UNIX_TIMESTAMP('2016-11-11 23:59:59')
```

### 示例-对多个字段，每个字段加和统计

```mysql
SELECT
    `family_id`,
    `elder_cash_rate`,
    `elder_cash_glamour`,
    max(effective_anchor) AS c_effective_anchor,
    max(count_sign) AS c_anchor,
    sum(income_glamour) AS c_glamour,
    sum(income_glamour_pay) AS c_glamour_pay,
    sum(count_base_pay) AS c_base_pays,
    sum(family_pay) AS c_pay
FROM
    `stat_live_family_count_daily`
WHERE
    `dtime` BETWEEN '2016-11-01'
AND '2016-11-15'
GROUP BY
    family_id
ORDER BY
    c_glamour DESC
```

### 示例-根据一个字段分组统计另一个字段数据相加

```mysql
SELECT uid,COUNT(id) as cnt FROM stat_live_family_member_daily
WHERE is_valid = 1
GROUP BY uid
ORDER BY cnt DESC

```

### 示例-查询重复数据

```mysql
 select * from cj_live_family_users where uid in (
 	select uid from cj_live_family_users group by uid having count(0) > 1
 	)
```

### 示例-别名连接查询

```mysql
SELECT
    a.uid,b.update_time,a.nickname,a.birthday,a.album,a.type
FROM
    cj_location_base AS b,
    cj_user_base AS a
WHERE
    a.uid = b.uid
AND a.type = 0
AND a.birthday > '1970-01-01' 
AND a.album <> ''
AND b.update_time < 1451581200 
ORDER BY uid ASC
LIMIT 1, 1000
```
