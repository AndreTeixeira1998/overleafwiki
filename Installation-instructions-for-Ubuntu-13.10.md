This is how to install sharelatex on Ubuntu 13.10 with nginx.

Preparation
===========

ShareLaTeX needs an up to date node and redis, which you both can get
from a PPA. You also need a mongo db instance.

``` {.sourceCode .shell-session}
$ sudo add-apt-repository ppa:chris-lea/node.js
$ sudo apt-get update
$ sudo apt-get install nodejs redis-server mongodb-server
```

Sharelatex also needs a TexLive 2013 installation, which is not in the
repositories for Ubuntu 13.10. I installed it manually.

Remove all texlive packages and install the dependencies for TexLive.

``` {.sourceCode .shell-session}
$ sudo apt-get install tex-common texinfo equivs perl-tk perl-doc 
```

Download the installation script for TexLive:
[install-tl-unx.tar.gz](http://www.tug.org/texlive/acquire-netinstall.html)
and unpack it. Get a root shell-session (the install script does not
work with sudo) and run the installer

``` {.sourceCode .shell-session}
$ cd install-tl*
$ sudo -i
$ ./install-tl
```

In the menu:

-   `O` for options
-   `L` for "create symlinks in standard directories"
-   Confirm the default paths (all in /usr/local)
-   `R` to change to the main menu
-   `I` to start the installation

ShareLaTeX install
==================

Pull the source and install the node modules and then, with grunt (which
is one of the installed node modules), install the rest.

``` {.sourceCode .shell-session}
$ git clone https://github.com/sharelatex/sharelatex.git
$ cd sharelatex
$ npm install
$ grunt install
```

Check if all dependencies are installed:

``` {.sourceCode .shell-session}
$ grunt check --force
```

Minify the JS files in the web folder (see the config key
"useMinifiedJs")

``` {.sourceCode .shell-session}
$ cd web
$ grunt compile:minify
$ cd ..
```

You can test the server with

``` {.sourceCode .shell-session}
$ grunt run
```

ShareLaTeX configuration
========================

Edit `config/settings.development.coffee` to your needs. You can
generate a password for httpAuthPass and sessionSecret with the command
`pwgen 20`. Options you probably want to change:

``` {.sourceCode .coffee-script}
# ...
httpAuthUser = "sharelatex"
httpAuthPass = "generated password"
# ...
sessionSecret = "another generated random string"

module.exports =
        # ...
        internal:
                web:
                        port: webPort = 3000
                        host: "localhost"
                documentupdater:
                        port: docUpdaterPort = 3003
                        host: "localhost"
                clsi:
                        port: clsiPort = 3013
                        host: "localhost"
                filestore:
                        port: filestorePort = 3009
                        host: "localhost"
                trackchanges:
                        port: trackchangesPort = 3015
                        host: "localhost"
        # ...

        # Where your instance of ShareLaTeX can be found publically. Used in emails
        # that are sent out, generated links, etc.
        siteUrl : 'https://sharedlatex.mydomain.tld'

        # Same, but with http auth credentials.
        httpAuthSiteUrl: 'https://#{httpAuthUser}:#{httpAuthPass}@sharedlatex.mydomain.tld'

        # ...

        defaultFeatures: defaultFeatures =
                collaborators: -1
                dropbox: false # disable dropbox because it is not yet open sourced
                versioning: true
        # ...

        email:
                fromAddress: "sharelatex@mydomain.tld"
        # The default replyTo field, if it should be set
                replyTo: "sharelatex@mydomain.tld"
                transport: "Sendmail" # or whatever you need
                parameters:
                        # important if you use Sendmail because the parameters option
                        # cannot be empty
                        args: []

        # ...

        # Should javascript assets be served minified or not. Note that you will
        # need to run `grunt compile:minify` within the web-sharelatex directory
        # to generate these.
        useMinifiedJs: true

        # Should static assets be sent with a header to tell the browser to cache
        # them.
        cacheStaticAssets: true

        # If you are running ShareLaTeX over https, set this to true to send the
        # cookie with a secure flag (recommended).
        secureCookie: true
```

nginx and upstart script
========================

My very simple upstart script for this in
`/etc/init/sharelatex.mydomain.tld.node.conf`:

``` {.sourceCode .shell-session}
description "Node server for sharelatex.mydomain.tld"

setuid myuser
setgid mygroup

start on net-device-up
stop on shutdown

respawn

chdir /path/to/sharelatex/
exec grunt run
```

And the nginx config in
`/etc/nginx/sites-available/sharelatex.mydomain.tld.conf`:

``` {.sourceCode .nginx}
server {
        listen       80;
        server_name  sharelatex.mydomain.tld;
        # force redirect http to https
        rewrite ^/(.*) https://$server_name/$1 permanent; 
}
server {
    listen 443 ssl;

    set $static_path /path/to/sharelatex/web/public/;


    server_name sharelatex.mydomain.tld;
    # certificate from startssl
    # unified for nginx
    # see https://www.startssl.com/?app=42
    ssl_certificate /etc/nginx/ssl/sharelatex.mydomain.tld-unified.crt;
    ssl_certificate_key /etc/ssl/private/sharelatex.mydomain.tld.key;

    location / {
        proxy_pass    http://localhost:3000/;
        # to tell the node server that this is https
        proxy_set_header        X-Forwarded-Proto $scheme;
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
```

To start everything:

``` {.sourceCode .shell-session}
$ sudo start sharelatex.mydomain.tld.node
$ sudo /etc/init.d/nginx reload
```

Things that are not (yet) available
===================================

In reference of the [ShareLaTeX
wiki](https://github.com/sharelatex/sharelatex/wiki/Things-not-included-in-these-repo%27s)

/templates
----------

Templates will be included when that part of the page is redesigned.

/dropbox
--------

Will be rewritten and then open sourced. If the feature currently is
enabled, it will crash the application if accessed.

help pages
----------

Those are links to an instance of media wiki which will not be included
in the repos.

chat
----

The chart is not yet open source but the application will work fine
without it.