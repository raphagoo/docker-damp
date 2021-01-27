# Multiples services BDD

Pour faire tourner plusieurs versions de mySql ou mariaDB, quelques adaptaions sont nécessaires.

## adaptations du service mysql57

* modification de la variable d'environnement `DB_VOLUME` (`/chemin_fichiers_bdd/mysql_57` remplacé par `/chemin_fichiers_bdd`)
* modification du volume des données (`${DB_VOLUME}` remplacé par `${DB_VOLUME}/mysql_57`)
* modification du port public mysql (13306 remplacé par 3357)

## ajout du service mariadb10

* ajout du service mariadb sur le port 3410
* ajout des dépendances au service mariadb10
