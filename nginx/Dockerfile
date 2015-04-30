# кривыми добрыми руками, krasina15

FROM ubuntu:trusty
MAINTAINER Sergey Tarasenko <krasina15@gmail.com>

ENV DEBIAN_FRONTEND noninteractive
RUN apt-get -y update
RUN apt-get -y install software-properties-common
RUN apt-get -y install python-software-properties
RUN add-apt-repository ppa:chris-lea/nginx-devel
RUN apt-get -y update

RUN apt-get -y install nginx-extras

VOLUME ["/etc/nginx", "/var/log/nginx"]

EXPOSE 80 443

CMD ["/usr/sbin/nginx", "-c", "/etc/nginx/nginx.conf"]

