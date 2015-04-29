build:

	docker build -t skuvault/statsd .

run:
	- 1 create /usr/local/skuvault/statd
	- 2 copy config.js to /usr/local/skuvault/statd/
	- 3 tune up confing.js
	- 4 copy upstart script to /etc/init
	- 5 start service
