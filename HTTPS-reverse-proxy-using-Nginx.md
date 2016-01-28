To run a ShareLaTeX Docker image on HTTPS a reverse proxy will need to be added in front. We Recommend Nginx, HAproxy 1.5+ would also work, apache seems to struggle with websockets so we don't recommend it.


## Example Nginx SSL config

	server {
	    listen 443 ssl;
	    server_name    _; # Catch all, see http://nginx.org/en/docs/http/server_names.html

        server_name mydockersharelatex.com;
        ssl_certificate /etc/nginx/ssl/nginx.crt;
        ssl_certificate_key /etc/nginx/ssl/nginx.key;

	    client_max_body_size 50M;

	    location / {
	        proxy_pass http://localhost:80;
	        proxy_set_header X-Forwarded-Proto $scheme;
	        proxy_http_version 1.1;
	        proxy_set_header Upgrade $http_upgrade;
	        proxy_set_header Connection "upgrade";
	        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
	        proxy_read_timeout 3m;
	        proxy_send_timeout 3m;
	    }
	}