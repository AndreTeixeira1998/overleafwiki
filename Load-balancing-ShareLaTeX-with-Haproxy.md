ShareLaTeX can be easily run on multiple hosts to scale horizontally. The main things to keep in mind are:

* All containers need to point to the same Mongodb, redis and disk stores
* A users should have sticky sessions to the same container

    
This is an example Haproxy config which is similar to the one used on ShareLaTeX.com. Note the  `cookie sharelatex-docker-X` which sets the cookie for the backend server.


        global
            log /dev/log    local0
            log /dev/log    local1 notice
            chroot /var/lib/haproxy
            stats socket /run/haproxy/admin.sock mode 660 level admin
            stats timeout 30s
            user haproxy
            group haproxy
            daemon

        defaults
            log global
            mode    http
            option  httplog
            option  dontlognull
            timeout connect 5000
            timeout client  50000
            timeout server  50000
            default_backend sharelatex

        frontend http-in
            bind *:80
            default_backend sharelatex

        backend sharelatex
            option httpchk GET /status
            cookie      SERVERID insert indirect nocache
            balance     leastconn
            mode        http
            server docker-0 172.17.42.1:5000 cookie sharelatex-docker-0 check inter 2000
            server docker-1 172.17.42.1:5001 cookie sharelatex-docker-1 check inter 2000

        listen admin *:4999
          mode http
          stats auth user:pass
          stats uri /