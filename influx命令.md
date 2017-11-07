# 启动客户端
### 默认没有用户名、密码
```
# influx
Connected to http://localhost:8086 version 1.3.7
InfluxDB shell version: 1.3.7
> 
```
### 设置用户名、密码
- 创建admin帐号密码并授权
```
> create user admin with password 'admin'
> grant all privileges to admin
> exit
```

- 修改配置文件
```
# vim influxdb.conf
[http]
   enabled = true
   bind-address = ":8086"
   auth-enabled = true
   realm = "InfluxDB"
   log-enabled = true
   write-tracing = false
   pprof-enabled = true
   https-enabled = false
   https-certificate = "/etc/ssl/influxdb.pem"

# systemctl restart influxdb
```
- 登录
```
# influx
Connected to http://localhost:8086 version 1.3.7
InfluxDB shell version: 1.3.7
> auth
username: admin
password: 
```
或者
```
# influx -username admin -password admin
```

# 用户管理命令
### 管理员用户管理
- 创建一个新的管理员用户
```
CREATE USER <username> WITH PASSWORD '<password>' WITH ALL PRIVILEGES
```

- 为一个已有用户授权管理员权限
```
GRANT ALL PRIVILEGES TO <username>
```

- 取消用户权限
```
REVOKE ALL PRIVILEGES FROM <username>
```

- 展示用户及其权限
```
SHOW USERS
```

### 非管理员用户管理：
- 创建一个新的普通用户
```
CREATE USER <username> WITH PASSWORD '<password>'
```

- 为一个已有用户授权
```
GRANT [READ,WRITE,ALL] ON <database_name> TO <username>
```

- 取消权限
```
REVOKE [READ,WRITE,ALL] ON <database_name> FROM <username>
```

- 展示用户在不同数据库上的权限
```
SHOW GRANTS FOR <user_name>
```

### 普通用户账号功能管理
- 重设密码
```
SET PASSWORD FOR <username> = '<password>'
```
   
- 删除用户
```
DROP USER <username>
```

# 显示数据库
```
> show databases
name: databases
name
----
_internal
```

# 创建数据库
```
> create database mydb
> show databases
name: databases
name
----
_internal
mydb
```

# 删除数据库
```
> drop database mydb
> show databases
name: databases
name
----
_internal
```

# 显示数据表
```
> use mydb
Using database mydb
> show measurements
> 
```

# 创建数据表
```
> insert cpu,host=serverA,region=us_west value=0.64
> show measurements
name: measurements
name
----
cpu
```

# 删除数据表
```
> drop measurement cpu
> show measurements
> 
```

# 添加数据
```
> insert cpu,host=serverA,region=us_west value=0.64
> select * from cpu
name: cpu
time                host    region  value
----                ----    ------  -----
1509956976304106075 serverA us_west 0.64
```

# 更新数据
```
> insert cpu,host=serverA,region=us_west value=0.66 1509956976304106075
> select * from cpu
name: cpu
time                host    region  value
----                ----    ------  -----
1509956976304106075 serverA us_west 0.66
```
    tags 和 timestamp相同时数据会执行覆盖操作，相当于InfluxDB的更新操作。

# 删除数据
```
> delete from cpu where time=1509956976304106075
> select * from cpu
> 
```

# 注意
```
Strings only need to be quoted when they're used as field values. Tag values can only ever be strings, and do not need to be quoted. 
tags里的字符串不用加引号，fields里的字符串必须加引号
```
- 错误
```
> insert temperature,machine=unit42 type=assembly,external=25,internal=37
ERR: {"error":"unable to parse 'temperature,machine=unit42 type=assembly,external=25,internal=37': invalid boolean"}

> 
```

- 正确
```
> insert temperature,machine=unit42 type="assembly",external=25,internal=37
> 
```
```
> insert temperature,machine=unit42,type=assembly external=25,internal=37
> 
```
