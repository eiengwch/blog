---
layout: post
title: app 测试文章
categories: html
description: 测试文章
keywords: test
---

测试文章测试文章测试文章测试文章测试文章测试文章测试文章测试文章测试文章测试文章测试文章测试文章测试文章测试文章测试文章测试文章测试文章测试文章


---
```

<?php

    public function getUserInviteData()
    {
        $uid = $this->uid;
        if (empty($uid)) {
            return server_json([], false, '用户权限检验失败！请重新登录！');
        }
        $invite_array = UserService::getInviteUidArray($uid);
        if (!empty($invite_array)) {
            return server_json($invite_array);
        }
        return server_json([], false, 'data is empty');
    }

?>

```