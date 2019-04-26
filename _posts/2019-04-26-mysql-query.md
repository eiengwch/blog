---
layout: post
title: Mysql 常用的关系查询
categories: Mysql
description: 常用的关系查询，子查询，连接查询等
keywords: Mysql
---


### Mysql 子查询

1. 示例-单表子查询

```mysql
SELECT id,uid,family_id,base_pay,cash_base,is_valid
FROM stat_live_family_member_daily WHERE uid in (
	SELECT uid FROM stat_live_family_member_daily WHERE dtime >= '2016-11-01'GROUP BY uid HAVING SUM(is_valid) >= 13)
AND dtime >= '2016-11-01'
AND base_pay >= 1
ORDER BY uid  
```

2. 示例-多表子查询

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
