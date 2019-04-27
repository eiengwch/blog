---
layout: post
title: Linux Ubuntu安装Crontab工具
categories: Linux
description: linux 环境下安装crontab工具
keywords: linux crontab
---


### Crontab 安装

查看是否已经安装crontab

```sh
root@e:~# crontab -l 
```

若没有安装，用如下命令安装

```sh
root@e:~# apt-get  install vixie-cron
root@e:~# apt-get  install crontabs
```

查看是否已安装完成

```sh
root@e:~# crontab -e
root@e:~# crontab -l
```

修改编辑工具 nano 为 vim 命令如下

```sh
root@e:~# sudo select-editor
Select an editor.  To change later, run 'select-editor'.
  1. /bin/ed
  2. /bin/nano        <---- easiest
  3. /usr/bin/vim.basic
  4. /usr/bin/vim.tiny


Choose 1-4 [2]: 4
```

添加crontab任务

```sh
root@e:~# crontab -e 
```

添加crontab 任务 编辑这个文件 例如我添加将当前 日期每个一分钟写入到log txt 文件中

```sh
# Edit this file to introduce tasks to be run by cron.
# 
# Each task to run has to be defined through a single line
# indicating with different fields when the task will be run
# and what command to run for the task
# 
# To define the time you can provide concrete values for
# minute (m), hour (h), day of month (dom), month (mon),
# and day of week (dow) or use '*' in these fields (for 'any').# 
# Notice that tasks will be started based on the cron's system
# daemon's notion of time and timezones.
# 
# Output of the crontab jobs (including errors) is sent through
# email to the user the crontab file belongs to (unless redirected).
# 
# For example, you can run a backup of all your user accounts
# at 5 a.m every week with:
# 0 5 * * 1 tar -zcf /var/backups/home.tgz /home/
# 
# For more information see the manual pages of crontab(5) and cron(8)
# 


*/1 * * * * date >> /tmp/log.txt 


```

查看crontab添加列表

```sh
root@e:~# crontab -l
```

然后我们查看我们到任务执行情况

```sh
root@e:~# tail -f /tmp/log.txt
2016年 02月 28日 星期日 18:46:01 CST
2016年 02月 28日 星期日 18:47:01 CST
2016年 02月 28日 星期日 18:48:01 CST
2016年 02月 28日 星期日 18:49:01 CST
2016年 02月 28日 星期日 18:50:01 CST
2016年 02月 28日 星期日 18:51:02 CST
```
