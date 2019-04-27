---
layout: post
title: Php 调试工具
categories: Php
description: 定义一个php调试工具，有助于快速准确调试api接口
keywords: php debug
---


### 代码示例

```php

/**
 * [生成日期目录]
 * @param  string $file_name [日子文件名]
 * @return [type]            [日子文件名和路径]
 */
function log_file($file_name = '')
{
    if(empty($file_name))
    {
        $name = 'log_'.'.php';
    }else
    {
        $name = $file_name.'.php';
    }

    // logs这一级目录
    $directory = dirname(dirname(__FILE__)).'/data';
    if ( ! is_dir($directory))
    {
        mkdir($directory, 0777);
        chmod($directory, 0777);
    }

    // mylog这一级目录
    $directory .= DIRECTORY_SEPARATOR.'log';
    if ( ! is_dir($directory))
    {
        mkdir($directory, 0777);
        chmod($directory, 0777);
    }
    // 年这一级目录
    $directory .= DIRECTORY_SEPARATOR.date('Y');
    if ( ! is_dir($directory))
    {
        mkdir($directory, 0777);
        chmod($directory, 0777);
    }

    // 月这一级目录
    $directory .= DIRECTORY_SEPARATOR.date('m');
    if ( ! is_dir($directory))
    {
        mkdir($directory, 0777);
        chmod($directory, 0777);
    }

    // 天这一级目录
    $directory .= DIRECTORY_SEPARATOR.date('d');
    if ( ! is_dir($directory))
    {
        mkdir($directory, 0777);
        chmod($directory, 0777);
    }

    // 要写入的文件
    $filename = $directory.DIRECTORY_SEPARATOR.$name;
    if ( ! file_exists($filename))
    {
        file_put_contents($filename,'<?php die();?>'.PHP_EOL);
        chmod($filename, 0777);
    }
    return $filename;
}

/**
 * [写日志,带时间 这个为临时调试使用的log]
 * @param  [type|array] $params [要写入的数据]
 * @param  string $file   [日至文件名]
 * @return [type]         [description]
 */
function my_debug($params, $file = 'my_debug')
{
    clearstatcache();
    $file = log_file($file);
    $size = @filesize($file);
    $time = date('Y-m-d H:i:s');
    $contents = ($size ? '' : "<?php die();?>\n") . $time . PHP_EOL . var_export($params, TRUE) . PHP_EOL.PHP_EOL;
    @file_put_contents($file, $contents, FILE_APPEND | LOCK_EX);
}

```

调用方式
```php

$data = ['name'=>'demo', 'uid'=>100];
my_debug($data);

```

到网站根目录下面查看日志文件
```php
data/log/2018/08/01/my_debug.php
```
