layout: layout
title: 【mysql】mysql笔记-mysql函数运用
keywords:
  - mysql
  - mysql函数
  - mysql子查询
  - mysql联表查询
categories:
  - mysql
tags:
  - mysql
toc: true
date: 2018-05-16 09:31:00
---
#### 主副表查询
```
SELECT *,(select count({$dbPrefix}vedios.pid) from jdn_vedios where {$dbPrefix}vedios.pid={$dbPrefix}column.id) as pidCount	FROM `{$dbPrefix}column` WHERE `pid` = 197
```
#### 加法运算
```
"UPDATE `{$this->DbPrefix}videos` SET `views`=views+1 WHERE `id` = {$param['id']}"
```
<!--more-->
#### 子表查询
```
 $sql = "SELECT
        *,
        (SELECT group_concat(CONCAT(' ',{$prefix}fengge.title,' ')) FROM {$prefix}fengge WHERE FIND_IN_SET({$prefix}fengge.id,{$prefix}brand.fengge)) as fengge_str, -- 查风格中文
        (SELECT group_concat(CONCAT(' ',{$prefix}dingwei.title,' ')) FROM {$prefix}dingwei WHERE FIND_IN_SET({$prefix}dingwei.id,{$prefix}brand.dingwei)) as dingwei_str, -- 查定位中文
        (SELECT group_concat(CONCAT(' ',{$prefix}touzi.title,' ')) FROM {$prefix}touzi WHERE FIND_IN_SET({$prefix}touzi.id,{$prefix}brand.touzi)) as touzi_str, -- 查投资
        (SELECT group_concat(CONCAT(' ',{$prefix}zhengce.title,' ')) FROM {$prefix}zhengce WHERE FIND_IN_SET({$prefix}zhengce.id,{$prefix}brand.zhengce)) as zhengce_str, -- 查政策
        (SELECT group_concat(CONCAT(' ',{$prefix}diqu.title,' ')) FROM {$prefix}diqu WHERE FIND_IN_SET({$prefix}diqu.id,{$prefix}brand.diqu)) as diqu_str -- 地区
        FROM `{$prefix}brand`
        WHERE `username` = 'olisi'
        LIMIT 1";
        return ($this->query($sql))[0];
```
[mysql常用函数](https://blog.csdn.net/sugang_ximi/article/details/6664748)

#### 不重复查询函数 `distinct`和`GROUP BY`
  &emsp;`distinct`和`GROUP BY`用于查找单个字段不重复,如果个字段有多个相同值就以第一个为准,相对于`distinct`的只输出单个字段的数据,`GROUP BY `能输出以单个字段不重复的所有数据相较下,`GROUP BY`的操作会丰富些。
``` mysql
    CREATE DATABASE IF NOT EXISTS `debug`;
    USE `debug`;

    CREATE TABLE IF NOT EXISTS `func_distinct`(
       `id` INT UNSIGNED AUTO_INCREMENT,
       `title` VARCHAR(100) NOT NULL COMMENT '标题',
       PRIMARY KEY ( `id`  )
   )ENGINE=InnoDB DEFAULT CHARSET=utf8;

    INSERT INTO `func_distinct` (`id`,`title`) VALUES ( NULL,'标题1');
    INSERT INTO `func_distinct` (`id`,`title`) VALUES ( NULL,'标题1');
    INSERT INTO `func_distinct` (`id`,`title`) VALUES ( NULL,'标题2');
    INSERT INTO `func_distinct` (`id`,`title`) VALUES ( NULL,'标题2');

    SELECT distinct `title` from `func_distinct`;         /*输出单个title字段*/
    SELECT COUNT(distinct `title`) from `func_distinct`;  /*统计不重复标题的数量*/
    SELECT * FROM `func_distinct` GROUP BY `title`;       /*输出单个title字段*/
    SELECT COUNT(*) FROM `func_distinct` GROUP BY `title`;/*统计各个重复标题的数量*/

```
#### 分割符查询函数 `FIND_IN_SET`
  &emsp;`FIND_IN_SET`用于查找字符中被","分割的字串,如`SELECT FIND_IN_SET('b','a,b,c,d')`;会返回`b`;它同`like`查询不一样，`FIND_IN_SET`要准确得多。以下实例查找`green`标签的数据:
``` mysql 
    CREATE DATABASE IF NOT EXISTS `debug`; /*测试库*/
    USE `debug`;

    CREATE TABLE IF NOT EXISTS `func_find_in_set`(
       `id` INT UNSIGNED AUTO_INCREMENT,
       `tag` VARCHAR(100) NOT NULL COMMENT '标签',
       PRIMARY KEY ( `id`  )
   )ENGINE=InnoDB DEFAULT CHARSET=utf8;

    INSERT INTO `func_find_in_set` (`id`,`tag`) VALUES ( NULL,'red,yellow');
    INSERT INTO `func_find_in_set` (`id`,`tag`) VALUES ( NULL,'red,green');
    INSERT INTO `func_find_in_set` (`id`,`tag`) VALUES ( NULL,'green,white');
    INSERT INTO `func_find_in_set` (`id`,`tag`) VALUES ( NULL,'yellow,pink');

    SELECT * FROM `func_find_in_set` WHERE FIND_IN_SET('green',func_find_in_set.tag); /*查找标签为green的数据*/
    SELECT COUNT(*) FROM `func_find_in_set` WHERE FIND_IN_SET('green',func_find_in_set.tag); /*统计green标签的数量*/
```

####  字符替换 `REPLACE`
``` mysql
    UPDATE `table` SET `colum`=REPLACE(`colum`,'toReplaceString','targetString')
```




