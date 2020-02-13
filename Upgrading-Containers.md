## Upgrading from older versions

> ⚠️ **Warning**: Please make sure to back up all Mongo, Redis and on-disk data before upgrading

### Migrations

Data stored in MongoDB will be automatically migrated to the latest schema when upgrading docker releases. **This can make downgrades impossible.** 

It is easy to test the migration first. This can be done by copying the MongoDB database and doing a test run against the copied data:

```shell
# open a mongo shell and create a copy of the database
$ docker-compose exec mongo mongo
> db.copyDatabase("sharelatex","sharelatex-copy")
```

Update `docker-compose.yml` to point to the new DB and restart Overleaf:

```
SHARELATEX_MONGO_URL=mongodb://dockerhost/sharelatex-copy
```

### Closing Editor

Todo a more seamless migration of containers you can close the editor before shutting the container down. This can be done via the `close editor` tab in the `Admin -> Manage Site` panel. There are 2 buttons

* Close Editor - Stops anyone trying to load up the editor
* Disconnect all users - Kicks anyone who is currently connected to the editor forcing a refresh. If the editor is closed the refresh will not take them back into the editor.

Once the editor is closed the only way to open is to restart the docker container.

### Upgrade process

To use the new docker container stop and remove the currently running Overleaf container:

```
$ docker-compose stop sharelatex
$ docker-compose rm sharelatex
```

Checkout the latest version:

```
$ docker pull sharelatex/sharelatex:latest
```

Restart the container, that will be using the updated version of Overleaf:

```
$ docker-compose up -d sharelatex
```