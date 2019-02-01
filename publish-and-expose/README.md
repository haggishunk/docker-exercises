# Explore Dockerfile EXPOSE and Docker Run Port Publishing

Docker containers are almost always interacted with via network connections. Docker has two conventions to manage the ports that a container can be reached through:  EXPOSE and PUBLISH.

It is important to note that these connections are like port mapping.  An external request to the container connects with a published port which then gets routed to a (possibly different) internal port that the application in the container listens on.

The EXPOSE keyword is part of the Dockerfile and is used as way to communicate to the user of the image what port the containerized application listens to.  An advanced usage of the -P flag to the docker run command can automatically publish any EXPOSE-d ports in the Dockerfile to arbitrary ports above 32767.  While an nondeterminate port might be troublesome for some users, a docker network on a host allows several containers to communicate easily even with these conditions.

## Requirements

- [Docker](https://get.docker.com/)

## Usage

### Build

Build your image using defaults (at the time of writing alpine:latest is 3.5)
```
make build
```
or the manual way,
```
docker build -t publish-and-expose:1.0 .
```

### Run

Run your image in a container
```
make run
```
or the manual way,
```
docker run -d publish-and-expose:1.0 /bin/sh
```

But can the nginx server be reached?  By default nginx listens on port 80...
```
$ curl localhost:80
curl: (7) Failed to connect to localhost port 80: Connection refused
```

Well, we didn't ever specify the port to *publish* the container on.
```
make run-publish
```
or the manual way
```
docker run -d -p 8080:80 publish-and-expose:1.0
```

This time we mapped your host's port 8080 to route traffic to port 80 inside the container.  You should see the default nginx welcome page.
```
$ curl localhost:8080
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
...
```

Now let's explore how the -P flag and the EXPOSE keyword can work for us.
```
make run-automap
```
or
```
docker run -d -P publish-and-expose:1.0
```

What port is it on?
```
$ docker ps
CONTAINER ID        IMAGE                    COMMAND                  CREATED             STATUS              PORTS                   NAMES
085597dcb058        publish-and-expose:1.0   "nginx -g 'daemon of…"   3 seconds ago       Up 1 second         0.0.0.0:32772->80/tcp   publish-and-expose-automap
...
```

Make an http request to the port shown on the image, in the above example 32772.
```
$ curl localhost:32772
```
And you should see the familiar nginx welcome site.

### Clean

Clean up your containers and images
```
make clean
```
or the manual way

use this command to stop Docker containers running the image you built
```
docker stop `docker ps -aq --filter ancestor=publish-and-expose:1.0`
```
Then remove all the containers that were running the image
```
docker container rm `docker container ls -aq --filter ancestor=publish-and-expose:1.0`
```
Remove the base image
```
docker image rm publish-and-expose:1.0
```
