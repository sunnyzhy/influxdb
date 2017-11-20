# 官网
https://portal.influxdata.com/downloads

# 安装
```
# mkdir -p /usr/local/influxdb

# cd /usr/local/influxdb

# wget -P /usr/local/influxdb https://dl.influxdata.com/influxdb/releases/influxdb-1.3.7.x86_64.rpm

# yum -y localinstall influxdb-1.3.7.x86_64.rpm
```

# 启动
```
# systemctl start influxdb

# ps -aux | grep influxdb
influxdb  4441  0.1  0.4 274284 15516 ?        Ssl  10:22   0:00 /usr/bin/influxd -config /etc/influxdb/influxdb.conf
root      4521  0.0  0.0 112664   964 pts/0    R+   10:26   0:00 grep --color=auto influxdb
```

# 开机启动influxdb
```
# systemctl enable influxdb
```

# 开机不启动influxdb
```
# systemctl disable influxdb
```
