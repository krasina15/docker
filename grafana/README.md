0. pull:
	docker pull skuvault/grafana

0. build:
	docker build -t skuvault/grafana .

prerun:
1. create /usr/local/skuvault/grafana/conf, /usr/local/skuvault/gradana/data
2. create grafana.ini in to /usr/local/skuvault/grafana/conf
```
grafana.ini:

[server]
http_addr = 0.0.0.0
http_port = 3000
[security]
admin_user = admin
admin_password = admin
secret_key = SW2YcwTIb9zpOOhoPsMm
```
3. tune up grafana.init

run:
4. (optional) create upstart script in to /etc/init/
```
grafana_container.config

description "grafana container"
author "Sergey Tarasenko krasina15@gmail.com"
start on filesystem and started docker
stop on runlevel [!2345]
respawn
respawn limit 6 10
post-stop exec sleep 30
script

        DOCKER=/usr/bin/docker
        LOCAL_GRAFANA_CONF=/usr/local/skuvault/grafana/conf
        LOCAL_GRAFANA_DATA=/usr/local/skuvault/grafana/data
        REMOTE_GRAFANA_CONF=/etc/grafana
        REMOTE_GRAFANA_DATA=/usr/share/grafana/data

        $DOCKER run \
-v $LOCAL_GRAFANA_CONF:$REMOTE_GRAFANA_CONF \
-v $LOCAL_GRAFANA_DATA:$REMOTE_GRAFANA_DATA \
-p 3000:3000 skuvault/grafana
end script
```
5. start container

other:
6. mapping directories
- /etc/nginx = /usr/local/skuvault/nginx/conf
- /var/log/nginx = /usr/local/skuvault/nginx/logs
