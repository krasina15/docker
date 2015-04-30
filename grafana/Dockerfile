# кривыми добрыми руками, krasina15

FROM ubuntu:trusty
MAINTAINER Sergey Tarasenko <krasina15@gmail.com>

ENV DEBIAN_FRONTEND noninteractive
RUN apt-get -y update
RUN apt-get -y install libfontconfig wget adduser openssl ca-certificates
RUN wget http://grafanarel.s3.amazonaws.com/builds/grafana_latest_amd64.deb
RUN dpkg -i grafana_latest_amd64.deb

EXPOSE 3000

VOLUME ["/var/lib/grafana", "/var/log/grafana", "/etc/grafana"]

WORKDIR /usr/share/grafana

CMD ["/usr/sbin/grafana-server", "--config", "/etc/grafana/grafana.ini"]

