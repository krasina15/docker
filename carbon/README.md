0. pull:
	docker pull skuvault/carbon

0. build:
	docker build -t skuvault/carbon .

prerun:
1. create /usr/local/skuvault/carbon, /usr/local/skuvault/graphite/data
2. create carbon.conf in to /usr/local/skuvault/carbon/
```
carbon.conf:

[cache]
STORAGE_DIR    = /var/lib/graphite/
CONF_DIR       = /etc/carbon/
LOG_DIR        = /var/log/carbon/
PID_DIR        = /var/run/
LOCAL_DATA_DIR = /var/lib/graphite/whisper/
ENABLE_LOGROTATION = False
USER = root
MAX_CACHE_SIZE = inf
MAX_UPDATES_PER_SECOND = 500
MAX_CREATES_PER_MINUTE = 50
LINE_RECEIVER_INTERFACE = 0.0.0.0
LINE_RECEIVER_PORT = 2003
ENABLE_UDP_LISTENER = False
UDP_RECEIVER_INTERFACE = 0.0.0.0
UDP_RECEIVER_PORT = 2003
PICKLE_RECEIVER_INTERFACE = 0.0.0.0
PICKLE_RECEIVER_PORT = 2004
LOG_LISTENER_CONNECTIONS = True
USE_INSECURE_UNPICKLER = False
CACHE_QUERY_INTERFACE = 0.0.0.0
CACHE_QUERY_PORT = 7002
USE_FLOW_CONTROL = True
LOG_UPDATES = False
LOG_CACHE_HITS = False
LOG_CACHE_QUEUE_SORTS = True
CACHE_WRITE_STRATEGY = sorted
WHISPER_AUTOFLUSH = False
WHISPER_FALLOCATE_CREATE = True
[relay]
LINE_RECEIVER_INTERFACE = 0.0.0.0
LINE_RECEIVER_PORT = 2013
PICKLE_RECEIVER_INTERFACE = 0.0.0.0
PICKLE_RECEIVER_PORT = 2014
LOG_LISTENER_CONNECTIONS = True
RELAY_METHOD = rules
REPLICATION_FACTOR = 1
DESTINATIONS = 127.0.0.1:2004
MAX_DATAPOINTS_PER_MESSAGE = 500
MAX_QUEUE_SIZE = 10000
USE_FLOW_CONTROL = True
[aggregator]
LINE_RECEIVER_INTERFACE = 0.0.0.0
LINE_RECEIVER_PORT = 2023
PICKLE_RECEIVER_INTERFACE = 0.0.0.0
PICKLE_RECEIVER_PORT = 2024
LOG_LISTENER_CONNECTIONS = True
FORWARD_ALL = True
DESTINATIONS = 127.0.0.1:2004
REPLICATION_FACTOR = 1
MAX_QUEUE_SIZE = 10000
USE_FLOW_CONTROL = True
MAX_DATAPOINTS_PER_MESSAGE = 500
MAX_AGGREGATION_INTERVALS = 5
```
2. create storage-schemas.conf in to /usr/local/skuvault/carbon/
```
storage-schemas.conf:

[carbon]
pattern = ^carbon\.
retentions = 60:90d
[default_1min_for_1day]
pattern = .*
retentions = 60s:1d

```
3. tune up carbon.conf and storage-schemas.conf

run:
4. (optional) create upstart script in to /etc/init/
```
carbon_container.config

author "Sergey Tarasenko krasina15@gmail.com"
start on filesystem and started docker
stop on runlevel [!2345]
respawn
respawn limit 6 10
post-stop exec sleep 30
script
        DOCKER=/usr/bin/docker
        LOCAL_CARBON_CONF=/usr/local/skuvault/carbon
        LOCAL_GRAPHITE_DATA=/usr/local/skuvault/graphite/data
        REMOTE_CARBON_CONF=/etc/carbon
        REMOTE_GRAPHITE_DATA=/var/lib/graphite

        $DOCKER run \
-v $LOCAL_CARBON_CONF:$REMOTE_CARBON_CONF \
-v $LOCAL_GRAPHITE_DATA:$REMOTE_GRAPHITE_DATA \
-p 2003:2003 -p 2004:2004 -p 7002:7002 \
skuvault/carbon
end script
```
5. start container

other:
6. mapping directories
- /usr/local/skuvault/carbon/conf = /etc/carbon
- /usr/local/skuvault/carbon/logs = /var/log/carbon
- /usr/local/skuvault/graphite/data = /var/lib/graphite
