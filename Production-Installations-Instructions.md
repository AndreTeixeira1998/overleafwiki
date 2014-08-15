This guide assumes that you want to install ShareLaTeX at `/var/www/sharelatex`. There's nothing special about this path, but you'll need to customize the instructions yourself if you want to install it elsewhere.

### Dependencies

Make sure you have all the of the required [[Dependencies]] installed.

### Downloading and Compiling ShareLaTeX

First checkout the ShareLaTeX Repository:

```bash
$ sudo git clone https://github.com/sharelatex/sharelatex.git /var/www/sharelatex
$ cd /var/www/sharelatex
```

Next install all the node modules and ShareLaTeX services:

```bash
$ sudo npm install
$ sudo grunt install
```

It is a good idea to run the ShareLaTeX files as a non-root user. I.e.

```sh
$ sudo chown -R www-data:www-data /var/www/sharelatex
```

### Config file

The installation step should have generated a default config file in `config/settings.development.coffee`. You can place this anywhere, and tell ShareLaTeX where to find it with the `SHARELATEX_CONFIG` environment variable. On a server, somewhere like `/etc/sharelatex/settings.coffee` makes sense:

```sh
$ sudo mkdir /etc/sharelatex
$ sudo mv config/settings.development.coffee /etc/sharelatex/settings.coffee
```

You should open this file, review the default settings, and make any modifications that you need. Note that you will likely want to change `DATA_DIR` and `TMP_DIR`. E.g.

```
# In /etc/sharelatex/settings.coffee
DATA_DIR = '/var/lib/sharelatex/data'
TMP_DIR  = '/var/lib/sharelatex/tmp'
```

You should make sure that these directories exist.

### Running ShareLaTeX

ShareLaTeX consists of several HTTP different services which run on different ports. The main entry points to these services can be found at `sharelatex/web/app.js`, `sharelatex/docstore/app.js`, `sharelatex/chat/app.js`, etc. On Ubuntu 12.04, we recommend running these services via [Upstart](http://upstart.ubuntu.com/). 

The following is an example upstart script for the `web` service and should be placed in `/etc/init/sharelatex-web.conf`:

```
description "sharelatex-web"
author      "ShareLaTeX <team@sharelatex.com>"

start on started mountall
stop on shutdown

respawn

limit nofile 8192 8192

pre-start script
	mkdir -p /var/log/sharelatex
end script

script
	SERVICE=web
	USER=www-data
	GROUP=www-data
	NODE=node # You may need to replace with an absolute path to Node if it's not in your PATH.

	echo $$ > /var/run/sharelatex-$SERVICE.pid
	chdir /var/www/sharelatex/$SERVICE
	exec sudo -u $USER -g $GROUP env SHARELATEX_CONFIG=/etc/sharelatex/settings.coffee NODE_ENV=production $NODE app.js >> /var/log/sharelatex/$SERVICE.log 2>&1
end script
```

You can find a collection of upstart scripts for each service in [sharelatex/package/upstart](https://github.com/sharelatex/sharelatex/tree/master/package/upstart).

### Logs

The above upstart script will cause the ShareLaTeX processes to log out to `/var/log/sharelatex/sharelatex-SERVICE.log` where `SERVICE` is one of `web`, `docstore`, `chat`, etc.

### User and Group

The above upstart script will run the ShareLaTeX services as `www-data:www-data`. You should make sure that the sharelatex files and directories are owned or accessible by this user/group combination.

### Accessing ShareLaTeX on port 80

By default ShareLaTeX runs the web interface on port 3000. We recommend running a reverse proxy like Nginx or Apache in front of ShareLaTeX to serve it on port 80. More details can be found in [[Running Nginx as a Reverse Proxy]].