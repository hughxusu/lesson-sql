# 定义数据库

SQL通用语法

1. SQL语句可以单行或多行书写，以分号结尾。
2. SQL语句可以使用空格/缩进来增强语句的可读性。
3. PgSQL数据库的SQL语句不区分大小写，通常使用小写。
4. 注释：
   * 单行注释：`--`注释内容或`#`注释内容。
   * 多行注释：`/* 注释内容 */`

## 创建数据库

查询所有数据库

```sql
\list -- 简写\l
```

在终端中输入

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/mysql/Xnip2026-01-02_21-42-13.jpg" style="zoom:40%;" />

查询当前数据库

```sql
select current_database();
```

查看版本数据库版本

```sql
select version();
```

### 创建数据库

使用`create`命令创建数据库

```sql
create database sqllesson;
```

> [!alert]
>
> 在同一个服务器中，不能创建两个名称相同的数据库，否则将会报错。

创建数据库并指定字符集

```sql
create database lessons encoding 'UTF8';
```

* `charset utf8mb4`使用utf8字符集。

### 删除数据库

使用`drop`命令删除数据库

```sql
drop database lessons;
```

### 切换数据库

在同一个服务器中可以创建多个数据库，使用`use`命令选中需要使用的数据库

```sql
\c sqllesson
```

选择后使用`select current_database();`查看数据库选择状态。

## 数据库IDE

在控制台中操作数据库十分不便，在日常的开发中，通常会借助图形化界面，操作数据库。

[DataGrip](https://www.jetbrains.com/zh-cn/datagrip/)是[jetbrains](https://www.jetbrains.com/zh-cn/)旗下的一款，适用于关系型和NoSQL数据库的强大跨平台IDE。虽然DataGrip是收费软件学生和教师可以通过[大学电子邮件地址](https://www.jetbrains.com/shop/eform/students)申请免费使用。

> [!warning]
>
> 邮箱必须是以`.edu`结尾的电子邮箱。

[DataGrip软件下载](https://www.jetbrains.com/zh-cn/datagrip/download/)

初始化项目

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/mysql/Xnip2025-12-16_22-01-33.jpg" style="zoom:40%;" />

创建新数据库链接

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/mysql/Xnip2026-01-03_09-18-42.jpg" style="zoom:40%;" />

配置链接信息

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/mysql/Xnip2026-01-03_09-27-20.jpg" style="zoom:40%;" />

选择数据库

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/mysql/Xnip2026-01-03_09-32-04.jpg" style="zoom:40%;" />

使用DataGrip创建数据库

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/mysql/Xnip2026-01-03_09-38-59.jpg" style="zoom:40%;" />

架构（Schema）：数据库的“蓝图”，定义了表的结构，以及表之间的关系

* Schema隔离了数据库内的命名空间。
* 可以给不同用户，分配不同Schema的权限。
* 对于大型项目，实现按业务模块隔离。
* 不同Schema的表之间，可以建立外键关联。

Database、Schema和数据表之间的关系

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/mysql/database-schema-table.png" style="zoom:50%;" />

SQL命令行

<img src="https://raw.githubusercontent.com/hughxusu/lesson-knowledge/develop/images/mysql/Xnip2026-01-03_10-05-52.jpg" style="zoom:40%;" />

