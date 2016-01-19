# Использование Fail2ban для защиты Wordpress от атак

Для своей работы Fail2ban использует регулярные выражения и в случае нахождения совпадений в лог-файлах блокирует IP и после истечения срока блокировки разблокирует.

После настройки мы будем иметь в лог-файлах (/var/log/auth.log) все события авторизации в Wordpress.

## Установка fail2ban

```
apt-get update && apt-get install fail2ban
```

После установки fail2ban будет находить попытки авторизации по SSH и после 5 попыток будет блокировать IP на 600 секунд.

## Журналирование событий авторизации

Необходимо перенаправить события авторизации в Wordpress в системный журнал авторизации (`/var/log/auth.log`).
Для этой задачи хорошо подходит плагин [WP fail2ban](https://wordpress.org/plugins/wp-fail2ban/), или можно написать свой плагин, если данный плагин не подходит. После активации указанного плагина все события авторизации отправляются в журнал `/var/log/auth.log`

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
