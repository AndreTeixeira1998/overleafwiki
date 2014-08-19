# Frequently Asked Questions

**How do I run ShareLaTeX with SSL?**

You'll need to set up SSL in a front-end web server, like Nginx. There are plenty of guides to this available (e.g. https://www.digitalocean.com/community/tutorials/how-to-create-a-ssl-certificate-on-nginx-for-ubuntu-12-04), and you can base your Nginx config on the page at [[Nginx as a Reverse Proxy]].

In your ShareLaTeX config file you'll need to add the line:

```
behindProxy: true
```

to let ShareLaTeX know that it can trust the X-Forwarded-Proto header and should behave as though on HTTPS even though it is getting proxied requests over HTTP. If you only serve ShareLaTeX over SSL (recommended), then you can also set the `secureCookie` setting to only send the ShareLaTeX login cookies over a secure connection (recommended):

```
secureCookie: true
```