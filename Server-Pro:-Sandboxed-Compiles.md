Overleaf Server Pro comes with the option to run compiles in a secured sandbox environment for enterprise security. It does this by running every project in its own secured docker environment. 

> ⚠️ **Warning**: previous versions of Server Pro supported a docker-in-Docker setup for Sandboxed Compiles. This is no longer supported starting at v2.0. Original instructions to setup docker-in-docker [are available here](https://github.com/overleaf/overleaf/wiki/Docker-on-Docker-compiles)


### Setup

Running Docker inside Docker can be hard to set up, and in many cases simply can't be made to work well. For those situations, we use instead Sandboxed Compiles with "Sibling Containers" instead of the a docker-in-docker setup. (From version 0.5.11 onwards)

With Sibling Containers, the Overleaf container doesn't start the compiler containers inside of itself, instead it communicates with the Host docker instance to spawn the compilers on the Host, alongside of itself.

To make this work we need to do a few things:

- Mount the host docker socket into the Overleaf container
- Set the `SANDBOXED_COMPILES_SIBLING_CONTAINERS` environment variable to `true`
- Set the `SANDBOXED_COMPILES_HOST_DIR` to the path on the host where the compile files will be located

Notably, the `privileged` flag is not required when using sibling containers.

### Enabling Sibling Containers

Set the environment variables `DOCKER_RUNNER`, `SANDBOXED_COMPILES` and `SANDBOXED_COMPILES_SIBLING_CONTAINERS` to `true`.

Example:

```
--env DOCKER_RUNNER='true'
--env SANDBOXED_COMPILES='true'
--env SANDBOXED_COMPILES_SIBLING_CONTAINERS='true'
```


### Mounting the docker socket

We need to mount the host docker socket (usually `/var/run/docker.sock`) into the Overleaf container.
In this example, note the addition of an extra `-v` option with `/var/run/docker.sock:/var/run/docker.sock`.

```
docker run -d \
  -v ~/sharelatex_data:/var/lib/sharelatex \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -p 5000:80 \
  --name=sharelatex \
  quay.io/sharelatex/sharelatex-pro
```


It is important that the docker socket is owned by a group with the group id of `999` to match the group id used inside of the Overleaf container. 


Check the group, in this case `docker`
```
> ls -l /var/run/docker.sock
srw-rw---- 1 root docker 0 May 31 08:57 /var/run/docker.sock
```

See the groupid of the docker group is 999
```
> getent group docker
docker:x:999:
```


### Mapping the host compile directory

The Overleaf container writes its data to a directory which is mounted from the host. For this example, let's say that this mount is `/data/sharelatex_data:/var/lib/sharelatex`.

Within that directory, there will be a `data/compiles` directory. Because the compiler containers will be spawned on the Host (not inside the Overleaf container), we need to specify the full path to that directory on the host:

```
--env SANDBOXED_COMPILES_HOST_DIR='/data/sharelatex_data/data/compiles'
```


### An example docker-compose file

In this example, note the following:

- the docker socket volume mounted into the Overleaf container
- `DOCKER_RUNNER` set to "true"
- `SANDBOXED_COMPILES` set to "true"
- `SANDBOXED_COMPILES_SIBLING_CONTAINERS` set to "true"
- `SANDBOXED_COMPILES_HOST_DIR` set to "/data/sharelatex_data/data/compiles", the place on the host where the compile data will be written

```yaml
version: '2'
services:
    sharelatex:
        restart: always
        image: quay.io/sharelatex/sharelatex-pro:latest
        container_name: sharelatex
        depends_on:
            - mongo
            - redis
        ports:
            - 80:80
        links:
            - mongo
            - redis
        volumes:
            - /data/sharelatex_data:/var/lib/sharelatex
            - /var/run/docker.sock:/var/run/docker.sock    #### IMPORTANT
        environment:
            SHARELATEX_MONGO_URL: mongodb://mongo/sharelatex
            SHARELATEX_REDIS_HOST: redis
            SHARELATEX_APP_NAME: 'My ShareLaTeX'
            SHARELATEX_SITE_URL: "http://sharelatex.mydomain.com"
            SHARELATEX_NAV_TITLE: "Our ShareLaTeX Instance"
            SHARELATEX_ADMIN_EMAIL: "support@example.com"
            ...
            DOCKER_RUNNER: "true"
            SANDBOXED_COMPILES: "true"
            SANDBOXED_COMPILES_SIBLING_CONTAINERS: "true"    #### IMPORTANT
            SANDBOXED_COMPILES_HOST_DIR: "/data/sharelatex_data/data/compiles"  #### IMPORTANT
```

### Changing the TexLive Image

Server Pro uses two environment variables to determine which texlive images to use for sandboxed compiles: 

- `TEX_LIVE_DOCKER_IMAGE`: name of the default image for new projects
- `ALL_TEX_LIVE_DOCKER_IMAGES`: comma-separated list of all images in use

The current default is `quay.io/sharelatex/texlive-full:2017.1`, but you can override these values in the `environment` section of the docker-compose file.

Here's an example where we default to texlive 2018 for new projects, and keep both 2018 and 2017 in use for older projects:

```
    TEX_LIVE_DOCKER_IMAGE: "quay.io/sharelatex/texlive-full:2018.1"
    ALL_TEX_LIVE_DOCKER_IMAGES: "quay.io/sharelatex/texlive-full:2018.1,quay.io/sharelatex/texlive-full:2017.1"
```