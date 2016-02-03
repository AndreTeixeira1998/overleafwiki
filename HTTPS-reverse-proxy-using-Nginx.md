To run a ShareLaTeX Docker image on HTTPS a reverse proxy will need to be added in front. We Recommend Nginx, HAproxy 1.5+ would also work, apache seems to struggle with websockets so we don't recommend it.


## Example Nginx SSL config

This Nginx config is a good foundation for setting up ShareLaTeX with HTTPS, it will deal with websockets correctly and has some sane defaults. It assumes nginx is running on the same host as the docker container, if this is not the case change the `proxy_pass http://localhost:5000;` accordingly.


	server {
		listen 443 ssl;
		server_name    _; # Catch all, see http://nginx.org/en/docs/http/server_names.html

		server_name sharelatextest.com;
		ssl_certificate /etc/nginx/ssl/nginx.crt;
		ssl_certificate_key /etc/nginx/ssl/nginx.key;

		ssl_protocols               TLSv1 TLSv1.1 TLSv1.2;
		ssl_prefer_server_ciphers   on;

		# used cloudflares ciphers https://github.com/cloudflare/sslconfig/blob/master/conf
		ssl_ciphers                 EECDH+CHACHA20:EECDH+AES128:RSA+AES128:EECDH+AES256:RSA+AES256:EECDH+3DES:RSA+3DES:!MD5;

		# config to enable HSTS(HTTP Strict Transport Security) https://developer.mozilla.org/en-US/docs/Security/HTTP_Strict_Transport_Security
		# to avoid ssl stripping https://en.wikipedia.org/wiki/SSL_stripping#SSL_stripping	
		add_header Strict-Transport-Security "max-age=31536000; includeSubdomains;";

		server_tokens off;

		add_header X-Frame-Options SAMEORIGIN;

		add_header X-Content-Type-Options nosniff;

		client_max_body_size 50M;

		location / {
			proxy_pass http://localhost:5000; # change to whatever host/port the docker container is listening on.
			proxy_set_header X-Forwarded-Proto $scheme;
			proxy_http_version 1.1;
			proxy_set_header Upgrade $http_upgrade;
			proxy_set_header Connection "upgrade";
			proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
			proxy_read_timeout 3m;
			proxy_send_timeout 3m;
		}
	}