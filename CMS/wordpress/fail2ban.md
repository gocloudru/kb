# Использование Fail2ban для защиты Wordpress от атак

Для своей работы Fail2ban использует регулярные выражения и в случае нахождения совпадений в лог-файлах блокирует IP и после истечения срока блокировки разблокирует.

После настройки мы будем иметь в лог-файлах (/var/log/auth.log) все события авторизации в Wordpress.

## Установка fail2ban

```
apt-get update && apt-get install fail2ban
```

После установки fail2ban будет находить попытки авторизации по SSH и после 5 попыток будет блокировать IP на 600 секунд.

## Журналирование событий авторизации

...

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
