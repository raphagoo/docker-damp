# Lamp stack

pour le dev

## Quick start

```bash
# install docker (as admin on windows):
choco install docker -y
choco install docker-compose -y

# touch env file
cp template.env .env
code .env

# touch apache config
cp apache/apache.template.conf apache/conf/apache.conf
code apache/conf/apache.conf

# build & start:
docker-compose up -d
```

## Inside php

Db connexion should use service name and local port:

```yaml
# docker-compose.yml
services:
    db:
        ports:
        - "13306:3306"

    web:
        depends_on:
        - db
```

```php
// my PHP
$cnx = new PDO("mysql:host=db;port=3306", "cnx-name", "cnx-pwd");
```

## Using xDebug on VScode

* PHP must be installed on host
* Add a debug section in `.vscode/launch.json`;<br>
  `pathMappings` is the most important property
  ```json
  {
      "name": "Docker XDebug",
      "type": "php",
      "request": "launch",
      "port": 9000,
      "pathMappings": {
          "/home/docker/dev/nissan/gembakaizen/api": "${workspaceFolder}"
      },
      "xdebugSettings": {
          "max_data": 65535,
          "show_hidden": 1,
          "max_children": 100,
          "max_depth": 5
      }
  },
  ```

## MySql tweaks

```sql
SELECT @@sql_mode
```
gives: `ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION`

## Useful commands

* `docker-compose config` *validate composer file (using .env)*
* `docker-compose up --build -d` *create + start, with build, and detach*
* `docker-compose down` *stop + remove*
* `docker-compose start`
* `docker-compose stop`
* `docker exec -ti php72_apache bash` *execute interactively through TTY inside the container `php72_apache` the command `bash`*
* `docker logs -f  mysql_57`

## Useful links

* https://docs.microsoft.com/fr-fr/virtualization/windowscontainers/manage-docker/configure-docker-daemon
