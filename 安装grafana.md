# 官网
https://grafana.com/grafana/download

# 安装
``` javascript
# mkdir -p /usr/local/grafana
# cd /usr/local/grafana
# wget -P /usr/local/grafana https://s3-us-west-2.amazonaws.com/grafana-releases/release/grafana-4.6.1-1.x86_64.rpm
# yum -y localinstall grafana-4.6.1-1.x86_64.rpm
```

# 启动
``` javascript
# systemctl start grafana-server
# ps -aux | grep grafana-server
grafana   6299  0.4  0.5 357596 22988 ?        Ssl  11:16   0:00 /usr/sbin/grafana-server --config=/etc/grafana/grafana.ini --pidfile=/var/run/grafana/grafana-server.pid cfg:default.paths.logs=/var/log/grafana cfg:default.paths.data=/var/lib/grafana cfg:default.paths.plugins=/var/lib/grafana/plugins
root      6365  0.0  0.0 112664   972 pts/0    R+   11:17   0:00 grep --color=auto grafana-server
```

# 访问grafana
打开浏览器输入 http://localhost:3000 就可以访问了。用户名和密码默认都是admin。
