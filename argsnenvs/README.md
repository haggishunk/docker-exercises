# Explore ARGs and ENVs in Docker Build and Run

Docker ARGs and ENVs are sometimes confused, but have very different scopes.

ARGs are used only in the FROM statement and can be used toset the base image, base image version or specify an alias for the build stage.  ARGs can be overridden at build-time.

ENVs are declared in the Dockerfile for variable substitution or usage in the underlying image.  These ENVs can be overridden at runtime.

## Requirements

- Docker

## Usage

### Build

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

### Run

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

Inside the container you can see the Dockerfile-specified environment variable ENVIRO1 is present as a file
```
/ # cat message
from the past
```

While the environment variable ENVIRO1 shows the value shows the value you specified in the run command previously.
```
/ # echo $ENVIRO1
i am from the outside
```

### Clean

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

