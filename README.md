# Lamp stack

pour le dev

## Quick start

```bash
# build & start:
docker-compose up -d

# stop and remove all
docker-compose down

```

## Exec

In order to run a command directly in docker machine, just do:

```bash
docker exec -ti php72_apache ls -al /var/log/apache2
```
