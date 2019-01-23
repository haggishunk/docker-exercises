# Explore ARGs and ENVs in Docker Build and Run

## Requirements

- Docker

## Usage

Build your image using defaults
```
make build
```
or the manual way,
```
docker build -t <name>:<ver> .
```

Build your image with a different base image or version
```
make build BASEIMAGEVERSION=3.1
```
or the manual way,
```
docker build --build-arg BASEIMAGEVERSION=<alpine_ver> -t <name>:<ver> .
```

Run your image in a container
```
make run
```
or the manual way,
```
docker run -it <name>:<ver> /bin/sh
```

Run your image in a container and inject an environment variable
```
make run ENVIRO1="i am from the outside"
```
or the manual way,
```
docker run -it --env ENVIRO1=<value> <name>:<ver> /bin/sh
```

Clean up your containers and images
```
make clean
```
or the manual way

use this command to list Docker containers using the image you built
```
docker container ls -a --filter ancestor=<name>:<ver>
```
or quietly to return a list of container ids
```
docker container ls -aq --filter ancestor=<name>:<ver>
```
supply that list to docker container rm
```
docker container rm $(docker container ls -aq --filter ancestor=<name>:<ver>)
```

