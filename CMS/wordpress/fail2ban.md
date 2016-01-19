# Использование Fail2ban для защиты Wordpress от атак

## Установка fail2ban

```
apt-get update && apt-get install fail2ban
```

## Настройка фильтра

```
cd /etc/fail2ban/filter.d
wget http://plugins.svn.wordpress.org/wp-fail2ban/trunk/wordpress.conf
```

/etc/fail2ban/jail.d/wordpress.conf

```
[wordpress]
enabled = true
filter = wordpress
logpath = /var/log/auth.log
port = http,https
```

```
service fail2ban restart
```
