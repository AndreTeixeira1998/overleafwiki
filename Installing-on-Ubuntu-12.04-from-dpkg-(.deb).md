We have created an easy to install package for Ubuntu 12.04, 64-bit. (It may work more recent versions as well, but we have only tested with 12.04). You can download the latest release from the Github releases page: https://github.com/sharelatex/sharelatex/releases.

### Dependencies

Make sure you have all the of the required [[Dependencies]] installed. These are (redis-server > 2.6.12, mongodb-org > 2.4.0, nodejs > 0.10.0).

### Installing

After downloading the .deb packages, install it with `dkpg`:

```sh
$ sudo dpkg -i sharelatex_VERSION_amd64.deb
```

### Setting up Nginx

The package contains an Nginx config file which will tell Nginx to act as a reverse HTTP proxy to the ShareLaTeX instance running on port 3000. This config file is installed at `/etc/nginx/conf.d/sharelatex.conf`. You will still need to manually install Nginx (> 1.4.0 for websocket support) though. For more information, see [[Nginx as a Reverse Proxy]]