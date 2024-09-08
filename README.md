<div style="text-align: center;">

<img src="https://lh4.googleusercontent.com/proxy/TYEapJ_0zp4AE90WAgWNTUQk5JS-WsX4L531MR8f0uyLCfdP3h6a1f4TEOMnSmHpeqZZzGzYq4C8BwAhA3TgZzBTTBWHVfo9kbp0DKuIwpclIvfVZZEG3b8RikCKjMHt5RPKigIk0g" alt="Image" style="max-width: 100%; height: auto;">

</div>



# Docker Setup for MariaDB

This is a basic **MariaDB** setup for Docker using Docker Compose. It creates a database service with persistent storage using volumes, connected to an external network for use with other containers like web applications or reverse proxies.

## Prerequisites

- Docker and Docker Compose must be installed.
- The external Docker network `webnet-network` must already exist.
- Create one if needed (webnet-network is a generic name)
```bash
docker network create webnet-network
```

## Docker Compose File

```yaml
version: '3.8'

services:
  database:
    image: mariadb:latest
    container_name: MySQL-Database
    restart: unless-stopped
    environment:
      MYSQL_ROOT_PASSWORD: <ROOT_PASSWORD>
      MYSQL_DATABASE: <DB_NAME>
      MYSQL_USER: <DB_USER>
      MYSQL_PASSWORD: <DB_PASSWORD>
    volumes:
      - db_data:/var/lib/mysql
    networks:
      - webnet-network

volumes:
  db_data:

networks:
  webnet-network:
    external: true
```

## Environment Variables 

**`MYSQL_ROOT_PASSWORD:`** Set the root password for your database. \
**`MYSQL_DATABASE:`** Name of the default database to create. \
**`MYSQL_USER:`** Non-root user to be created with access to the database. \
**`MYSQL_PASSWORD:`** Password for the non-root user.

## Steps to Deploy

Clone the repository or copy the above YAML code into a `docker-compose.yml` file.

Replace the environment variable placeholders (`<ROOT_PASSWORD>`, `<DB_NAME>`, `<DB_USER>`, `<DB_PASSWORD>`) with your own secure values.

Make sure the external Docker network `webnet-network` is created by running:
```bash
docker network create webnet-network
```
Start the container:
```
docker-compose up -d
```
Verify the container is running with:
```
docker ps
```

## Networking
The `webnet-network` is an external network that should already exist, allowing your database to communicate with other services (like web applications or reverse proxies).


