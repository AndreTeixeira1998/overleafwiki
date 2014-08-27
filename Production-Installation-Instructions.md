This guide assumes that you want to install ShareLaTeX at `/var/www/sharelatex`. There's nothing special about this path, but you'll need to customize the instructions yourself if you want to install it elsewhere.

*NB. We have an Ubuntu 12.04 package which performs all of these steps automatically: [[Installing on Ubuntu 12.04 from dpkg (.deb)]]*

### Dependencies

Make sure you have all the of the required [[Dependencies]] installed.

### Downloading and Compiling ShareLaTeX

First checkout the release branch of the ShareLaTeX Repository:

```bash
$ sudo mkdir -p /var/www
$ sudo git clone -b release https://github.com/sharelatex/sharelatex.git /var/www/sharelatex
$ cd /var/www/sharelatex
```

Next install all the node modules and ShareLaTeX services:

```bash
$ sudo npm install
$ sudo grunt install
```

It is a good idea to run the ShareLaTeX files as a non-root user. I.e.

```sh
$ sudo adduser --system --home /var/www/sharelatex --no-create-home --group sharelatex
$ sudo chown -R sharelatex:sharelatex /var/www/sharelatex
```

### Config file

The installation step should have generated a default config file in `config/settings.development.coffee`. You can place this anywhere, and tell ShareLaTeX where to find it with the `SHARELATEX_CONFIG` environment variable. On a server, somewhere like `/etc/sharelatex/settings.coffee` makes sense:

```sh
$ sudo mkdir /etc/sharelatex
$ sudo mv config/settings.development.coffee /etc/sharelatex/settings.coffee
```

You should open this file, review the default settings, and make any modifications that you need. Note that you will need to change `DATA_DIR` and `TMP_DIR` to point to a system-wide path. E.g.

```
# In /etc/sharelatex/settings.coffee
DATA_DIR = '/var/lib/sharelatex/data'
TMP_DIR  = '/var/lib/sharelatex/tmp'
```

Make sure that these directories exist and are read/writeable by the ShareLaTeX user:

```bash
$ sudo mkdir -p /var/lib/sharelatex/data
$ sudo mkdir -p /var/lib/sharelatex/data/user_files
$ sudo mkdir -p /var/lib/sharelatex/data/compiles
$ sudo mkdir -p /var/lib/sharelatex/data/cache
$ sudo mkdir -p /var/lib/sharelatex/tmp
$ sudo mkdir -p /var/lib/sharelatex/tmp/uploads
$ sudo mkdir -p /var/lib/sharelatex/tmp/dumpFolder
$ sudo chown -R sharelatex:sharelatex /var/lib/sharelatex
```

### Running ShareLaTeX

ShareLaTeX consists of several HTTP different services which run on different ports. The main entry points to these services can be found at `sharelatex/web/app.js`, `sharelatex/docstore/app.js`, `sharelatex/chat/app.js`, etc. On Ubuntu 12.04, we recommend running these services via [Upstart](http://upstart.ubuntu.com/). 

The following is an example upstart script for the `web` service and should be placed in `/etc/init/sharelatex-web.conf`:

```
description "sharelatex-web"
author      "ShareLaTeX <team@sharelatex.com>"

start on (local-filesystems and net-device-up IFACE!=lo)
stop on shutdown

respawn

limit nofile 8192 8192

pre-start script
    mkdir -p /var/log/sharelatex
end script

script
    SERVICE=web
    USER=sharelatex
    GROUP=sharelatex
    # You may need to replace this with an absolute 
    # path to Node.js if it's not in your system PATH.
    NODE=node
    SHARELATEX_CONFIG=/etc/sharelatex/settings.coffee
    LATEX_PATH=/usr/local/texlive/2014/bin/x86_64-linux

    echo $$ > /var/run/sharelatex-$SERVICE.pid
    chdir /var/www/sharelatex/$SERVICE
    exec sudo -u $USER -g $GROUP env SHARELATEX_CONFIG=$SHARELATEX_CONFIG NODE_ENV=production PATH=$PATH:$LATEX_PATH $NODE app.js >> /var/log/sharelatex/$SERVICE.log 2>&1
end script
```

You can find a collection of upstart scripts for each service in [sharelatex/package/upstart](https://github.com/sharelatex/sharelatex/tree/master/package/upstart).

### Logs

The above upstart script will cause the ShareLaTeX processes to log out to `/var/log/sharelatex/SERVICE.log` where `SERVICE` is one of `web`, `docstore`, `chat`, etc.

### User and Group

The above upstart script will run the ShareLaTeX services as `sharelatex:sharelatex`. You should make sure that the sharelatex files and directories are owned or accessible by this user/group combination.

### Accessing ShareLaTeX on port 80

By default ShareLaTeX runs the web interface on port 3000. We recommend running a reverse proxy like Nginx (version >=1.4) or Apache in front of ShareLaTeX to serve it on port 80. More details can be found at: [[Nginx as a Reverse Proxy]].

### Problems

If you have any problems getting ShareLaTeX to work, the first place to check is our [[FAQ]]. Lots of common problems and solutions are listed there.