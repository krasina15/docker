# кривыми добрыми руками, krasina15

FROM ubuntu:trusty
MAINTAINER Sergey Tarasenko <krasina15@gmail.com>

ENV DEBIAN_FRONTEND noninteractive
RUN apt-get -y update
RUN apt-get -y install graphite-carbon
RUN echo "CARBON_CACHE_ENABLED=true" > /etc/default/graphite-carbon

VOLUME ["/etc/carbon", "/var/lib/graphite"]

EXPOSE 2003 2004 7002

CMD ["/usr/bin/carbon-cache", "--debug", "--config=/etc/carbon/carbon.conf", "start"]
