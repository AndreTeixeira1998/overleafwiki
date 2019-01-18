The ShareLaTeX Server Pro image is used and configured in the same way as the open source ShareLaTeX Docker image. Instead use the image `quay.io/sharelatex/sharelatex-pro.` in your [docker-compose.yml](https://github.com/sharelatex/sharelatex/blob/master/docker-compose.yml).

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

Basic configuration can be found on the [quickstart guide](https://github.com/sharelatex/sharelatex/wiki/Quick-Start-Guide) page, with customizations such as [LDAP](https://github.com/sharelatex/sharelatex/wiki/Server-Pro:-LDAP-Config) and [Sandboxed Compiles](https://github.com/sharelatex/sharelatex/wiki/Server-Pro:-sandboxed-compiles) available in the Server Pro image.