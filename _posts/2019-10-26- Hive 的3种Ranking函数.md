---
layout: post
title: "Hive 的3种Ranking函数"
description: ""
categories: [ALG]
tags: [Hive]
redirect_from:
  - /2019/10/26/
---



> 有的文章把`ROW_NUMBER(), RANK(), DENSE_RANK()`这三个函数也叫做排序，这样容易与` Order by, Sort by ,Dristribute by,Cluster By`混淆。所以本文保留其英文名称，统称前者为`Rank`函数，后者为`Order`函数。

本文将用几个例子来说明`ROW_NUMBER(), RANK(), DENSE_RANK()`这三个`Ranking`函数的使用方法和区别。

先构造数据集`t`如下：

| user |
| ---- |
| a    |
| a    |
| a    |
| b    |
| c    |
| c    |
| d    |
| e    |

## ROW_NUMBER()

我们输入查询语句：

```sql
SELECT user, ROW_NUMBER() OVER(ORDER BY user ASC) AS rn
FROM t
```

注意需要在`over() `里显式的加上`ORDER BY V`子句。

输出结果：

| user | rn   |
| ---- | ---- |
| a    | 1    |
| a    | 2    |
| a    | 3    |
| b    | 4    |
| c    | 5    |
| c    | 6    |
| d    | 7    |
| e    | 8    |

另外一种加上`PARTITION`的用法是：

```sql
SELECT user, ROW_NUMBER() OVER(PARTITION BY user ORDER BY user ASC) AS rn
FROM t
```

结果的排序方式也是按照`user`分组：

| user | rn   |
| ---- | ---- |
| a    | 1    |
| a    | 2    |
| a    | 3    |
| b    | 1    |
| c    | 1    |
| c    | 2    |
| d    | 1    |
| e    | 1    |

这种用法在分组取`TopN`的时候非常好用。

## RANK()

查询语句：

```sql
SELECT user, RANK() OVER(ORDER BY user ASC) AS rn
FROM t
```

输出结果：

| user | rn   |
| ---- | ---- |
| a    | 1    |
| a    | 1    |
| a    | 1    |
| b    | 4    |
| c    | 5    |
| c    | 5    |
| d    | 7    |
| e    | 8    |

可以看到每一组的排序结果是一样的，就像是体育比赛，同样成绩的名字并列，并在后面留下空档，比如前面第二名并列了，随后就是第四名。

## DENSE_RANK()

这个函数比较于`RANK()`可以避免空档，它是稠密的(`dense`)。

查询语句：

```sql
SELECT DISTINCT user, DENSE_RANK() OVER(ORDER BY v) AS rn
FROM t
```

输出结果：

| user | rn   |
| ---- | ---- |
| a    | 1    |
| a    | 1    |
| a    | 1    |
| b    | 2    |
| c    | 3    |
| c    | 3    |
| d    | 4    |
| e    | 5    |

## 总结

最后将前面讲到的例子汇总到一起，这样看的更加清楚。

查询语句：

```sql
SELECT  user
        ,ROW_NUMBER() OVER(ORDER BY user) AS rn1
        ,ROW_NUMBER() OVER(PARTITION BY user ORDER BY user ASC) AS rn2
        ,RANK() OVER(ORDER BY user) AS rn3
        ,DENSE_RANK() OVER(ORDER BY user) AS rn4
FROM    t
```

输出结果：

| user | rn 1(ROW_NUMBER) | rn2(PATITION) | rn3(RANK) | rn4(DENSE_RANK) |
| ---- | ---------------- | ------------- | --------- | --------------- |
| a    | 1                | 1             | 1         | 1               |
| a    | 2                | 2             | 1         | 1               |
| a    | 3                | 3             | 1         | 1               |
| b    | 4                | 1             | 4         | 2               |
| c    | 5                | 1             | 5         | 3               |
| c    | 6                | 2             | 5         | 3               |
| d    | 7                | 1             | 7         | 4               |
| e    | 8                | 1             | 8         | 5               |

[^Ranking Functions: ROW_NUMBER(), RANK(), and DENSE_RANK()]: https://dzone.com/articles/difference-between-rownumber

