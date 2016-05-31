ShareLaTeX Server Pro comes with the option to run compiles in a secured sandbox environment for enterprise security. It does this by running every project in its own secured docker environment. 

##### Enable

To enable this set `--env SANDBOXED_COMPILES='true'` when creating the container


Once the docker container is running it will take **10-15 minutes for compiles to work** while secure version of texlive is installed in the background