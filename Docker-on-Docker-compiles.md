> ⚠️ **Warning**: Please note that running docker-on-docker for compilation is unsupported since Server Pro v2.0 release. 

ShareLaTeX Server Pro comes with the option to run compiles in a secured sandbox environment for enterprise security. It does this by running every project in its own secured docker environment. 

In previous versions of Server Pro (<2.0) running Docker inside Docker was one of the supported options. This can be hard to set up, and in many cases simply can't be made to work well. For those situations, we can use Sandboxed Compiles with "Sibling Containers" instead of the normal docker-in-docker setup (from version 0.5.11 onwards). 

With Sibling Containers, the Sharelatex container doesn't start the compiler containers inside of itself, instead it communicates with the Host docker instance to spawn the compilers on the Host, alongside of itself.

Please [check the manual for instructions](https://github.com/sharelatex/sharelatex/wiki/Server-Pro:-sandboxed-compiles) to setup Sibling Containers.

### Enable

To enable this set `--env DOCKER_RUNNER='true' --env SANDBOXED_COMPILES='true' --privileged` when creating the container.

You will also need to set `--privileged` flag to `true` for the sharelatex container.

Once the docker container is running it will take **10-15 minutes for compiles to work** while secure version of texlive is installed in the background


#### Potential issues

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


### An example docker-compose file

In this example, note the following:

- the docker socket volume mounted into the sharelatex container
- `DOCKER_RUNNER` set to "true"
- `SANDBOXED_COMPILES_SIBLING_CONTAINERS` set to "true"
- `SANDBOXED_COMPILES_HOST_DIR` set to "/data/sharelatex_data/data/compiles", the place on the host where the compile data will be written

```
version: '2'
services:
    sharelatex:
        restart: always
        image: quay.io/sharelatex/sharelatex-pro:latest
        container_name: sharelatex
        privileged: true
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
```

