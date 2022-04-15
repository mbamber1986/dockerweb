# APache and php server for docker


## What is this ?

the following package is a WIP Docker Based webserver with Support for php-fpm.

All docker files are included within this package and you are more than welcome to update and modify these to your own needs.

Please be aware that this is uploaded as working is recommended for development and not production servers unless stated otherwise.

## Whats included ? 

Apache-alpine
php-alpine

the image versions can be changed and controlled using the .env file which bundled within this repository, we have currentlt set this to apache 2.4 and php 8.0 with these being the latest

## The Env file

```
site_name =
domain = 
root= 
www= 

PHP_VERSION=8.0
PHP_NAME=php-

APACHE_VERSION=2.4
APACHE_NAME=Apache

```
* the site name is a user friendly name mainly used for the dockers container  in place of container_name within the docker compose file.

* the domain is used to pass the website name to apaches vhost.

* Root is used to tell both docker cli and the docker compose file where you working directory starts

* www is used to tell both apache and the docker file where your website index file is being held e.g. /var/www/html/public_html

### Please note it is a good idea to keep your root and www on the same file path e.g /var/www/html as (root) and /var/www/html/public_html as your (www) path.


## Installation

To install as is , cd into the  directory root (where the docker compose file is) and enter the following syntax

``` 
1 : docker network create web

2 : docker-compose up -d --build

```

A network is required in order to give the container an ip by default we have named ours web but you are free to change this within the docker comopose file.

## Advanced Customisation

* ### Adding Mysql
This package was designed to run off a seperate mysql Database container, but also realised that option is not for everyone and did bundle the options to support a mysql database simply add the following snippet of code to the docker-compose.yml file.

```
  mysql:
    image: ${db_type}:${db_version}
    restart: always
    volumes:
      - ${db_local_storage_path}:/var/lib/mysql
    networks:
      - mysql
    ports:
      - ${db_port}
    environment:
      MYSQL_ROOT_PASSWORD: "${db_root_password}"
    container_name: ${db_name}
  phpmyadmin:
    image: phpmyadmin
    environment:
      PMA_HOST: mysql
    ports:
      - ${pma_port}
    networks:
      - mysql
    restart: always
    container_name: phpmyadmin
```

Additionally the  following variables will need to be added to your .env file

```
db_name=
db_type=
db_version=
db_local_storage_path
db_root_password

db_port = "3306:3306"
pma_port = "8080:80"
```

* db_name : this is the user friendly name you will give your docker container
* db_type : this will be the image name mysql, maria db etc refer to docker hub for images and there names 
* db_version: this is the version you wish to use :latest is possibly the easier option but not alwways the best for your own use.
* db_local_storagae_path: this is the folder where mysql will store its data on the host to sync with the container e.g. ./data/mysql.

* ## support with traefik

this package can be optimised with traefik's routing repo for more information about traefik <a href='http://traefik.io' target="_new">Click here</a>

this allows the ability of multiple containers sharing port 80 without any conflicts.

copy the following code into your apache stack and remove any ports you have applied

```
labels:
    - "traefik.enable=true"
    - "traefik.http.routers.${site_name}_web.rule=Host(`${domain}`)"
    - "traefik.http.routers.${site_name}_web.entrypoints=web"
    - "traefik.http.routers.${site_name}_websecure.rule=Host(`${domain}`)"
    - "traefik.http.routers.${site_name}_websecure.entrypoints=websecure"
    - "traefik.http.routers.${site_name}_websecure.tls.certresolver=myresolver"
    - "traefik.docker.network=web"
```
as mentioned before the use of ${site_name} and ${domain} are used to give the container a unique name and the correct domain name in order for the  traefik router to function correctly.
