# Centreon-Docker

This is a docker for Centreon. How to install a Centreon in 5 minutes ?

## Start a centreon central server instance

Write your docker-compose.yml

```yml
version: '3'
services: 
  centreon-central:
    build: ./centreon-central/
    image: kbeaugrand/centreon-central:latest
    container_name: centreon-central
    privileged: true
    environment:
      - PHP_DATE_TIMEZONE=Europe/Paris
    links: 
      - db:db
    ports: 
      - "8080:80"
    privileged: true
    volumes:
      - /sys/fs/cgroup:/sys/fs/cgroup:ro
      - centreon-config:/etc/centreon
      
  db:
    build: ./centreon-db/
    image: kbeaugrand/centreon-db:latest
    container_name: centreon-db
    environment: 
      - MYSQL_ROOT_PASSWORD=my-secret-pw
    volumes:
      - db-data:/var/lib/mysql

volumes: 
  db-data:
  centreon-config:
```

Start your stack

```sh
docker-compose up -d
```

## Variables

### `PHP_DATE_TIMEZONE`

The default timezone used by all date/time functions. Prior to PHP 5.4.0, this would only work if the TZ environment variable was not set. The precedence order for which timezone is used if none is explicitly mentioned is described in the date_default_timezone_get() page. See List of Supported Timezones for a list of supported timezones.

## Need persistant data

Important note: There are several ways to store data used by applications that run in Docker containers. We encourage users to familiarize themselves with the options available.

For more informations please go to : [https://docs.docker.com/storage/](https://docs.docker.com/storage/)

### DB persistant data

You can map the following mounting point to your docker volume or your file system mounting point:

`/var/lib/mysql`

### Centreon's configuration files

You can map the following mounting points to your docker volumes or your file system mounting points:

* `/etc/centreon`
