# docker-help
This repo contains what I need for running docker in windows 

# mysql
```python
services:
  mysqldb:
    image: mysql:8.0
    container_name: mysqlcontainer
    command: --default-authentication-plugin=mysql_native_password
    restart: unless-stopped
    volumes:
      - ./dbinit/init.sql:/docker-entrypoint-initdb.d/0_init.sql
      - D:/database:/var/lib/mysql
    ports:
      - 3306:3306
    expose:
      - 3306
    environment:
      - MYSQL_DATABASE=patientdb
      - MYSQL_USER=admin
      - MYSQL_PASSWORD=root
      - MYSQL_ROOT_PASSWORD=root
      - SERVICE_TAGS=dev
      - SERVICE_NAME=mysqldb
    networks:
      - internalnet

networks:
  internalnet:
    driver: bridge
```

# MySQL Docker Compose Configuration

## Services

### `mysqldb`

- **Image:** `mysql:8.0`
- **Container Name:** `mysqlcontainer`
- **Command:** `--default-authentication-plugin=mysql_native_password`
- **Restart Policy:** `unless-stopped`

#### Volumes

- `./dbinit/init.sql:/docker-entrypoint-initdb.d/0_init.sql`: Mounts the local `./dbinit/init.sql` file into the container's `/docker-entrypoint-initdb.d/` directory. This file is executed during MySQL initialization.
- `D:/database:/var/lib/mysql`: Mounts the local `D:/database` directory to the MySQL data directory for persistent storage.

#### Ports

- Maps port `3306` from the host to port `3306` inside the container, allowing external access to the MySQL service.

#### Expose

- Exposes port `3306` to other services on the same network.

#### Environment Variables

- `MYSQL_DATABASE`: Specifies the name of the initial database to be created.
- `MYSQL_USER`: Sets the MySQL user for the database.
- `MYSQL_PASSWORD`: Sets the password for the MySQL user.
- `MYSQL_ROOT_PASSWORD`: Sets the root password for MySQL.
- `SERVICE_TAGS`: Adds a development tag for the service.
- `SERVICE_NAME`: Sets the service name to "mysqldb."

#### Networks

- **Network:** `internalnet`
- **Driver:** `bridge`

---


This Docker Compose configuration sets up a MySQL service using the official MySQL Docker image (version 8.0). The service is configured with persistent storage, port mapping, environment variables, and network settings.

