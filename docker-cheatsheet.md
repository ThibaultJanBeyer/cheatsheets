[back to overwiev](/../..)

# Docker Cheatsheet

## Table of Contents

- [Install](#install)
- - …
- [Basics](#basics)
- - …
- [Containers](#containers)
- - …
- [Container Volumes](#container-columes)
- [Container Communication](#container-communication)
- - …
- [Processes](#processes)
- [Images](#images)
- [Custom Images](#custom-images)
- - …
- [Docker Compose](#docker-compose-1)
- - …
- [Swarm](#swarm)
- - …
- [Services](#services)
- - …
- [Rolling updates](#rolling-updates)
- [Useful](#useful)
- - [Create an ubuntu dev-environement in seconds](#create-an-ubuntu-dev-environement-in-seconds)
- - [SSH into a running container](#ssh-into-a-running-container)
- - [Copy files from Docker container to host](#copy-files-from-Docker-container-to-host)
- [Troubleshooting](#troubleshooting)
- - [Permission denied](#permission-denied)

## Install

Docker: https://www.docker.com/products  
Docker Compose: https://docs.docker.com/compose/install/  
 https://github.com/docker/compose/releases

### Linux

#### Docker

```
wget -qO- https://get.docker.com/ | sh
```

#### Docker-Compose

```
curl -L https://github.com/docker/compose/releases/download/1.23.1/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
```

## Basics

### What are Containers

A _virtual machine_ grabs CPU, MEM, DISKIO, NETWORK, etc. and slices it into virtual variants to build virtual machines that look and feel like real physical servers.

Docker on the other hand, instead of slicing physical resources it slices operating system resources. I.e. Process Namespace, Network Stack, File System. Every container gets its own version of those.

Docker creates more like a virtual operating system that share the same physical resources. Which is way more lightweight than virtual machines.

### Version

```
docker -v
```

Shows Version of client and server.

Docker Client is the interface that takes commands and run the respective API calls to the Deamon.

### Info

```
docker info
```

Shows what is going on on a host. I.e. running containers. Version info.

```
docker node ls
```

lists all docker nodes in a swarm

## Containers

### Run

```
docker run hello-world
```

`hello-world` here is the name of a docker image.

```
docker run -d --name web -p 80:8080 foo/bar
```

`-d`: run in background  
`--name web`: give it a friendly unique name  
`-p 80:8080`: map a port inside the container (here 8080) to a port on the machine (here 80)  
`foo/bar`: get the image named `bar` from user `foo`

```
docker run -it --name temp ubuntu:latest /bin/bash
```

`-it`: in interactive terminal mode (ssh inside the container)  
`/bin/bash`: run bash process (give me a terminal that allows me to interact with that container)  
You usually don’t do that.  
Inside, you can run `ctrl+P+Q` to quit without exiting the process.

### list

```bash
docker ps
docker ps -a
docker ps -aq
...
```

see [Processes](#processes)

### start / stop / rm

```
docker start <container>
docker stop <container>
docker rm <container>
```

`docker rm` completely removes it as opposed to `stop`.

```
docker stop $(docker ps -aq)
```

runs docker stop against the output of `docker ps -aq`:  
`-aq`: all containers in quiet mode (q = just the IDs).

### exec

```
docker exec <id> <command>
docker exec d63 node foo.js
```

Executes a command inside a running docker container

### logs

```
docker logs <id>
```

Outputs the logs of that container

### "SSH" (Bash) into a container

1. Use docker ps to get the name of the existing container.
2. Get a bash shell in the container.

```bash
docker exec -it <container name> /bin/bash
```

_Note: Generically, use `docker exec -it <container name> <command>` to execute whatever command you specify in the container._

## Container Volumes

```
docker run -p 80:8080 -v $(pwd):/var/www node
```

`-v <host-location>:<location>`: create a volume. `<location>` is the container volume (alias inside the container). `<host-location>` is the location on your computer.  
`$(pwd)`: current working directory.

```
docker inspect <container>
```

Will display the source/target location of mounted volumes. (in the `Mounts:` area)

```bash
docker run -p <h-port>:<c-port> -v <h-location>:<c-location> -w "<location>" <image> <command>
# Example: docker run -p 80:8080 -v $(pwd):/var/www -w "/var/www" node npm start
```

`-w "<location>"`: specifies the working directory inside the container (where the command will be run).  
`<command>`: will be run after container creation. Example `npm start`.

```
docker rm -v <container>
```

`-v`: also removes the container that was created/managed by docker (outside of the container). _Note: It will not remove your source code if you specified any._

## Container Communication

Options:  
Use Legacy Linking  
Add containers to Bridge Network (preffered)

### Legacy Linking

Linking containers by name:

1. run container by name
2. link to running container by name
3. repeat for additional containers

4. run container by name

```
docker run -d --name <name> <image>
# Example
docker run -d --name my-postgres postgres
```

`-d`: run in background  
`<name>`: give container a name

2. link to running container by name

```
docker run -d -p <h-port>:<c-port> --link <c-name>:<alias> <username>/<imagename>
# Example
docker run -d -p 80:8080 --link my-postgres:postgres johndoe/foobar
```

`--link`: links the container to a running container where `<c-name>` is the container name and `<alias>` an alias we give the container-link internally.

Like that containers can speak to each other.  
For example: when you give the link an alias as we did in the example above. Inside that second container you can call that first container by calling the alias. Instead of for example `localhost:5000`, you would do `postgres:5000`.

### Container Networks

Allows to isolate containers in groups.

1. Create a bridge network
2. Add containers to that network

3. Create a bridge network

```
docker network create --driver <drivername> <network-name>
docker network create --driver bridge my_isolated_network
```

`--driver`: a network driver type. It can even be cross host. Common is `bridge`.

2. Add containers to that network

```
docker run -d --net=<network-name> --name <name> <image>
docker run -d --net=my_isolated_network --name myMongo mongo
```

`--net=<network-name>`: run container into that specific network  
`--name`: give it a name to link to ("link" to this container by name / = server name to reference)  
_Note: linking === communicating_

Official name of this technique: `Container networking With Bridge Driver`.  
Is used similarely as legacy linking when it comes to code.

#### Inspecting

```
docker network inspect my_isolated_network
```

will list information about the network, including linked containers

## Processes

```
docker ps
```

shows all running Processes

```
docker ps -a
```

shows all processes, running or not, active or exited.

```
docker ps -aq
```

List all processes running and not running but only their IDs (quiet)

## Images

```
docker images
```

See information on all images we have locally.

```
docker pull ubuntu
```

docker pull will just pull an image from https://hub.docker.com but not apply/run it immediately.

```
docker pull ubuntu:14.0.4
```

pulls an image of specific version.

```
docker rmi 4ab4c602aa5e
```

removes an image by ID (repository:tag would also be valid)

```
docker rmi $(docker images -q)
```

removes all images (rocker rmi => docker images -q (= all image ids))

## Custom Images

### Dockerfile

Example Dockerfile:

```dockerfile
FROM        node
MAINTAINER  John Doe

ENV         NODE_ENV=production
ENV         PORT=3000

COPY        . /var/www
WORKDIR     /var/www
VOLUME      ["/foo/bar", "/logs"]

RUN         npm install

EXPOSE      $PORT

ENTRYPOINT  ["node", "server.js"]
```

`FROM`: a base image where it will be build ontop  
`MAINTAINER`: name of the person maintaining that file  
`ENV`: set environment variables for the container  
`COPY`: will bake the code inside the volume layer of that image  
`WORKDIR`: where does it run the code?  
`VOLUME`: attach one or more volumes from the docker host (the one environment that will run the image)  
`RUN`: code to run when building  
`EXPOSE`: ports to expose. `$PORT` is the environment variable set earlier.  
`ENTRYPOINT`: code to run when starting

### Build an image

```
docker build -f <dockerfile> -t <username>/<imagetag> <context>
```

`-t`/`--t`: tag the build (to be found on dockerhost)  
`context`: what context it will be build from (i.e. where the dockerfile is)  
`-f`: where the dockerfile is located (and how it is named)

Usual flow for node:

```
rm -r node_modules
docker build -f Dockerfile -t johndoe/mynode .
```

### Publish image to docker hub

```
docker push <username>/<imagetag>
```

Typical flow:

```
docker login
docker push <username>/<imagetag>
```

Pushes to a public repo on your dockerhub

## Docker Compose

Useful to manage automatically different lifecycles of services.

### File

docker-compose.yml

```yml
version: "2"

services:
  nginx:
    container_name: nginx
    build:
      context: .
      dockerfile: .docker/docker-nginx.dockerfile
    links:
      - node1:node1
      - node2:node2
    ports:
      - "80:80"
      - "443:443"
    env_file:
      - ././docker/env/app.${APP_ENV}.env
    network:
      - foo-network

  node1:
    container_name: node-foo-1
    build:
      context: .
      dockerfile: .docker/node.dockerfile
    # environment:
    # NODE_ENV: development
    # v--- use env_file to group several env variables inside (see below)
    env_file:
      - ././docker/env/app.${APP_ENV}.env
    networks:
      - foo-network
    ports:
      - "8080"
    working_dir: /var/www/foo
    volumes:
      - .:/var/www/foo
  node2:
    container_name: node-foo-2
    build:
      context: .
      dockerfile: .docker/node.dockerfile
    env_file:
      - ././docker/env/app.${APP_ENV}.env
    networks:
      - foo-network
    ports:
      - "8080"
    working_dir: /var/www/foo
    volumes:
      - .:/var/www/foo

  mongo:
    container_name: mongo
    image: mongo
    build:
      context: .
      dockerfile: .docker/docker-mongo.dockerfile
    ports:
      - "27017:27017"
    networks:
      - foo-network
    env_file:
      - ././docker/env/app.${APP_ENV}.env
      - ././docker/env/mongo.${APP_ENV}.env

networks:
  nodeapp-network:
    driver: bridge
```

### Commands

```
docker-compose build
docker-compose up
docker-compose down
docker-compose logs
docker-compose ps
docker-compose stop
docker-compose rm
docker-compose scale <servicename>=<amount>
```

`build`: builds the images  
`up/down`: starts/stops the built images as containers (down also removes the containers `docker-compose down --rmi all --volumes` will remove all containers, all their images and the docker managed volumes)  
`logs`: shows the output of the containers  
`ps`: lists the services  
`stop/start/rm`: stops/starts/removes the services

_Note: you can also run all commands on a single service by adding the name. I.e. `docker-compose up --no-deps node`. Latter will run the node container without any service dependencies._

### Example structure

```
.docker/
    config/
        …
    env/
        app.development.env
            > NODE_ENV=development
            > Env2=foo
        mongo.development.env
            > MONGODB_ROOT_USERNAME=dbadmin
            > MONGODB_ROOT_PASSWORD=password
            > MONGODB_ROOT_ROLE=root
            > MONGODB_USERNAME=webrole
            > MONGODB_PASSWORD=password
            > MONGODB_DBNAME=foo
            > MONGODB_ROLE=readWrite
            > NODE_ENV=development
    mongo_scripts/
        backup_job.sh
        entrypoint.sh
        first_run.sh
            > applies env variables to mongodb
            > ROOT_USER=${MONGODB_ROOT_USERNAME}
            > ROOT_PASS=${MONGODB_ROOT_PASSWORD}
            > ROOT_DB="admin"
            > ROOT_ROLE=${MONGODB_ROOT_ROLE=root}
            > …
        run.sh
            > scedule cron jobs for backup with backup_job.sh
            > run first_run.sh
    node_scripts/
        …
    redis_scripts/
        …
    docker-mongo.dockerfile
    docker-nginx.dockerfile
    docker-node.dockerfile
    docker-redis.dockerfile
```

```
Example of setup on pluralsight:
https://app.pluralsight.com/player?course=docker-web-development&author=dan-wahlin&name=docker-web-development-m7&clip=5&mode=live
```

#### Dockerfile Examples

##### nginx

```dockerfile
FROM nginx:latest
MAINTAINER foo
# copy custom nginx config
COPY ./.docker/config/nginx.conf /etc/nginx/nginx.conf
# copy public resources to nginx path
COPY ./public /var/www/public
EXPOSE 80 443
ENTRYPOINT ["nginx"]
```

##### node

```dockerfile
FROM node:latest
MAINTAINER foo
WORKDIR /var/www/foo
RUN npm i -g pm2@latest
RUN mkdir -p /var/log/pm2
EXPOSE 8080
ENTRYPOINT ["pm2", "start", "server.js", "--name", "foo", "--log", "/var/log/pm2/pm2.log", "etc…"]
```

## Swarm

```bash
docker swarm init --advertise-addr <ip>:<port> --listen-addr <ip>:<port>
# Example: docker swarm init --advertise-addr 213.244.192.13:2377 --listen-addr 213.244.192.13:2377
```

`--advertise-addr <ip>:<port>`: no matter how many NICs and IPs, this is the one to use for swarm related stuff as using the API  
`--listen-addr <ip>:<port>`: what the node listens on for swarm manager traffic

All nodes will have to be able to reach this IP.  
Any Port will work but native engine port is `2375`, secure engine is `2376`, so `2377` is the unofficial swarm port.  
Also add these 2 commands to each manager and each worker.  
**Important: these IPs are the Worker/Managers OWN IPs (not the lead manager ip)**

### List nodes

```
docker node ls
```

lists all docker nodes in a swarm. Can only be run by managers

### Add managers

```
docker swarm join-token manager
```

Gives the exact command to add managers to the swarm.  
Managers are also acting as workers.  
Add `--advertise-addr <server-ip>:2377 --listen-addr <server-ip>:2377` at the end of the command. Make sure, that `server-ip` is the IP of the server you’re currently on.

### Add workers

```
docker swarm join-token worker
```

Gives the exact command to add workers to the swarm

### Make worker a manager

```
docker node promote <id>
```

Promotes a worker to become a manager.

## Services

```
docker service <create | ls | ps | inspect | update | rm>
```

### Inspect

```
docker service inspect --pretty <name>
```

### Create

```
docker service create --name <name> -p <port>:<port> --replicas <amount> <image>
```

`-p`: maps ports accross entire service
`--replicas`: how many tasks inside the service (distribute task around X worker nodes in the network. When one goes down, anotherone takes over)

### Remove

```
docker service rm <name>
```

removes the service

### Scaling

```
docker service scale <name>=<amount>
```

Is an alias for: `docker service update --replicas <amount> <name>`  
`name`: the name of the service to scale  
`amount`: the amount of replicas it should have

Keep in mind. Adding new nodes to the deck does not automatically rescale the cluster.

### Rolling updates

```
docker service update --image <image>:<version> --update-parallelism <number> --update-delay <time>s <service-name>
```

`--update-parallelism 3`: update _3_ tasks at a time
`--update-delay 10s`: update _3_ tasks every _10_ seconds

Get service name -

```bash
$ docker service ls
```

Force-update/recreate it -

```bash
$ docker service update --force MY-APP_nginx
```

### Stack (docker-compose service)

```bash
docker stack deploy --compose-file=docker-compose.yml
```

Same as `docker-compose up -d`

```bash
docker stack rm
```

Same as `docker-compose down`

## Useful

### Create an ubuntu dev-environement in seconds:

```bash
docker run \
  -e HOST_IP=$(ifconfig en0 | awk '/ *inet /{print $2}') \
  -v $(pwd):/src \
  -t -i \
  ubuntu /bin/bash
```

Will start a docker ubuntu machine and bind some volume to `src` into the machine. Now you can play around from within the terminal.

### SSH into a running container

```bash
docker exec -it <container name> /bin/bash
```

### Copy files from Docker container to host

```bash
docker cp <containerId>:/file/path/within/container /host/path/target
```

## Troubleshooting

### Permission denied

If you get following error:
```bash
docker: Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post http://%2Fvar%2Frun%2Fdocker.sock/v1.26/containers/create: dial unix /var/run/docker.sock: connect: permission denied.
See 'docker run --help'.
```

You’ll have to add your user to the `docker` group:
```bash
sudo usermod -a -G docker <user>
```

Replace `<user>` with your user account. (You can find your user account name by typing `whoami` in the console)  
And then restart your console (or open a new one) for the changes to take effect.

