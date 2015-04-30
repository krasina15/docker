0. pull:
	docker pull skuvault/graphite

0. build:
	docker build -t skuvault/graphite .

prerun:
1. create /usr/local/skuvault/graphite/conf, /usr/local/skuvault/graphite/data
2. create local_settings.py  in to /usr/local/skuvault/graphite/conf

local_settings.py:

SECRET_KEY = '1UNSAFE_DEFAULT'
ALLOWED_HOSTS = [ '*' ]
LOG_RENDERING_PERFORMANCE = True
LOG_CACHE_PERFORMANCE = True
LOG_METRIC_ACCESS = True
GRAPHITE_ROOT = '/usr/share/graphite-web'
CONF_DIR = '/etc/graphite'
STORAGE_DIR = '/var/lib/graphite/whisper'
CONTENT_DIR = '/usr/share/graphite-web/static'
WHISPER_DIR = '/var/lib/graphite/whisper'
LOG_DIR = '/var/log/graphite'
INDEX_FILE = '/var/lib/graphite/search_index'  # Search index file
DATABASES = {
    'default': {
        'NAME': '/var/lib/graphite/graphite.db',
        'ENGINE': 'django.db.backends.sqlite3',
        'USER': '',
        'PASSWORD': '',
        'HOST': '',
        'PORT': ''
    }
}

3. tune up local_settings.py

run:
4. (optional) create upstart script in to /etc/init/

graphite_container.config

description "carbon container"
author "Sergey Tarasenko krasina15@gmail.com"
start on filesystem and started docker
stop on runlevel [!2345]
#respawn
#respawn limit 6 10
#post-stop exec sleep 30
script
        DOCKER=/usr/bin/docker
        LOCAL_GRAPHITE_CONF=/usr/local/skuvault/graphite/conf
        LOCAL_GRAPHITE_DATA=/usr/local/skuvault/graphite/data
        REMOTE_GRAPHITE_CONF=/etc/graphite
        REMOTE_GRAPHITE_DATA=/var/lib/graphite

        $DOCKER run \
-v $LOCAL_GRAPHITE_CONF:$REMOTE_GRAPHITE_CONF \
-v $LOCAL_GRAPHITE_DATA:$REMOTE_GRAPHITE_DATA \
-p 8000:8000 \
skuvault/graphite
end script

5. start container

other:
6. mapping directories
- /usr/local/skuvault/graphite/conf = /etc/graphite
- /usr/local/skuvault/graphite/data = /var/lib/graphite

