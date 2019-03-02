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
```
> INSERT cpu,host=serverA,region=us_west value=0.64

>
```

这样，一个点——测量变量是cpu，tags是host和region，测量值value为0.64，被写入到数据库中了。
然后，可以通过如下语句查到这条数据记录：
```
> SELECT "host", "region", "value" FROM "cpu"
name: cpu
---------
time                                     host       region   value
2016-10-21T19:28:07.580664347Z  serverA   us_west    0.64
```

在一个测量变量里含有两种fields：
```
> INSERT temperature,machine=unit42,type=assembly external=25,internal=37
>
```

一次查询操作中，可以使用*操作符获取所有的fields和tags：
```
> SELECT * FROM "temperature"
name: temperature
-----------------
time                                     external     internal   machine    type
2016-10-21T19:28:08.385013942Z  25              37          unit42  assembly

>
```
