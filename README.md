# Setup

First create a www directory in the root of this project (where .lando.yml lives)

```bash
 mkdir www
 # and put your laravel project there, public dir should be at /www/public
```

Run Lando:

```bash
 lando start
```

.lando.yml:

```bash
name: lando-lamp-laravel
recipe: lamp
config:
  webroot: app/public # lando needs to be able to resolve the dir to generate appserver urls, use a basic temp file (app/pubic/index.php) if app is not ready
proxy:
  appserver:
    - lando-lamp-laravel.lndo.site
services:
  appserver:
    config: {}
    ssl: true
    type: 'php:8.1'
    via: apache
    xdebug: false
    webroot: app/public
  database:
    config: {}
    authentication: mysql_native_password
    type: 'mysql:5.7'
    portforward: true
    creds:
      user: lamp
      password: lamp
      database: lamp
tooling:
  composer:
    service: appserver
    cmd: composer --ansi
  db-import <file>:
    service: ':host'
    description: Imports a dump file into a database service
    cmd: /helpers/sql-import.sh
    user: root
    options:
      host:
        description: The database service to use
        default: database
        alias:
          - h
      no-wipe:
        description: Do not destroy the existing database before an import
        boolean: true
  'db-export [file]':
    service: ':host'
    description: Exports database from a database service to a file
    cmd: /helpers/sql-export.sh
    user: root
    options:
      host:
        description: The database service to use
        default: database
        alias:
          - h
      stdout:
        description: Dump database to stdout
  php:
    service: appserver
    cmd: php
  mysql:
    service: ':host'
    description: Drops into a MySQL shell on a database service
    cmd: mysql -uroot
    options:
      host:
        description: The database service to use
        default: database
        alias:
          - h
```

Migrate the default Laravel database:

```bash
lando artisan migrate
```

To rebuild:

```bash
lando rebuild -y
```

Environment info:

```bash
lando info
lando status
```

Help:

```bash
lando --help
```

Clear and rebuild config path:

```bash
php artisan config:clear
php artisan config:cache

```

Clear other caches:

```bash
php artisan route:clear
php artisan view:clear
php artisan cache:clear
```

Full reset:

```bash
php artisan config:clear
php artisan cache:clear
php artisan route:clear
php artisan view:clear
php artisan config:cache

```
