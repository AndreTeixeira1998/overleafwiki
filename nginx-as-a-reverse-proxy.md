Below is an example nginx config file which is not dissimilar to the one used to run www.sharelatex.com. You will need to have nginx 1.4 or greater for websocket support. 

	server {
		listen         80;
		server_name    custom.sharelatex.com;

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