# Docker Mysql server

The following code base is a docker container for a singular mysql database server designed to be shared with other containers.

## getting started

this build features variables which are stored within an env file. Env files are not uploaded due to the fact they hold sensitive data like passwords, a env.example file is uploaded please rename this to .env in order to use

enter the required features you wish to apply to the database.

to get your required version of mysql database at  <a href="hub.docker.com" target="_new">hub.docker.com</a>  


## requirements.

this will work standalone or can be modified to be stacked with other container applications

simply locate the docker file and enter the following command
```
docker-compose up -d
```

Need to recreate it ?

```
docker-compose up -d --force-recreate
```

if you wish to use this with other containers you need to create a seperate network called mysql

```
docker network create mysql
```

under your other docker-compose files you will need to add the mysql network to your contaner inside the services section

and again 
after the services section you need to define the network is being used as follows

```
networks:
  mysql: 
    external:
      name: mysql
```

this will allow the container to comunicate outside with other containers using the same network connection.

This build has had a volume applied in order to store the data within a volume rather than folder structure of the container, this is optional and can be changed  by simply changing your volume path to a local storage if needed

if the volume is used you are required to instantiate the volume by using the following command

```
docker volume create mysql_data
```

## Optional

if you wish to change your datbase simply change the following

```
image: mysql:${mysql_version}
```

check <a href="hub.docker.com" target="_new">hub.docker.com</a> for all the databases you can use.

Please feel free to change the service name amd container name if you wish

<hr>
### ! note this has only been tested on mysql and mariadb other results may be different.

### !if you choose to swap the database type you will need to create a new volume or storage path, if this is not done the datase container will just go in a restart loop

<hr>

the network name can be changed if you wish but it is only a naming convention leaving it will not cause issues to the container.


if you wish to use this as is with the docker web server please <a href="http://github.com/Dealta-Complex/Docker-web" target="_new">click here</a>


<input type="checkbox" checked/> Mysql 8.0,28 has been tested and confirmed to work.