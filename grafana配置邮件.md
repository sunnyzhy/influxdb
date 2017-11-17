# 开启163邮箱的客户端授权验证
```
登录163邮箱 -> 设置 -> 帐号与邮箱中心 -> 客户端授权密码 -> 选择开启 -> 设置授权码(auth_code)
```

# 配置smtp
```
# cd /etc/grafana

# vim grafana.ini
#################################### SMTP / Emailing ##########################
[smtp]
enabled = true
host = smtp.163.com:25
user = xxx@163.com
# If the password contains # or ; you have to wrap it with trippel quotes. Ex """#password;"""
password = auth_code
cert_file =
key_file =
skip_verify = true
from_address = xxx@163.com
from_name = Grafana
# EHLO identity in SMTP dialog (defaults to instance_name)
;ehlo_identity = dashboard.example.com

[emails]
;welcome_email_on_sign_up = false

# systemctl restart grafana-server
```

# 新增Alerting
```
Alerting -> Notification channels -> New Channel 
```
