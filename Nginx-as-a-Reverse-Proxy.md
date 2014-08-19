The ShareLaTeX web service runs on port 3000 by default. We recommend running a Nginx as a reverse proxy in front of it which listens on port 80 and proxies requests through the local sharelatex process on port 3000.

ShareLaTeX uses websockets, which are only supported in Nginx 1.4 and above. The default installation of Nginx on Ubuntu 12.04 is too old, so you'll need to either install Nginx from source (http://nginx.org/), or install it via an official Nginx build for Ubuntu:

```bash
$ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv ABF5BD827BD9BF62
$ echo 'deb http://nginx.org/packages/ubuntu/ precise nginx' | sudo tee /etc/apt/sources.list.d/nginx.list
$ sudo apt-get update
$ sudo apt-get install nginx
```

Below is an example Nginx config file which is similar to the one used at http://www.sharelatex.com. This should be placed in `/etc/nginx/conf.d/sharelatex.conf`:

```
server {
	listen         80;
	server_name    _; # Catch all, see http://nginx.org/en/docs/http/server_names.html

	set $static_path /var/www/sharelatex/web/public;

	location / {
		proxy_pass http://localhost:3000;
		proxy_set_header Host $http_x_forwarded_host;
		proxy_http_version 1.1;
		proxy_set_header Upgrade $http_upgrade;
		proxy_set_header Connection "upgrade";
		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
		proxy_read_timeout 3m;
		proxy_send_timeout 3m;
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
```