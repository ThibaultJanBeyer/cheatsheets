[back to overwiev](/../..)

# Docker Cheatsheet

##### Table of Contents

[Install](#install)
[Basics](#basics)

## Install

### Linux

```
wget -qO- https://get.docker.com/ | sh
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

### Container Volumes

```
docker run -p 80:8080 -v $(pwd):/var/www node
```

`-v <host-location>:<location>`: create a volume. `<location>` is the container volume (alias inside the container). `<host-location>` is the location on your computer.  
`$(pwd)`: current working directory.  

```
docker inspect <container>
```

Will display the source/target location of mounted volumes. (in the `Mounts:` area)

```
docker run -p <h-port>:<c-port> -v <h-location>:<c-location> -w "<location>" <image> <command>
# Example: 
docker run -p 80:8080 -v $(pwd):/var/www -w "/var/www" node npm start
```

`-w "<location>"`: specifies the working directory inside the container (where the command will be run).  
`<command>`: will be run after container creation. Example `npm start`.  

```
docker rm -v <container>
```

`-v`: also removes the container that was created/managed by docker (outside of the container). *Note: It will not remove your source code if you specified any.*  

## Processes

```
docker ps
```

shows all running Processes

```
docker ps -a
```

shows all processes, running or not, active or exited.

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

### Custom Images

#### Dockerfile

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

#### Build an image

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

#### Publish image to docker hub

```
docker push <username>/<imagetag>
```

Typical flow:
```
docker login
docker push <username>/<imagetag>
```
Pushes to a public repo on your dockerhub

## Swarm

```
docker swarm init --advertise-addr <ip>:<port> --listen-addr <ip>:<port>
```
`--advertise-addr <ip>:<port>`: no matter how many NICs and IPs, this is the one to use for swarm related stuff as using the API  
`--listen-addr <ip>:<port>`: what the node listens on for swarm manager traffic  
All nodes will have to be able to reach this IP.  
Any Port will work but native engine port is `2375`, secure engine `2376`, `2377` is the unofficial swarm port.  
Also add these 2 commands to each manager and each worker.
**Important: there IPs are the Worker/Managers OWN IPs (not the lear manager ip)**

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

## Rolling updates

```
docker service update --image <image>:<version> --update-parallelism <number> --update-delay <time>s <service-name>
```
`--update-parallelism 3`: update *3* tasks at a time
`--update-delay 10s`: update *3* tasks every *10* seconds

