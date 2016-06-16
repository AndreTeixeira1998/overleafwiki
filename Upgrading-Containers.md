### Upgrading from older versions

*Please make sure to back up all Mongo, Redis and on-disk data before upgrading.*

#### Migrations
Data stored in Mongodb will be automatically migrated to the latest schemea when upgrading docker releases. **This can make downgrades impossible.** 

It is to test the migration first. This can be done by copying the mongodb database and doing a test run against the copied data.

```
db.copyDatabase(sharelatex,sharelatex-copy)
# start the container up pointing at the new db
--env SHARELATEX_MONGO_URL=mongodb://dockerhost/sharelatex-copy
```

#### Upgrade process
To use the new docker container stop and remove the currently running ShareLaTeX container:

```
$ docker stop sharelatex
$ docker rm sharelatex
```

Start a new container with the updated version of ShareLaTeX (to upgrade to version 0.2.0 for example):

```
$ docker run -d -v ~/sharelatex_data:/var/lib/sharelatex --name=sharelatex sharelatex/sharelatex:0.2.0
```