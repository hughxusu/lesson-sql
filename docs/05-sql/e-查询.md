# 查询数据表

在数据库服务中，查询操作是最多的操作，其频率远超于增删改。在查询的过程中，通常还会伴随到条件、排序、分页等操作。查询语句的关键字为`select`。

为了演示查询操作，首先需要导入批量数据，数据文件保存在`\codes\mysql\songs_data_v1.csv`，使用DataGrip导入文件中的数据

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/mysql/Xnip2025-12-21_10-38-30.jpg" style="zoom:40%;" />

## 基本语法

标准查询语句如下

```sql
SELECT        -- 字段列表
FROM          -- 表名列表
WHERE         -- 条件列表
GROUP BY      -- 分组字段列表
HAVING        -- 分组后条件列表
ORDER BY      -- 排序字段列表
LIMIT         -- 分页参数
```

### 基础查询

查询指定字段

```sql
select id, title, audio_format, file_url from songs;
```

查询全部字段

```sql
select * from songs;
```

> [!warning]
>
> `*`号查询所有字段，在实际开发中尽量少用，主要是不直观、影响效率。

给字段设置别名

```sql
select id, title as '歌名', audio_format as '文件格式' from songs;
```

设置别名的简写方式

```sql
select id, title '歌名', audio_format '文件格式' from songs;
```

去除重复记录

```sql
select distinct audio_format from songs;
```

* `distinct`去除查询结果中的完全重复行。

### 条件查询

```sql
select id, title, duration from songs where duration >= 300;
```

* `where duration >= 300`选择`duration`大于300秒的歌曲。

查询条件中，可以使用运算符

| 运算符                | 功能                                           |
| --------------------- | ---------------------------------------------- |
| `>`                   | 大于                                           |
| `>=`                  | 大于等于                                       |
| `<`                   | 小于                                           |
| `<=`                  | 小于等于                                       |
| `=`                   | 等于                                           |
| `<>`或`!=`            | 不等于                                         |
| `between ... and ...` | 在某个范围之内（含最小、最大值）               |
| `in (...)`            | 在in之后的列表中的值，多选一                   |
| `like`                | 模糊匹配（`_`匹配单个字符，`%`匹配任意个字符） |
| `is null`             | 是NULL                                         |

查询不是MP3的歌曲

```sql
select id, title, audio_format from songs where audio_format != 'mp3';
```

查询时长在280到300之间的歌曲

```sql
select id, title, duration from songs where duration between 280 and 300;
```

> [!alert]
>
> `between 280 and 300`区间值的大小顺序不能弄错。

查询`'wav'`、`'m4a'`和`'wav'`这三种格式的数据

```sql
select id, title, audio_format from songs where audio_format in ('wav', 'm4a', 'wav');
```

查询名字是两个字的歌曲，`_`匹配任意一个字符。

```sql
select id, title from songs where title like '__';
```

查询名字中有“天”字的歌曲，`%`匹配任意多个字符。

```sql
select id, title from songs where title like '%天%';
```

查询`file_url`文件路径是空的记录有多少

```sql
select id, title, file_size from songs where file_size is null;
```

查询条件中，可以使用逻辑运算符

| 运算符        | 逻辑 | 描述                   |
| ------------- | ---- | ---------------------- |
| `and` 或 `&&` | 与   | 两个条件都为真，则为真 |
| `or` 或 `||`  | 或   | 一个条件为真，则为真   |
| `not` 或 `!`  | 非   | 对条取反               |

查询名字中有“天”字，且只有两个字的歌曲

```sql
select id, title from songs where title like '%天%' and title like '__';
```

查询名字有三个字或两个字的歌曲

```sql
select id, title from songs where title like '___' or title like '__';
```

部分`or`查询可以转换成`in`查询，这种情况优先使用`in`查询

```sql
select id, title, audio_format from songs where audio_format = 'm4a' or audio_format = 'flac' or audio_format = 'wav';
select id, title, audio_format from songs where audio_format in ('m4a', 'flac', 'wav');
```

查询非空数据

```sql
select id, title, duration from songs where duration is not null;
```

