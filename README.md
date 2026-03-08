## Setup the project Docker-Mappix

Clone this repository

```bash
git clone https://github.com/olirati/docker-mappix.git
```
Jump inside directory

```bash
cd docker-mappix
```

Restore submodules

```bash
git submodule update --init --recursive
```

Build and run the Docker stack

```bash
docker compose up -build
```

Later run it with 
```bash
docker compose up
```

And stop it with

```bash
docker compose down
```

### Setup initial database for application

Enter inside the php container

'''bash
docker-compose exec php bash
```

Update composer modules with command

```
composer update
```

And Initialize the Mappix database ( if not already done )

```
# Update access permission for public ( needed to save Overpass cache )
chmod -R 777 public/
#
# Create the databases
php bin/console doctrine:database:create
php bin/console make:migration
php bin/console doctrine:migrations:migrate
#
# Then exit the container
exit
```

### Useful development commands

Start stack:

```
docker compose up -d --build
```

Enter PHP container:

```
docker compose exec php bash
```

Run migrations:

```
php bin/console doctrine:migrations:migrate
```

## project structure

```
project/
│
├─ cert/
│   ├─ localhost.crt
│   └─ localhost.key
│
├─ docker/
│   ├─ nginx/
│   │   └─ default.conf
│   └─ php/
│       └─ Dockerfile
│
├─ docker-compose.yml
└─ mappix/
```

## Docker uses :

### Alpine as certgen

Used to create the certificates if they do not already exists

then nginx + vite starts using the certificates

### Mariadb

Used for database needs

### Nginx

Used to serve web requests with https enabled

### Node

Used for Vite developpement with https enabled

## Access services

### Symfony HTTPS:

```
https://localhost:8443
```

### Vite dev server:

```
https://localhost:5173
```

### phpMyAdmin:

```
http://localhost:8081
```

```
phpMyAdmin login

Server:    database
User:      root
Password:  root
```

## Browser warning

Because the certificate is self-signed, the browser will show a warning the first time. Accept it once and GPS APIs will work.

💡 Advanced improvement (recommended for dev environments)
Instead of raw self-signed certs, you can automatically generate locally trusted certificates using mkcert inside Docker. This avoids browser warnings and makes HTTPS dev much smoother.

If you want, I can show a fully automated mkcert Docker setup where:

certificates are trusted by the host

generated automatically

usable by nginx + vite

perfect for GPS / camera / WebRTC testing.

## Prepare Docker Symfony developper stack

Setup the stack
```bash
docker-compose build --no-cache
```

docker-compose up

docker-compose down

