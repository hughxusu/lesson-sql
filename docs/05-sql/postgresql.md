# PostgreSQL

docker启动命令

```shell
docker run --name lesson-postgres -e POSTGRES_PASSWORD=123456 -p 5432:5432 -d postgres:18.1

docker run --name mysql -e MYSQL_ROOT_PASSWORD=123456 -p 3306:3306 -d mysql:8.4.7
```

登录postgresql

```shell
psql -U postgres
```

查询所有数据库

```sql
SELECT datname FROM pg_database;
```

查询当前数据库

```sql
SELECT current_database();
```