> [!note]
>
> 查询条件的优先级：
>
> 1. 优先级顺序从高到低：小括号、`not`、比较运算符、逻辑运算符。
> 2. `and`和`or`同时出现时，`and`优先级比`or`高。

### 聚合函数

聚会函数主要用于，对于数据进行统计，将一列数据作为一个整体，进行纵向计算。

> [!note]
>
> 1. 使用聚会函数后，看不到原始数据，只能看到统计结果。
> 2. `null`值不参与聚合函数运算。

常用的聚合函数

| 函数    | 功能     |
| ------- | -------- |
| `count` | 统计数量 |
| `max`   | 最大值   |
| `min`   | 最小值   |
| `avg`   | 平均值   |
| `sum`   | 求和     |

聚会函数的标准语法

```sql
select   -- 聚会函数
from     -- 表名
```

统计数据总数

```sql
select count(*) from songs;
```

统计`duration`非空的数据数量

```sql
select count(duration) from songs;
```

统计歌曲平均时长

```sql
select avg(duration) from songs;
```

统计歌曲的最长事件和最短时间

```sql
select max(duration) from songs;
select min(duration) from songs;
```

统计包含“天”字的歌曲，总体的时长是多少

```sql
select sum(duration) from songs where title like '%天%';
```

### 分组查询

分组查询的关键字

* `group by`用于控制分组的字段。
* `having`用于控制分组后的过滤条件。

`where`与`having`都是控制查询条件，二者的区别：

* 执行时机不同：`where`是分组之前进行过滤，不满足`where`条件，不参与分组；而`having`是分组之后对结果进行过滤。
* 判断条件不同：`where`不能对聚合函数进行判断，而`having`可以。

> [!note]
>
> 1. 对于分组查询，查询的字段一般为聚合函数和分组字段，查询其他字段无任何意义。
> 1. 支持多字段分组，语法为：`group by columnA, columnB`

统计不同文件类型歌曲的数量

```sql
select audio_format, count(*) as count from songs group by audio_format;
```

统计不同类型文件的文件平均大小

```sql
select audio_format, avg(file_size) from songs group by audio_format;
```

统计时长超过10MB的歌曲，对`audio_format`进行聚合，且数量超过8首歌的文件格式。

```sql
select audio_format, count(*) as format_count from songs where file_size > 10000000 group by audio_format having format_count > 8;
```

### 排序查询

对数据进行排序的关键字为`order by`

* 排序的方式：`asc`升序（默认值，可省略）；`desc`降序。 
* 如果是多字段排序，当第一个字段值相同时，才会根据第二个字段进行排序。

根据时长对歌曲进行排序

```sql
select id, title, duration from songs order by duration;
select id, title, duration from songs order by duration desc;
```

根据歌曲的时长和文件大小对数据进行排序

```sql
select id, title, duration, file_size from songs order by duration desc, file_size desc;
```

### 分页查询

分页查询（Pagination Query）是搜索的记录，分割成多个逻辑“页”，每次只返回其中一页的数据。不同的数据库有不同的实现，MySQL中使用`limit`进行分页。分页查询的计算公式
$$
\text{起始索引}=\left(\text{查询页码}-1\right)\times\text{每页显示记录数}
$$
查询第一页数据

```sql
select id, title, duration from songs limit 0, 10;
select id, title, duration from songs limit 10;
```

* `limit`关键字接收两个值：第一个是起始索引；第二个是每页显示记录数。
* 查询第一页数据时可以省略起始索引。

查询第三页数据

```sql
select id, title, duration from songs limit 20, 10;
```

## 执行顺序

在MySQL中语法的书写顺序和查询语句的执行数据线是不同的，下面是MySQL语句中查询关键字的执行顺序

![](https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/mysql/e8abec649d8c37cc00f1e2d03c6906c8.png)

查询时长超过300秒的歌曲

```sql
select id, title, duration from songs where duration > 320;
```

给歌曲表起别名，重新验证搜索结果

```sql
select s.id, s.title, s.duration from songs as s where s.duration > 320;
```

* `songs as s`定义别名后，可以在`select`关键字后使用`s.id`，可以说明`from`关键字先执行

代码格式更改为如下

```sql
select songs.id, songs.title, songs.duration as sd from songs where sd > 320;
```

程序执行提示：sd未知。

