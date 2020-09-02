----

## ! Announcing the Overleaf Toolkit !

The [Overleaf Toolkit](https://github.com/overleaf/toolkit) is the new way to deploy and manage Overleaf Community Edition and Server Pro.

We encourage new Overleaf users to get started with the Toolkit [Quick Start Guide](https://github.com/overleaf/toolkit/blob/master/doc/quick-start-guide.md), instead of using this guide. The instructions on this wiki will remain in place for existing docker-compose based deployments.

----




**Make sure you have read this guide before running Overleaf with any important data** 

Overleaf is installed run via [Docker](https://www.docker.com/), the following command will pull down the latest version:

```
$ docker pull sharelatex/sharelatex
```

If you are using Overleaf Server Pro the image name will be [slightly different](https://github.com/overleaf/overleaf/wiki/Server-Pro:-setup).

### Using a compose file

It is recommended to use our [docker-compose.yml](https://github.com/overleaf/overleaf/blob/master/docker-compose.yml) file to get up and running. In addition to installing and starting Overleaf it also setup [redis](https://redis.io/) and [mongodb](https://www.mongodb.com/), with an option of HTTPS via [nginx proxy](https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/). The docker compose file also allows for a nice interface for passing environment variables to Overleaf.

### Operating systems
We recommend a debian based operating system such as Ubuntu for Overleaf, this is what the software has been developed using and most people use when running Overleaf.

### Storing Data & Backups

The directory you mount at `/var/lib/sharelatex` is where Overleaf will store data such as images, this is what you will need to backup, in addition to mongodb & redis. The example mounts it from  `~/sharelatex_data`t but it can be anywhere accessible from the docker image. 

### LaTeX environment

To save bandwidth, the Overleaf image only comes with a minimal install of [TeXLive](https://www.tug.org/texlive/). To upgrade to a complete TeXLive installation, run the installation script in the Overleaf container with the following command:

```bash
$ docker exec sharelatex tlmgr install scheme-full
```

Alternatively you can install packages manually as you need by replacing `scheme-full` with the package name.

Server Pro users have the option of using [Sandbox Compiles](https://github.com/sharelatex/sharelatex/wiki/Server-Pro:-sandboxed-compiles), which will automatically pull down a full TexLive image. 


### Creating and Managing users

Once the Overleaf instance is running, visit the `/launchpad` page to set up your first admin user. 

Altenatively, use the following command to create your first user and make them an admin:

```
$ docker exec sharelatex /bin/bash -c "cd /var/www/sharelatex; grunt user:create-admin --email=joe@example.com"
```

This will create a user with the given email address if they don't already exist, and make them an admin user. You will be given a URL to visit where you can set the password for this user and log in for the first time.

**NOTE**: the command above will always yield a URL pointing to `http://localhost/` if you've updated your port forwarding to something like `ports: - 8080:80`, you should use the correct port to visit the password confirmation page: `http://localhost:8080/user/password/set?passwordResetToken=<token>`. Another option is to set `SHARELATEX_SITE_URL` environment variable to `http://localhost:8080` or `http://sharelatex.mydomain.com`.

**Creating normal users**

Once you are logged in as an admin user, you can visit `/admin/register` on your Overleaf instance and create new users. If you have an [email backend configured](https://github.com/overleaf/overleaf/wiki/Configuring-SMTP-Email) the new users will be sent an email with a URL to set their password. If not, you will have to distribute the password reset URLs manually. These are shown when you create a user.

