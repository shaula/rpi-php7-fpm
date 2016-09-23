# PHP7 Docker image for Raspberry Pi

## Why?

Because some PHP7 packages are not available from repositories like php-redis or php-memcached.

## How?

I took the official docker file from [DockerHub](https://hub.docker.com/_/php/) as a base.

The following adjustments were necessary:

* the ```FROM``` was changed from ```debian:jessie``` to ```resin/rpi-raspbian```.

## PHP Extensions

The following extensions are already bundled:

* Core
* ctype
* curl
* date
* dom
* fileinfo
* filter
* ftp
* hash
* iconv
* json
* libxml
* mbstring
* mysqlnd
* openssl
* pcre
* PDO
* pdo_sqlite
* Phar
* posix
* readline
* Reflection
* session
* SimpleXML
* SPL
* sqlite3
* standard
* tokenizer
* xml
* xmlreader
* xmlwriter
* zlib

Possible values for ```ext-name``` are:
bcmath
bz2
calendar
ctype
curl
dba
dom
enchant
exif
fileinfo
filter
ftp
gd
gettext
gmp
hash
iconv
imap
interbase
intl
json
ldap
mbstring
mcrypt
mysqli
oci8
odbc
opcache
pcntl
pdo
pdo_dblib
pdo_firebird
pdo_mysql
pdo_oci
pdo_odbc
pdo_pgsql
pdo_sqlite
pgsql
phar
posix
pspell
readline
recode
reflection
session
shmop
simplexml
snmp
soap
sockets
spl
standard
sysvmsg
sysvsem
sysvshm
tidy
tokenizer
wddx
xml
xmlreader
xmlrpc
xmlwriter
xsl
zip

## Usage

This ```Dockerfile``` shows how to build upon this image:
```
FROM shaula/rpi-php7-fpm

# install PHP extensions & PECL modules with dependencies
RUN apt-get update \
 && apt-get install -y bzip2 git vim wget \
        libxslt1.1 libxslt1-dev \
        libicu-dev \
        libmcrypt-dev \
        libfreetype6-dev libjpeg62-turbo-dev libpng12-dev \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*

RUN docker-php-ext-install gd \
 && docker-php-ext-install intl \
 && docker-php-ext-install mcrypt \
 && docker-php-ext-install opcache \
 && docker-php-ext-install pdo_mysql mysqli \
 && docker-php-ext-install xsl \
 && docker-php-ext-install zip \
 && pecl install redis && docker-php-ext-enable redis
```