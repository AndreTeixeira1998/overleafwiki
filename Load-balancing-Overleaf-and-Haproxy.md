Overleaf can be easily run on multiple hosts to scale horizontally. The main things to keep in mind are:

* All containers need to point to the same MongoDB, Redis and disk stores
* A users should have sticky sessions to the same container
* The Overleaf Session Secret needs to be set the same


## Setting same Session Secret & Behind Proxy

The sessionSecret needs to be set the same on all docker instances. By default a random string is generated for you. The environment variable `SHARELATEX_SESSION_SECRET` can be used to set this. 

`--env SHARELATEX_SESSION_SECRET=something_random`

`--env SHARELATEX_BEHIND_PROXY=true`

`--env SHARELATEX_SECURE_COOKIE=true #only if using SSL`

	


## HAProxy

This is an example HAProxy config. It routes the same project compiles to the same Overleaf instance, this will improve compile performance due to the caching on disk.

	global
		log 127.0.0.1   local0
		maxconn 200
		ulimit-n 65536


	defaults
		log     global
		mode    http
		option tcplog

		retries 3
		timeout connect 5000ms
		timeout client 50000ms # use 185000ms for Server Pro
		timeout server 50000ms # use 185000ms for Server Pro

	frontend incoming_http
		bind *:80


		reqadd X-Forwarded-Proto:\ http

		capture request header Host len 31
		capture request header X-Forwarded-For len 15
		capture request header X-Forwarded-Proto len 5
		capture request header X-Forwarded-Host len 31

		option httplog

		# this stops the wrong backend being used for 2nd connection, e.g.
		# first connection is to /blog but then its /templates, without this
		# templates would be sent to /blog
		option http-server-close

		acl is_project path_beg       -i  /project

		use_backend sharelatex_project if is_project


		default_backend sharelatex_generic

	 

	backend sharelatex_generic
		option httpchk GET /status
		cookie      SERVERID insert indirect nocache
		balance     leastconn
		mode        http
		server docker-0 172.17.42.1:5000 check inter 10000
		server docker-1 172.17.42.1:5001 check inter 10000

	backend sharelatex_project
		option httpchk GET /status
		balance uri depth 2
		mode        http
		server docker-0 172.17.42.1:5000 check inter 10000
		server docker-1 172.17.42.1:5001 check inter 10000


	listen admin 107.170.168.215:2811
		mode http
		stats auth user:pass
		stats uri /

	listen admin *:4999
		mode http
		stats auth user:pass
		stats uri /

## HAProxy UI

The HAProxy UI is found on port `2811`, example: `http://sl-lin-stag-lb2-0:2811/`