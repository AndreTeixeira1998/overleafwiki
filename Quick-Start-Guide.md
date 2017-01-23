**Make sure you have read this guide before running ShareLaTeX with any important data** 

ShareLaTeX is installed run via [Docker](https://www.docker.com/), the following command will pull down the latest version and get it up and running for you:

```
$ docker run -d \
  -v ~/sharelatex_data:/var/lib/sharelatex \
  -p 5000:80 \
  --name=sharelatex \
  sharelatex/sharelatex
```

This will download the ShareLaTeX image and start it running in the background on port 5000. You should be able to access it at `http://localhost:5000/` assuming it can talk to the required mongodb & redis instances.

If you are using ShareLaTeX Server Pro the image name will be [slightly different](https://github.com/sharelatex/sharelatex/wiki/Server-Pro:-setup).

### Using a compose file

It is also recommended to use our [docker-compose.yml](https://github.com/sharelatex/sharelatex/blob/master/docker-compose.yml) file to get up and running. In addition to installing and starting ShareLaTeX it also setup redis and mongodb, with an option of https. The docker compose file also allows for a nice interface for passing settings to ShareLaTeX.

### Operating systems
We recommend a debian based operating system such as Ubuntu for ShareLaTeX, this is what the software has been developed using and most people use when running ShareLaTeX.

### Storing Data & Backups

The directory you mount at /var/lib/sharelatex is where ShareLaTeX will store data such as images, this is what you will need to backup, in addition to mongodb & redis. The example mounts it from  ~/sharelatex_data but it can be anywhere accessible from the docker image. 

### LaTeX environment

To save bandwidth, the ShareLaTeX image only comes with a minimal install of TeXLive. To upgrade to a complete TeXLive installation, run the following command:

```
$ docker exec sharelatex tlmgr install scheme-full
```

Or you can install packages manually as you need by replacing `scheme-full` by 
the package name.

Server Pro users have the option of [Sandbox Compiles](https://github.com/sharelatex/sharelatex/wiki/Server-Pro:-sandboxed-compiles) which will automatically pull down a full tex live image. 


### Creating and Managing users

Once the sharelatex instance is running, visit the `/launchpad` page to set up your first admin user. 

Altenatively, use the following command to create your first user and make them an admin:

```
$ docker exec sharelatex /bin/bash -c "cd /var/www/sharelatex; grunt user:create-admin --email joe@example.com"
```

This will create a user with the given email address if they don't already exist, and make them an admin user. You will be given a URL to visit where you can set the password for this user and log in for the first time.

**Creating normal users**

Once you are logged in as an admin user, you can visit `/admin/register` on your ShareLaTeX instance and create a new users. If you have an email backend configured in your settings file, the new users will be sent an email with a URL to set their password. If not, you will have to distribute the password reset URLs manually. These are shown when you create a user.

