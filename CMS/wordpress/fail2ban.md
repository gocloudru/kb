# Использование Fail2ban для защиты Wordpress от атак

Для своей работы Fail2ban использует регулярные выражения и в случае нахождения совпадений в лог-файлах блокирует IP и после истечения срока блокировки разблокирует.

После настройки мы будем иметь в лог-файлах (/var/log/auth.log) все события авторизации в Wordpress.

## Установка fail2ban

```
apt-get install fail2ban
```

После установки fail2ban будет находить попытки авторизации по SSH и после 5 попыток будет блокировать IP на 600 секунд.

## Журналирование событий авторизации

Необходимо перенаправить события авторизации в Wordpress в системный журнал авторизации (`/var/log/auth.log`).
Для этой задачи хорошо подходит плагин [WP fail2ban](https://wordpress.org/plugins/wp-fail2ban/), или можно написать свой плагин, если данный плагин не подходит. После активации указанного плагина все события авторизации отправляются в журнал `/var/log/auth.log`

В случае если используется Nginx как обратный (reverse) прокси-сервер, то стоит обратиться к [FAQ](https://wordpress.org/plugins/wp-fail2ban/faq/) (WP_FAIL2BAN_PROXIES), чтобы избежать пролировки всех пользователей

## Настройка фильта и fail

Указанный выше плагин уже содержит правила для поиска, его достаточно скачать по адресу `http://plugins.svn.wordpress.org/wp-fail2ban/trunk/wordpress.conf`:

`cd /etc/fail2ban/filter.d`

`wget http://plugins.svn.wordpress.org/wp-fail2ban/trunk/wordpress.conf`

Сейчас определим jail. Создадим файл `/etc/fail2ban/jail.d/wordpress.conf` и добавим следующее:

`[wordpress]`

`enabled = true`

`filter = wordpress`

`logpath = /var/log/auth.log`

`port = http,https`

`maxretry = 3`

`findtime = 10800`

`bantime = 86400`

После сохранения необходимо перезапустить сервис fail2ban.

```
service fail2ban restart
```

## Проверка работы

После того, как всё будет сделано необходимо совершить неправильную авторизацию, после которой в файле `/var/log/auth.log` должна появиться запись, например:

```
Jan 19 14:17:17 container2 wordpress(mydomain.com)[12576]: Authentication failure for vanzhiganov from xxx.xxx.xxx.xxx
```

После нескольких неудачных попыток (значение `maxretry`) авторизации fail2ban должен будет заблокировать IP и сделать запись в журнале `/var/log/fail2ban.log`:

```
2016-01-19 15:03:38,696 fail2ban.actions: WARNING [wordpress] Ban xxx.xxx.xxx.xxx
```

Если записи появляются, то всё настроено верно и работает и ваш wordpress блог защищён от подбора пароля.
