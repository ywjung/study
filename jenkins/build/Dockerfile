FROM jenkins/jenkins:lts-alpine

USER root

RUN apk add docker
ENV DOCKER_COMPOSE_VERSION 1.27.4
RUN curl -fsSL https://github.com/docker/compose/releases/download/$DOCKER_COMPOSE_VERSION/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose && chmod +x /usr/local/bin/docker-compose
