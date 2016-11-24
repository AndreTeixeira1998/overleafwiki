ShareLaTeX Server Pro comes with the option to run compiles in a secured sandbox environment for enterprise security. It does this by running every project in its own secured docker environment. 

##### Enable

To enable this set `--env SANDBOXED_COMPILES='true' --privileged` when creating the container


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