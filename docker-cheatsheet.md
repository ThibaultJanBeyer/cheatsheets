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

### Containers

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

`-it`: in terminal (ssh inside the container)  
`/bin/bash`: run bash process  
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

### Processes

```
docker ps
```

shows all running Processes

```
docker ps -a
```

shows all processes, running or not, active or exited.

### Images

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
