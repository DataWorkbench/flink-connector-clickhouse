# flink-connector-clickhouse

#### 配置
```shell
cp clickhouse-jdbc-0.2.4.jar /flink/lib
cp flink-connector-jdbc_2.11-1.11.1.jar /flink/lib
cp guava-19.0.jar /flink/lib
``` 

#### flink sql 自定义 (优化 ClickHouse 集群连接 )connector

```sql
%flink.conf

flink.yarn.appName zeppelin-test-ch

flink.execution.jars /Users/lucas/IdeaProjects/microi/flink-microi-conn/clickhouse/target/clickhouse-1.0-SNAPSHOT.jar
```

```sql
%flink.ssql

DROP TABLE IF EXISTS ch;
CREATE TABLE ch (
    id String,
    name String,
    age String,
    create_date Date
) WITH (
    'connector' = 'clickhouse',
    'url' = 'jdbc:clickhouse://172.16.8.164:8123,172.16.8.165:8123,172.16.8.166:8123/dc2', -- 可配置集群地址，写入时随机选择连接写入，不会一直使用一个连接写入
    'table-name' = 'user_name',
    'username' = 'default',
    'password' = '',
    'format' = 'json'
);
```

```sql
%flink.ssql(type=update)

select * from ch;

```

```sql
%flink.ssql(type=update)

insert into ch 
select '8' as id, '3' as name, '4' as age, cast('2020-01-03' as DATE) as create_date

```
