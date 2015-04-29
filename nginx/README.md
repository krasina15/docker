build:

	docker build -t skuvault/nginx .

run:

1. create /usr/local/skuvault/nginx, /usr/local/skuvault/nginx/logs
2. copy "conf" folder to /usr/local/skuvault/nginx/
3. tune up nginx conf
4. copy upstart script to /etc/init
5. start service
