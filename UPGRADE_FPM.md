# Upgrade pour passer à la version PHP-FPM

Inspiré par les images de [Guillaume Kulakowski](https://blog.kulakowski.fr/post/docker-pour-ma-stack-lamp) et ses [images Docker](https://github.com/llaumgui/docker-images/)

## Images Alpine

Les images utilisées existent en 2 distributions: Ubuntu et Alpine.

Le nouveau jeu d'images est basé sur la distribution Alpine, plus légère et orientée sécurité.

Alpine est une distribution Linux beaucoup plus légère qu'Ubuntu (environ 1/3, donc 5Mo au lieu de 15).

Il y a des différences notables dans les applications disponibles ou l'emplacement des fichiers de configurations, par ex:

* configuration d'Apache:
  * Ubuntu: `/etc/apache2/apache2.conf`
  * Alpine: `/usr/local/apache2/conf/httpd.conf`
* shell
  * Ubuntu: `bash`
  * Alpine: `/bin/sh`
* etc.

## Changements

* `/home/docker` remplacé par `/var/www` (bonne pratique)
