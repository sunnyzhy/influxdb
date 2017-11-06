# 启动客户端
```
# influx
Connected to http://localhost:8086 version 1.3.7
InfluxDB shell version: 1.3.7
> 
```

# 显示数据库
```
> show databases;
name: databases
name
----
_internal
```

# 创建数据库
```
> create database mydb;
> show databases;
name: databases
name
----
_internal
mydb
```

# 删除数据库
```
> drop database mydb;
> show databases;
name: databases
name
----
_internal
```

# 显示数据表
```
> use mydb;
Using database mydb
> show measurements;
> 
```

# 创建数据表
```
> insert cpu,host=serverA,region=us_west value=0.64
> show measurements;
name: measurements
name
----
cpu
```

# 删除数据表
```
> drop measurement cpu;
> show measurements;
> 
```

# 添加数据
```
> insert cpu,host=serverA,region=us_west value=0.64
> select * from cpu;
name: cpu
time                host    region  value
----                ----    ------  -----
1509956976304106075 serverA us_west 0.64
```

# 更新数据
```
> insert cpu,host=serverA,region=us_west value=0.66 1509956976304106075
> select * from cpu;
name: cpu
time                host    region  value
----                ----    ------  -----
1509956976304106075 serverA us_west 0.66
```
    tags 和 timestamp相同时数据会执行覆盖操作，相当于InfluxDB的更新操作。

# 删除数据
```
> delete from cpu where time=1509956976304106075;
> select * from cpu;
> 
```
