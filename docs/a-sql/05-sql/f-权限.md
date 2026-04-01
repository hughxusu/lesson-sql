# 数据库控制与函数

## 数据库控制

数据库控制主要用来管理数据库用户、控制数据库的访问权限。 

> [!alert]
>
> 这类操作，主要由数据库管理员执行，开发人员操作需要慎用！

MySQL中的所有用户保存在`mysql`数据库下`user`，查询用户表

```sql
select * from mysql.user;
```

> [!warning]
>
> `mysql` 数据库是系统数据库，无论当前位于哪个数据库都可以访问。

权限查询结果

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/mysql/Xnip2025-12-23_19-25-01.jpg" style="zoom:45%;" />

* Host字段表示当前用户访问的主机
  * `localhost`代表只能够在当前本机访问，不可以远程访问的。
  * `%`代表可以从任意地址访问。
* User字段表示访问该数据库的用户名。
* 在MySQL中需要通过Host和User来唯一用户。

### 用户管理

创建用户

```sql
create user 'harry'@'localhost' identified by '123456';
```

创建后使用DataGrip链接数据库

```sql
create user 'harry'@'%' identified by '123456';
```

修改用户的密码

```sql
alter user 'harry'@'%' identified by '12345'
```

删除用户

```sql
drop user 'harry'@'localhost';
```

可以登录新建用户查看器数据库权限。

### 权限控制

MySQL中常用的权限如下

| 权限                    | 说明               |
| ----------------------- | ------------------ |
| `all`、`all privileges` | 所有权限           |
| `select`                | 查询数据           |
| `insert`                | 插入数据           |
| `update`                | 修改权限           |
| `delete`                | 删除权限           |
| `alter`                 | 修改表             |
| `drop`                  | 删除数据库/表/视图 |
| `create`                | 创建数据库/表      |

查看权限

```sql
show grants for 'harry'@'%';
```

* `grant`表示MySQL中的授权命令的关键字，可以给用户赋予数据库权限。

赋予用户harry数据库所有表的所有操作权限

```sql
grant all on sqllesson.* to 'harry'@'%';
```

* `grant all`赋予全部数据库权限。
* `sqllesson.*`表示数据库sqllesson下的所有表。

撤销用户权限

```sql
revoke all on sqllesson.* from 'harry'@'%';
```

## 函数

函数是组织好，可重复使用的，用来实现单一或关联功能的代码段。MySQL中内置了部分函数，供用户调用，以方便数据的处理。MySQL中的函数主要分为以下四类： 字符串函数、数值函数、日期函数、流程函数。

### 字符串函数

1. `concat(s1, s2, ..., sn)`字符串拼接函数，将`s1, s2, ..., sn`拼接成一个字符串输出。``s1, s2, ..., sn`可以是任意数据类型。

```sql
select id, concat(title, ' - ', audio_format) from songs where duration > 300;
```

2. ` lower(str)`将字符串`str`全部转为小写

```sql
select lower('HELLO, WORLD!');
```

3. `upper(str)`将字符串`str`全部转为大写

```sql
select upper('hello, world!');
```

4. `lpad(str, n, pad)`左填充，用字符串`pad`对`str`的左边进行填充，达到`n`个字符串长度。

```sql
select id, lpad(title, 10, '-') from songs where id < 10;
```

5. `rpad(str, n, pad)`右填充，用字符串`pad`对`str`的右边进行填充，达到`n`个字符串长度。

```sql
update songs set title = rpad(title, 10, '-') where id < 10;
```

6. `trim(str)`去掉字符串头部和尾部的空格。

```sql
select trim('  hello, world!  ');
```

7. `substring(str, start, len)`字符串截取，返回从字符串`str`从`start`位置起的`len`个长度的字符串。

```sql
select substring('hello, world!', 1, 5);
```

### 数值函数

1. `ceil(x)`向上取整。

```sql
select ceil(9.1);
```

2. `floor(x)`向下取整。

```sql
select floor(10.9);
```

4. `mod(x, y)`返回`x/y`的模。

```sql
select mod(10, 3);
```

5. `rand()`返回0~1内的随机数。

```sql
select rand();
```

6. `round(x, y)`求参数`x`的四舍五入的值，保留`y`位小数。

```sql
select round(3.1415926, 2);
```

### 日期函数

1. `curdate()`返回当前日期。

```sql
select curdate();
```

2. `curtime()`返回当前时间。

```sql
select curtime();
```

3. `now()`返回当前日期和时间。

```sql
select now();
```

4. `year(date)`获取指定`date`的年份。
5. `month(date)`获取指定`date`的月份。
6. `day(date)`获取指定`date`的日期。

```sql
select year(now());
select month(now());
select day(now());
```

7. `date_add(date, INTERVAL expr type)`返回一个日期/时间值加上一个时间间隔`expr`后的时间值。

```sql
select date_add(now(), interval 10 year);
```

8. `datediff(date1, date2)`返回起始时间`date1`和结束时间`date2`之间的天数。

```sql
select datediff(now(), '2000-01-01');
```

### 流程函数

1. `if(value, t, f)`如果`value`为`true`，则返回`t`，否则返回`f`。如果文件是不为空，显示`'----'`，否则显示文件异常。

```sql
select id, title, if(file_size, '----', '文件异常') as '文件状态' from songs;
```

2. `ifnull(value1 , value2)`如果`value1`不为空，返回`value1`，否则返回`value2`。

```sql
select ifnull('ok', 'Default');
select ifnull('', 'Default');
select ifnull(null, 'Default');
```

3. `case when val1 then res1 ... else default end`如果`val1`为`true`，返回`res1`，…，否则返回`default`默认值。歌曲时间小于240，显示短；240到300之间，显示正常；大于300，显示长。

```sql
select id, title,
   (case
       when duration <= 240 then '短'
       when duration <= 300 then '正常'
       else '长' end
   ) as '长度'
from songs;
```

4. `case expr when val1 then res1 ... else default end`如果`expr`的值为`val1`，返回`res1`，…，否则返回`default`默认值。如果文件格式为mp3和wav返回音质一般，否则返回高清。

```sql
select id, title,
   (case audio_format
       when 'mp3' then '一般'
       when 'wav' then '一般'
       else '高清' end
   ) as '音质'
from songs;
```





