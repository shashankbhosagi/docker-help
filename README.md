# Docker-help
- This repo contains what I need for running docker on my computer.
- Note : I have windows so below configuration files may contain OS specific stuff, you need to tweak the configuration if you want to use it on Ubuntu/MacOS
- Note : I just got to know that we can use environment variables in `docker-compose.yml`, means in the volumes instead of directly mentioning `D:/database`, you can use a env variable there, and while running it on your system you can provide where you want to store the MySQL data directory for persistent storage.

# mysql (docker-compose.yml)
```python
services:
  mysqldb:
    image: mysql:8.0
    container_name: container_name
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
      - MYSQL_DATABASE=db_name
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

# Running MySQL Docker Compose Configuration

Follow these steps to run the MySQL Docker Compose configuration:

## 1. Create a Docker Compose File

Save the Docker Compose configuration in a file named `docker-compose.yml`. Use a text editor to create this file and copy the content into it.

## 2. Navigate to the Directory

Open a terminal or command prompt, and navigate to the directory where your `docker-compose.yml` file is located.

```bash
cd /path/to/your/docker-compose-directory
```

## 3. Run Docker Compose
Execute the following command to start the MySQL service:

```bash
docker-compose up -d
```

This command will download the MySQL image (if not already downloaded) and create a container based on your configuration.


## 4. Check Container Status
Check the status of the running MySQL container using the following command:
```bash
docker ps
```
You should see the MySQL container (`mysqlcontainer`) in the list with the status "Up."

![image](https://github.com/shashankbhosagi/docker-help/assets/78866224/1e54fb66-4643-4d56-87c1-719106f857ab)


## 5. Access MySQL Shell (Optional)
If you want to access the MySQL shell within the container, use the following command:

```bash
docker exec -it mysqlcontainer mysql -u root -p
```
Enter the root password when prompted.

![image](https://github.com/shashankbhosagi/docker-help/assets/78866224/cefe829a-8760-426b-b5b7-ba195d42b95b)



## 6. Stop and Remove the Container (Optional)
To stop and remove the MySQL container, use the following command:

```bash
docker-compose down
```
This command will stop and remove the containers defined in the `docker-compose.yml` file.












