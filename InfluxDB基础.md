# InfluxDB基本数据结构
|数据结构|含义|
|--|--|
|database|数据库|
|measurement|数据库中的表|
|retention policy|保存策略：让InfluxDB能够知道可以丢弃哪些数据，设置数据自动清除时间，从而更高效的处理数据|
|points|表里面的一行数据|

# points数据结构
|数据结构|含义|
|--|--|
|time|每个数据记录时间，是数据库中的主索引(会自动生成)|
|fields|各种记录值（没有索引的属性）也就是记录的值|
|tags|各种有索引的属性|
|series|表示这个表里面的数据，可以在图表上画成几条线：通过tags排列组合算出来。|

特别提醒：
1. time 相当于表的主键，当一条数据的time和tags完全相同时候，新数据会替换掉旧数据，旧数据则丢失

2. tags 和time可以作为排序字段，field则不可以。如：**ORDER BY time DESC**

3. 设置了保存策略后，若此保存策略为设置成默认保存策略（一个库可以有多个保存策略），则在查询时，表名（measurement）前，要加上保存策略
举例：
保留策略为two-hour不是默认保存策略，则查询时候，需要指定其保存策略
```sql
> select * from two-hour.measure where time > now() -10
```

4. fields和tags的字段类型是由存入的第一条记录值决定的。

- 如第一条记录fieldA的值为2，想插入一条记录，fieldA字段值为3.14的值，就会报错。因为该字段已经被初始化为整型了。

- 如第一条记录fieldB存储的是3,想插入一条记录，fieldB字段值为hello,则也会报错，该字段已被初始化成整型，不能再写入字符串了。

# InfluxDB语法
在InfluxDB中的数据是通过"时间序列组织的"，包含一个测量变量，比如cpu_load或者温度。时间序列有无数的点，每个点都是metric的离散采样。点包含time（一个时间戳）、一个measurement测量变量（比如是cpu_load），至少一个kv键值对field（测量变量的值，比如"value=0.64"，或者"temperature=21.2"），很多的kv键值对tags（包含了任何关于变量的元数据信息，比如"host=server01","region=EMEA", "dc=Frankfurt"） 。
在概念上，你可以把一个measurement看做是一个SQL表，主索引一直都是时间。**tags和fields实际上是表中的列。tags会被索引，但是fields不会。**区别是，使用InfluxDB，你可以有百万个测量变量，但是不需要设计前面的架构，空值不会被存储。
点会被写入到InfluxDB，使用行协议（Line Protocol），格式如下：
```
<measurement>[,<tag-key>=<tag-value>...] <field-key>=<field-value>[,<field2-key>=<field2-value>...] [unix-nano-timestamp]
```

下面是可以写入到InfluxDB的点的所有例子：
```
cpu,host=serverA,region=us_west value=0.64
payment,device=mobile,product=Notepad,method=credit billed=33,licenses=3i 1434067467100293230
stock,symbol=AAPL bid=127.46,ask=127.48
temperature,machine=unit42,type=assembly external=25,internal=37 1434067467000000000
```

使用influx命令行，可以插入一个个时间序列点，比如使用如下的语句：
```sql
> INSERT cpu,host=serverA,region=us_west value=0.64

>
```

这样，一个点——测量变量是cpu，tags是host和region，测量值value为0.64，被写入到数据库中了。
然后，可以通过如下语句查到这条数据记录：
```sql
> SELECT "host", "region", "value" FROM "cpu"
name: cpu
---------
time                                     host       region   value
2016-10-21T19:28:07.580664347Z  serverA   us_west    0.64
```

在一个测量变量里含有两种fields：
```sql
> INSERT temperature,machine=unit42,type=assembly external=25,internal=37
>
```

一次查询操作中，可以使用*操作符获取所有的fields和tags：
```sql
> SELECT * FROM "temperature"
name: temperature
-----------------
time                                     external     internal   machine    type
2016-10-21T19:28:08.385013942Z  25              37          unit42  assembly

>
```
