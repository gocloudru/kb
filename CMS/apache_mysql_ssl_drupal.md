## Установка Drupal 8 с Apache, MySQL и SSL

### Шаг1: Установка Apache и PHP

`sudo su`

`apt-get update`

`apt-get install apache2 -y`

`apt-get install -y php5 libapache2-mod-php5 php5-mysqlnd php5-curl php5-gd php5-intl php-pear php5-imagick php5-imap php5-mcrypt php5-memcache php5-ming php5-ps php5-pspell php5-recode php5-snmp php5-sqlite php5-tidy php5-xmlrpc php5-xsl php5-apcu`

`a2enmod rewrite ssl`

`systemctl restart apache2`
