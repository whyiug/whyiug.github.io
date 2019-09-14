---
layout: post
title: "SQL Server Functions"
description: "SQL Server Functions"
categories: [sql]
tags: [sql server]
redirect_from:
  - /2015/04/22/
---
## 时间函数

### datetime时间和UNIX时间戳的转化
**普通时间转换为时间戳**

```sql
select datediff(s, '1970-01-01 08:00:00', getdate());
```
**时间戳转化为普通事件**

```sql
select dateadd(s, SELECT DATEDIFF(S,'1970-01-01 08:00:00', GETDATE())  ,'1970-01-01 08:00:00');
```

## 新增或更新自增长列

### 新增

```sql
SET IDENTITY_INSERT table_name ON 
---insert ...
SET IDENTITY_INSERT table_name OFF 
```

### 更新

```sql
SET IDENTITY_INSERT table_name ON 
---delete ...
---insert ...
SET IDENTITY_INSERT table_name OFF 
```

### 重置自增长属性

```sql
DBCC CHECKIDENT(table_name, RESEED, 100)
```

## MD5加密

```sql
SELECT right(sys.fn_VarBinToHexStr(hashbytes('MD5','redinfo')),32)
```

