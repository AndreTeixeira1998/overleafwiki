## Upgrading from older versions

*Please make sure to back up all Mongo, Redis and on-disk data before upgrading.*

### Migrations
Data stored in Mongodb will be automatically migrated to the latest schemea when upgrading docker releases. **This can make downgrades impossible.** 

It is easy to test the migration first. This can be done by copying the mongodb database and doing a test run against the copied data.

```
db.copyDatabase(sharelatex,sharelatex-copy)
# start the container up pointing at the new db
--env SHARELATEX_MONGO_URL=mongodb://dockerhost/sharelatex-copy
```

### Closing Editor
Todo a more seamless migration of containers you can close the editor before shutting the container down. This can be done via the `close editor` tab in https://www.sharelatex.com/admin. There are 2 buttons

* Close Editor - Stops anyone trying to load up the editor
* Disconnect all users - Kicks anyone who is currently connected to the editor forcing a refresh. If the editor is closed the refresh will not take them back into the editor.

Once the editor is closed the only way to open is to restart the docker container.

### Upgrade process
To use the new docker container stop and remove the currently running ShareLaTeX container:

```
$ docker stop sharelatex
$ docker rm sharelatex
```

Start a new container with the updated version of ShareLaTeX (to upgrade to version 0.2.0 for example):

```
$ docker run -d -v ~/sharelatex_data:/var/lib/sharelatex --name=sharelatex sharelatex/sharelatex:0.2.0
```