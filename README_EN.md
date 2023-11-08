# Docker Compose WebServer Nginx version

Environment used (confirmed)

| Software | Version |
|:--|:--|
| Docker | 24.0.7 build afdd53b
| Ubuntu | Ubuntu 22.04.3 LTS x86_64 |

## 1. overview

Build a web server using Docker Compose.  
It's really easy.

## 2. configuration

The environment used is as follows.

| Software | Version |
|:--|:--|
| Nginx | 1.25.2 |
| PHP | 8.2.11-fpm |
| MariaDB | 11.1.2 |
| phpMyAdmin | latest |

## 3. how to use

### 3.1. Cloning a Repository

```bash
$ git clone https://github.com/waya0125/web-server.git
````

### Writing ``.env`'' files.

``bash
$ cd web-server
$ cp .env.example .env
$ nano .env # You can also use vim. I'm a nano guy.
The contents of the ```.

The contents of the ``.env`` file are as follows.

````bash
# .env
MYSQL_HOSTNAME=mariadb # Name written in Service name
MYSQL_DATABASE=database # initial database name to create, optional
MYSQL_USER=username # Initial user name to create optional
MYSQL_PASSWORD=password # Password for the initially created user optional
MYSQL_ROOT_PASSWORD=root_password # Password for the root user, but you can also use Random.
MYSQL_TCP_PORT=3310 # TCP port, you don't have to change it if you don't want to.
PHP_MEMORY_LIMIT=512M # PHP memory limit.
PMA_TCP_PORT=8001 # TCP port for phpMyAdmin You don't have to change it if you don't need to.
TZ=Azia/Tokyo # Time zone, change if necessary.
````

### Startup.

```bash
$ cd web-server
$ docker-compose up -d
```

### Checking if it works

Access `localhost:8000` with a browser.  
If you have changed the port, rewrite `8000` to access `localhost:8000`.  
If you see ``Hello World!

### 3.5. phpMyAdmin

Access `localhost:8001` with a browser.  
If you have changed the port, rewrite `8001` to access  
If phpMyAdmin is displayed, success.

## 4. other

### 4.1. database persistence

In `docker-compose.yml`, in the `mariadb` section, there is the following description.

```yml
# docker-compose.yml
volumes:
  # Volume mount
  - ./mariadb_data:/var/lib/mysql
```

This means to create a directory `. /mariadb_data` directory and store the database data in it.  
If you delete this directory, the database data will also be deleted.  
As long as this directory is not deleted, the database data will be persistent.

If you cannot start it, check if the folder has not been created or if you do not have permissions.

### 4.2. About phpMyAdmin

Usage is basically the same as normal phpMyAdmin.  
The database data is persistent, so any changes you make here will be reflected on the web server.

### 4.3. About versions

You can change the version by changing the `image` entry in `docker-compose.yml`.  
The reason why the version is explicitly specified is to explicitly specify the version as of the time when the operation is guaranteed.  
You can change this version to the latest version when you use it. 4.4.

### 4.4. About ports

You can change the port by changing the `ports` entry in `docker-compose.yml`.  
Since we are using Cloudflared in our environment, we do not use the standard 80 or 443.

## 5. Conclusion

If you have any questions, please send me an Issue or Pull Request.  
Twitter or DM on Misskey is fine.  
See the contact information on my profile when you send it.