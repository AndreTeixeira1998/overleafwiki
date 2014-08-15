The ShareLaTeX web service runs on port 3000 by default. We recommend running a reverse proxy in front of it which listens on port 80.

Below is an example Nginx config file which is similar to the one used at http://www.sharelatex.com. You will need to have Nginx 1.4 or greater for websocket support. 

	server {
		listen         80;
		server_name    my.domain.example.com;

		set $static_path /PATH_TO_SHARELATEX/public;

		location / {
			proxy_pass http://localhost:3000;
			proxy_set_header Host $http_x_forwarded_host;
			proxy_http_version 1.1;
			proxy_set_header Upgrade $http_upgrade;
			proxy_set_header Connection "upgrade";
		}

		location /stylesheets {
			expires 1y;
			root $static_path/;
		}

		location /minjs {
			expires 1y;
			root $static_path/;
		}

		location /img {
			expires 1y;
			root $static_path/;
		}
	}