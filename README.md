## Docker-Nodejs-ghost
### The Ghost deployment has three components:
* The Ghost service
* Database (MySQL) that will store your blog posts
* A web server (NGINX) that will proxy requests on HTTP and HTTPS to your Ghost service. 

These services are listed in a single Docker Compose file.This guide uses sudo wherever possible.

### Install Docker & Docker-Compose
The instructions assume that you have already installed [Docker](https://docs.docker.com/installation/) and [Docker Compose](https://docs.docker.com/compose/install/). 

### How to Start Docker-Nodejs-ghost
1) ##### Clone this project onto your Docker Host.

```
git clone https://github.com/kirtiazad11111/Docker-Nodejs.git . 
```
    
2) ##### Change the directory 

```
cd Docker-Nodesjs
```
  
3) ##### Update new database password where `your_database_root_password appears` in `docker-compose.yml`. The values for `database__connection__password` and `MYSQL_ROOT_PASSWORD` should be the same:
``` yaml
version: '3.1'
services:
  ghost:
    image: ghost:1-alpine
    container_name: ghost
    restart: always
    volumes:
      - /opt/ghost_content:/var/lib/ghost/content
    environment:
      database__client: mysql
      database__connection__host: db
      database__connection__user: root
      database__connection__password: your_database_root_password
      database__connection__database: ghost
    depends_on:
      - db
    networks:
      - app-network
  db:
    image: mysql:5.7
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: your_database_root_password
    volumes:
      - /opt/ghost_mysql:/var/lib/mysql
    networks:
      - app-network
  nginx:
    build:
      context: ./nginx
      dockerfile: Dockerfile
    restart: always
    depends_on:
      - ghost
    ports:
      - "80:80"
      - "443:443"
    volumes:
       - /opt/ssl/:/etc/ssl/certs/
    networks:
      - app-network
networks:
  app-network:
    driver: bridge
  ```
4) ##### The Docker Compose file creates a few Docker bind mounts:
    * `/var/lib/ghost/content` and `/var/lib/mysql` inside your containers are mapped to `/opt/ghost_content` and `/opt/ghost_mysql`. These locations store your Ghost content.
    * NGINX uses a bind mount for `/etc/ssl/` to access your self signed certificate.
    * Create directories for those bind mounts
```
sudo mkdir /opt/ghost_content
sudo mkdir /opt/ghost_mysql
sudo mkdir -p /opt/ssl/
```
5) ##### Create self sign certificate for Nginx 
```
openssl req -subj '/CN=localhost' -x509 -newkey rsa:4096 -nodes -keyout /opt/ssl/key.pem -out /opt/ssl/cert.pem -days 365
```
6) ##### Create dhparam certificate 
```
sudo openssl dhparam -out /opt/ssl/dhparam-2048.pem 2048
````
7) ##### Run the following commands to start docker-compose.yml
```
docker-compose up -d
```

8) #####  The  docker-compose command will pull the images from Docker Hub and then link them together based on the information inside the docker-compose.yml file. This will create ports, links between containers, and configure applications as requrired. After the command completes we can now view the status of our stack
```
docker-compose ps
```
