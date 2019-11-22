The Overleaf Server Pro image is used and configured in the same way as the open source Overleaf Docker image. Instead use the image `quay.io/sharelatex/sharelatex-pro` in your [docker-compose.yml](https://github.com/sharelatex/sharelatex/blob/master/docker-compose.yml).


## Obtaining Server Pro Image

First use your Server Pro credentials to log in:

```
docker login quay.io
Username: <sharelatex+your_key_name>
Password: <your key>
```

Then you can pull the image using Docker:

```
$ docker pull quay.io/sharelatex/sharelatex-pro:latest
```

## A Note on the Host Environment and Support

We _strongly_ recommend you run Server Pro on a recent version of Ubuntu, with a recent version of Docker. It's possible to run Server Pro on many other systems, but it will be much more difficult for us to offer support for systems that are far from the default Ubuntu/Docker setup. We also can't offer support for problems with Docker itself.


## Updating Image versions

First of all, read the instructions in [Upgrading Containers](https://github.com/overleaf/overleaf/wiki/Upgrading-Containers) section.

If you're explicit version numbering for the Docker images in [docker-compose.yml](https://github.com/sharelatex/sharelatex/blob/master/docker-compose.yml). For example, `quay.io/sharelatex/sharelatex-pro:2.0.0`. In this case, use the new version number should be enough.

If you're not using any version number (as in `quay.io/sharelatex/sharelatex-pro`), or you're using `latest` (`quay.io/sharelatex/sharelatex-pro:latest`), you need to `pull` again the latest version of the image before restarting Server Pro:

```
$ docker pull quay.io/sharelatex/sharelatex-pro:latest
```

## Where to go next

Basic configuration can be found on the [quickstart guide](https://github.com/sharelatex/sharelatex/wiki/Quick-Start-Guide) page, with customizations such as [LDAP](https://github.com/sharelatex/sharelatex/wiki/Server-Pro:-LDAP-Config) and [Sandboxed Compiles](https://github.com/sharelatex/sharelatex/wiki/Server-Pro:-sandboxed-compiles) available in the Server Pro image.