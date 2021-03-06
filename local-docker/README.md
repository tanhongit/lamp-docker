# Welcome to Local Docker Config

This is a simple Docker Compose workflow that sets up a LAMP network of containers for local

## I. Configuration requirements

To use the fpm image, you need an additional web server, such as apache, that can proxy http-request to the fpm-port of the container. For fpm connection this container exposes port 9000.

 - Web-server: Apache
 - DBMS (database management system): mariadb
 - SSL Certificate (using mkcert)
 
## II. Steps

### 1. Install ssl certificate
Using mkcert to create ssl certificate

#### For Ubuntu

```shell
sudo apt install libnss3-tools

sudo wget https://github.com/FiloSottile/mkcert/releases/download/v1.4.3/mkcert-v1.4.3-linux-amd64 && \
sudo mv mkcert-v1.4.3-linux-amd64 mkcert && \
sudo chmod +x mkcert && \
sudo cp mkcert /usr/local/bin/
```

Now that the mkcert utility is installed, run the command below to generate and install your local CA:

```shell
mkcert -install
```

### 2. Create ssl certificate for this project

Run:

```shell
cd docker/server/certs
mkcert local.localhost.com
```

### 3. Run to setup: 

```shell
docker-compose up -d
```

## III. Check the network ID and connect Database

### 1. Check CONTAINER ID
- Run `docker ps` to check the Container ID of **APP_NAME-db**
- Run the command `docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' <container_ID>`

```shell
docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}} <container_ID>
```

![image](https://imgur.com/eXqHQVb.png)
