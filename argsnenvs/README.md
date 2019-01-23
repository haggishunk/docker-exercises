docker build --build-arg VERSION=3.2 -t argsnenvs:3.2 .

docker run --env ENVIRO1="right now" -it argsnenvs:latest /bin/sh
