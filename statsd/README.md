0. pull:
	docker pull skuvault/statsd

0. build:
	docker build -t skuvault/statsd .

prerun:
1. create /usr/local/skuvault/statd
2. create config.js in to /usr/local/skuvault/statd/
```
config.js:1

{
  graphitePort: 2003,
  graphiteHost: " ",
  port: 8125,
  mgmt_port: 8126,
  backends: ['./backends/graphite'],
  debug: true
}

3. tune up confing.js
```
run:
4. (optional) create upstart script in to /etc/init/
```
statsd_container.config

description "statsd container"
author "Sergey Tarasenko krasina15@gmail.com"
start on filesystem and started docker
stop on runlevel [!2345]
respawn
respawn limit 6 10
post-stop exec sleep 30
script
        DOCKER=/usr/bin/docker
        LOCAL_S=/usr/local/skuvault/statsd
        REMOTE_S=/src/config
        $DOCKER run -v $LOCAL_S:$REMOTE_S -p 8125:8125/udp skuvault/statsd
end script
```
5. start container

other:
6. mapping directories

/src/config==/usr/local/skuvault/statsd
