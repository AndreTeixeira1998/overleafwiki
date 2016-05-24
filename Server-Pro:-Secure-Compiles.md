ShareLaTeX Server Pro comes with the option to run compiles in a secured sandbox environment for enterprise security. It does this by running every project in its own secured docker environment. 

##### Enable

To enable this set `--env DOCKER_IN_DOCKER='true'` when creating the container

##### Build Image

Once the container has been created you can build the secure image with the following command:

`docker exec sharelatex docker build --tag sharelatex-texlive /root/docker-image`