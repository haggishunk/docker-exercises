ARG BASEIMAGE=alpine
ARG BASEIMAGEVERSION=latest
FROM ${BASEIMAGE}:${BASEIMAGEVERSION}

RUN apk update && apk add curl && apk add socat && rm -rf /var/cache/*

ENV ENVIRO1 from the past
RUN ["/bin/sh", "-c", "echo $ENVIRO1 > /message"]
