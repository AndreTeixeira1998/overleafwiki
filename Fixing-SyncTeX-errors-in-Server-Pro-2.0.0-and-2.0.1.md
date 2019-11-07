Unfortunately Server Pro in its versions `2.0.0` and `2.0.1` has been released with a problem with SyncTeX functionality, causing the server to return a `400` error every time we try to sync the position between a document and the generated pdf. 

This problem is present only when using [Sandboxed Compiles](https://github.com/overleaf/overleaf/wiki/Server-Pro:-sandboxed-compiles).

It's possible to fix the issue with a workaround that involves:

1. The addition of a new environment variable to `sharelatex` container in `docker-compose.yml`
1. Executing a command after `sharelatex` container has been initiated.

### Step 1: adding `SYNCTEX_BIN_HOST_PATH` to `docker-compose.yml`

We need to add a new `SYNCTEX_BIN_HOST_PATH`, pointing to a `synctex` executable in the same host directory mounted as `/var/lib/sharelatex` inside the container. This means that if the volume is mounted as:

```yaml
volumes:
  - /var/sharelatex_data:/var/lib/sharelatex
```

Then the new environment variable should be:

```yaml
SYNCTEX_BIN_HOST_PATH: "/var/sharelatex_data/synctex"
```

### Step 2: executing a command inside `sharelatex` container

Once we've restarted the `sharelatex` container after adding the environment variable we need to execute the following command:

```
docker exec sharelatex cp /var/www/sharelatex/clsi/bin/synctex /var/lib/sharelatex/synctex 
```

Once that's done, we should start seeing `200` responses when syncing the position between document and pdf.

Executing again this command shouldn't be required after a service restart.