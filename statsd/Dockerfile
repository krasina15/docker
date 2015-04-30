# кривыми добрыми руками, krasina15

FROM ubuntu:trusty
MAINTAINER Sergey Tarasenko <krasina15@gmail.com>

ENV DEBIAN_FRONTEND noninteractive
RUN apt-get -y update
RUN apt-get -y install software-properties-common
RUN apt-get -y install python-software-properties
RUN add-apt-repository ppa:chris-lea/node.js
RUN apt-get -y update
RUN apt-get -y install git nodejs
RUN mkdir /src && git clone https://github.com/etsy/statsd.git /src/statsd
RUN mkdir /src/config

VOLUME ["/src/config"]

ADD config.js /src/statsd/config.js

EXPOSE 8125/udp 8126

CMD ["/usr/bin/node", "/src/statsd/stats.js", "/src/config/config.js"]
