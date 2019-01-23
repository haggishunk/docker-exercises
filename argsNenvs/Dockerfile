ARG VERSION=latest
FROM alpine:${VERSION}

RUN apk update && apk add curl && apk add socat && rm -rf /var/cache/*

ENV ENVIRO1 from the past
RUN ["/bin/sh", "-c", "echo $ENVIRO1 > /message"]


