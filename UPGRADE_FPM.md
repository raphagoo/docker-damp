# Passage à la version PHP-FPM

## Images Debian ou Alpine

Les images utilisées sont basées sur 2 distributions: Debian et Alpine.

La distribution Alpine, est sensée être plus légère (environ 1/3, donc 5Mo au lieu de 15) et orientée sécurité.

Mais, pour du PHP avec extensions, la donne change: 450Mo contre 600Mo

D'autre part, la reconstruction d'une image basée sur Alpine prend plus de temps, car les extensions sont recompilées à chaque étape

Donc la version Alpine doit être réservée aux images ne nécessitant pas de customisation, comme Apache ou MySql.

Il y a des différences notables dans les applications disponibles ou l'emplacement des fichiers de configurations, par ex:

* configuration d'Apache:
  * Ubuntu: `/etc/apache2/apache2.conf`
  * Alpine: `/usr/local/apache2/conf/httpd.conf`
* shell
  * Ubuntu: `bash`
  * Alpine: `/bin/sh`
* etc.

## Changements

* `/home/docker` remplacé par `/var/www` (bonne pratique), impacte:
  * les aliases du service Apache
  * la configuration xDebug dans vsCode
* configuration d'Apache dans `httpd/2.4/conf.d`
* le nom du container BDD a changé (`mysql57` au lieu de `db`), afin de pouvoir utiliser d'autres versions
* le serveur Apache utilise par défaut le port `80`, qu'il est possible d'ajuster dans le `docker-compose.yml`.
* xDebug utilise le port `9003`, port par défaut à partir de la version 3

## Choix de la version PHP

Le choix de la version PHP se fait dans le ```.htaccess``` du projet:

```apache
<FilesMatch \.php$>
    SetHandler "proxy:fcgi://php74:9000"
</FilesMatch>
```

## Transition

Les 2 pools de services (networks) sont disponibles pendant une phase de transition.

Le réglage du `.htaccess` couplé au choix du port permet de passer d'utiliser l'un ou l'autre des 2 réseaux.

Les configurations xDebug peuvent cohabiter dans vsCode
