# Frequently Asked Questions

#### I get an EADDRINUSE Error

There is probably a run away ShareLaTeX process running on one of the ports. Run `killall node` to get rid off it and try again.

#### I get an "Error: Could not load the bindings file" error

For some reason the bcrypt library does not always install correctly when running `grunt install`. You can reinstall it manually in the web directory:

```bash
$ cd web
$ rm -r node_modules/bcrypt
$ npm install bcrypt
```

If anyone can figure out why this happens I will be very grateful, because I don't know why this happens.

#### Syncing between code and PDF doesn't work

This relies on a custom binary to work. Go into the clsi directory and run `grunt compile:bin`. If there are warnings, then it will hopefully give you a clue what to fix. Note that you need the zlib headers:

```
$ sudo apt-get install zlib1g-dev
```

#### Spell Check doesn't work

Check that you have `aspell` installed and in your path. You can run `grunt check:aspell` to verify this and see a list of installed dictionaries. If you want to add additional languages beyond English, then add the following lines into your config:

```
languages: [
    {name: "English", code: "en"},
    {name: "French", code: "fr"}
]
```

where the `code` corresponds to aspell dictionary name.

#### How do I run ShareLaTeX with SSL?

You'll need to set up SSL in a front-end web server, like Nginx. There are plenty of guides to this available (e.g. https://www.digitalocean.com/community/tutorials/how-to-create-a-ssl-certificate-on-nginx-for-ubuntu-12-04), and you can base your Nginx config on the page at [[Nginx as a Reverse Proxy]].

In your ShareLaTeX config file you'll need to add the line:

```
behindProxy: true
```

to let ShareLaTeX know that it can trust the X-Forwarded-Proto header and should behave as though on HTTPS even though it is getting proxied requests over HTTP. If you only serve ShareLaTeX over SSL (recommended), then you can also set the `secureCookie` setting to only send the ShareLaTeX login cookies over a secure connection (recommended):

```
secureCookie: true
```