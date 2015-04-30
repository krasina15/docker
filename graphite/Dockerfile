# кривыми добрыми руками, krasina15

FROM ubuntu:trusty
MAINTAINER Sergey Tarasenko <krasina15@gmail.com>

ENV DEBIAN_FRONTEND noninteractive
RUN apt-get -y update
RUN apt-get -y install graphite-web

VOLUME ["/etc/graphite", "/var/lib/graphite"]

ENV PYTHONPATH /usr/lib/python2.7/dist-packages/graphite

EXPOSE 8000

CMD ["/usr/bin/graphite-manage", "runserver", "0.0.0.0:8000", "--pythonpath=/usr/lib/python2.7/dist-packages/graphite"]

