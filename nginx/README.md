0. pull:
	docker pull skuvault/nginx

0. build:
	docker build -t skuvault/nginx .

prerun:
1. create /usr/local/skuvault/nginx/conf, /usr/local/skuvault/nginx/logs
2. find somewhere the contents of debian/ubuntu, and put all that stuff into /usr/local/skuvault/nginx/conf
3. tune up nginx.conf and create and tune configs of webhosts

run:
4. (optional) create upstart script in to /etc/init/

nginx_container.config

description "nginx container"
author "Sergey Tarasenko krasina15@gmail.com"
start on filesystem and started docker
stop on runlevel [!2345]
respawn
respawn limit 6 10
post-stop exec sleep 30
script
        DOCKER=/usr/bin/docker
        LOCAL_NGINX_CONF=/usr/local/skuvault/nginx/conf
        LOCAL_NGINX_LOGS=/usr/local/skuvault/nginx/logs
        REMOTE_NGINX_CONF=/etc/nginx
        REMOTE_NGINX_LOGS=/var/log/nginx
        $DOCKER run -v $LOCAL_NGINX_CONF:$REMOTE_NGINX_CONF -v $LOCAL_NGINX_LOGS:$REMOTE_NGINX_LOGS -p 80:80 -p 443:443 skuvault/nginx
end script

5. start container

other:
6. mapping directories
- /etc/nginx = /usr/local/skuvault/nginx/conf
- /var/log/nginx = /usr/local/skuvault/nginx/logs
