ShareLaTeX Server Pro comes with the option to run compiles in a secured sandbox environment for enterprise security. It does this by running every project in its own secured docker environment. 

##### Enable

To enable this set `--env SANDBOXED_COMPILES='true' --privileged` when creating the container.

You will also need to set `--privileged` flag to `true` for the sharelatex container.

Once the docker container is running it will take **10-15 minutes for compiles to work** while secure version of texlive is installed in the background


##### Potential issues

There is a debian kernel bug on 3.16 with auFS. Where you may see errors such as the following:

	ln: failed to create symbolic link 
	‘/sys/fs/cgroup/systemd/name=systemd’: Operation not permitted 
	mount: cgroup already mounted or cpu busy 
	mount: according to mtab, cgroup is already mounted on 
	/sys/fs/cgroup/cpu,cpuacct 
	rmdir: failed to remove ‘cpu’: Not a directory 
	mount: cgroup already mounted or cpuacct busy 
	mount: according to mtab, cgroup is already mounted on 
	/sys/fs/cgroup/cpu,cpuacct 
	rmdir: failed to remove ‘cpuacct’: Not a directory 
	mount: cgroup already mounted or net_cls busy 

Sandboxed compiles work by running docker inside of docker and 2 levels of auFS device storage conflicting. It can be circumvented it by mounting a normal host directory for the docker in docker process to write to using `-v /var/sharelatex/docker-images:/var/lib/docker` Where `/var/sharelatex/docker-images` is a local directory to store the docker images.


## Sibling Containers

Running Docker inside Docker can be hard to set up, and in many cases simply can't be made to work well. For those situations, we can use Sandboxed Compiles with "Sibling Containers" instead of the normal docker-in-docker setup. (From version 0.5.11 onwards)

With Sibling Containers, the Sharelatex container doesn't start the compiler containers inside of itself, instead it communicates with the Host docker instance to spawn the compilers on the Host, alongside of itself.

To make this work we need to do a few things:

- Mount the host docker socket into the sharelatex container
- Set the `SANDBOXED_COMPILES_SIBLING_CONTAINERS` environment variable to `true`
- Set the `SANDBOXED_COMPILES_HOST_DIR` to the path on the host where the compile files will be located

Notably, the `privileged` flag is not required when using sibling containers.

### Enabling Sibling Containers

Set the environment variable `SANDBOXED_COMPILES_SIBLING_CONTAINERS` to `true`.

Example:

```
--env SANDBOXED_COMPILES_SIBLING_CONTAINERS='true'
```


### Mounting the docker socket

We need to mount the host docker socket (usually `/var/run/docker.sock`) into the Sharelatex container.
In this example, note the addition of an extra `-v` option with `/var/run/docker.sock:/var/run/docker.sock`.

```
docker run -d \
  -v ~/sharelatex_data:/var/lib/sharelatex \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -p 5000:80 \
  --name=sharelatex \
  quay.io/sharelatex/sharelatex-pro
```

### Mapping the host compile directory

The sharelatex container writes it's data to a directory which is mounted from the host. For this example, let's say that this mount is `/data/sharelatex_data:/var/lib/sharelatex`.

Within that directory, there will be a `data/compiles` directory. Because the compiler containers will be spawned on the Host (not inside the sharelatex container), we need to specify the full path to that directory on the host:

```
--env SANDBOXED_COMPILES_HOST_DIR='/data/sharelatex_data/data/compiles'
```


### An example docker-compose file

In this example, note the following:

- the docker socket volume mounted into the sharelatex container
- `SANDBOXED_COMPILES_SIBLING_CONTAINERS` set to "true"
- `SANDBOXED_COMPILES_HOST_DIR` set to "/data/sharelatex_data/data/compiles", the place on the host where the compile data will be written

```
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
            SANDBOXED_COMPILES: "true"
            SANDBOXED_COMPILES_SIBLING_CONTAINERS: "true"    #### IMPORTANT
            SANDBOXED_COMPILES_HOST_DIR: "/data/sharelatex_data/data/compiles"  #### IMPORTANT
```



