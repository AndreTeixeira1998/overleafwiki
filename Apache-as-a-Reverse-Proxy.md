The ShareLaTeX web service runs on ports 3000 and 3026 by default.  Apache comes pre-installed on most Linux/OS X distributions, and is the default web server.  Reverse proxy options are more limited than Nginx, but it should be sufficient for small-scale deployments.

First, make sure that `mod_proxy` is being loaded by making sure that the appropriate line of the Apache configuration file (`/etc/httpd/conf/httpd.conf` on Redhat), 

```
LoadModule proxy_module modules/mod_proxy.so
```

You'll need to use `VirtualHost`ing.  Enable it for all hosts on port 80 (and any other ports you need).

```
NameVirtualHost *:80
```

Assuming you've set up ShareLaTeX on the default ports: 3000 and 3026, then the following VirtualHost definition will work once you replace ServerName with the domain name that you're redirecting.

```
<VirtualHost *:80>
    ServerName [[[INSERT YOUR HOSTNAME HERE]]]
    ProxyPreserveHost On

    <LocationMatch "/socket.io">
      ProxyPass http://localhost:3026/socket.io
      ProxyPassReverse http://localhost:3026/socket.io
    </LocationMatch>

    <LocationMatch "/">
      ProxyPass http://localhost:3000/
      ProxyPassReverse http://localhost:3000/
    </LocationMatch>
</VirtualHost>
```

Note that this configuration relies on Node for static asset hosting.