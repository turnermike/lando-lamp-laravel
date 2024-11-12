# Setup

First create a docroot directory in the root of this project (where .lando.yml lives)

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
name: lando-laravel-mysql
recipe: laravel
config:
  webroot: www/public

services:
  phpmyadminserver:
    type: phpmyadmin
    hosts:
      - database
    ssl: true
  database:
    type: mysql:5.7
    portforward: true
  appserver:                                                          # PHP service
    type: php:8.1
    webroot: www/public
    via: apache
    ssl: true
    xdebug: false
    config:                                                           # Custom config files
      php: config/php.ini
  node:                                                               # For NPM commands
    type: node

proxy:
  phpmyadminserver:
    - pma.laravel.lndo.site

tooling:                                                              # lando artisan commands
  php:                                                                # lando php artisan cache:clear
    service: appserver
  composer:                                                           # lando composer install
    service: appserver
  npm:                                                                # lando npm run dev (to build laravel js)
    service: node
  node:
    service: node
  gulp:
    service: node
  yarn:
    service: node

bindAddress: '0.0.0.0'
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
