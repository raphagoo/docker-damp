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
    mysql57:
        ports:
        - "13306:3306"

    php74:
        depends_on:
        - mysql57
```

```php
// my PHP
$cnx = new PDO("mysql:host=mysql57;port=3306", "cnx-name", "cnx-pwd");
```

## Using xDebug on VScode

* PHP must be installed on host
* Add a debug section in `.vscode/launch.json`;

  `pathMappings` is the most important property

  ```json
  {
      "name": "Docker XDebug",
      "type": "php",
      "request": "launch",
      "port": 9003,
      "pathMappings": {
          "/var/www/dev/nissan/gembakaizen/api": "${workspaceFolder}"
      },
      "xdebugSettings": {
          "max_data": 65535,
          "show_hidden": 1,
          "max_children": 100,
          "max_depth": 5
      }
  },
  ```

## Apache configuration

Aliases should be defined in `httpd/2.4/conf.d/alias.conf`, refering to `/var/www/...`

## MySql tweaks

```sql
SELECT @@sql_mode
```

gives: `ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION`

## Adding further php image

It's possible to run as many php version as needed.
NOTE that adding or removing a service can make ophans.
Once the `docker-compose.yml` is changed, it can be useful build the material, using the following command:

* `docker-compose up --build -d --remove-orphans`

## Useful commands

NOTE: shell on ubuntu based image is `bash`, on alpine based image is `/bin/sh`

* `docker-compose config` *validate composer file (using .env)*
* `docker-compose up --build -d` *create + start, with build, and detach*
* `docker-compose down` *stop + remove*
* `docker-compose start`
* `docker-compose stop`
* `docker exec -ti php72_apache <shell command>` *execute interactively through TTY inside the container `php72_apache` the given command*
* `docker logs -f  mysql_57`
* `docker exec -ti -w /var/www/chemin_working_dir/ php74 command` *exécution d'une commande, par ex: composer*

## Useful links

* <https://docs.microsoft.com/fr-fr/virtualization/windowscontainers/manage-docker/configure-docker-daemon>

## License

This material comes under [license MIT](./LICENSE).
