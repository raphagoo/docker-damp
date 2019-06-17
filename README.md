# Lamp stack

pour le dev

## Quick start

```bash
# build & start:
docker-compose up -d

# stop and remove all
docker-compose down

```

## Useful commands

* `docker-compose up --build -d` *create + start, with build, and detach*
* `docker-compose down` *stop + remove*
* `docker-compose start`
* `docker-compose stop`
* `docker exec -ti php72_apache bash` *execute interactively through TTY inside the container `php72_apache` the command `bash`*
