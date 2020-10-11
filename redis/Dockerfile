FROM ubuntu:latest

RUN apt-get update
RUN apt-get -y upgrade
RUN apt-get install -qq -y redis-server

CMD /usr/bin/redis-server