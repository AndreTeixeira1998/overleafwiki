From version `2.0.0` until `2.4.2` we haven't been too specific about which version of Mongo should be used with Overleaf Community Edition and Overleaf Server Pro, with the exception for the minimum supported version specified in [supported dependencies list](https://github.com/overleaf/overleaf/wiki/Dependencies). 

Starting on `2.5.0`, any new release will indicate any change on the supported version of Mongo in its [release notes](https://github.com/overleaf/overleaf/wiki#release-notes).


Similarly, the version of [`mongo` docker image](https://hub.docker.com/_/mongo) in [`docker-compose.yml`](https://github.com/overleaf/overleaf/blob/master/docker-compose.yml) has been historically untagged. Starting on Overleaf CE/SP `2.5.0`, the tag will be updated to the version supported by the latest release of Overleaf CE/SP.

### Should I update Mongo?

You should **only** consider updating Mongo version if you're planning to upgrade your instance of Overleaf CE/SP. 

If you experience a specific problem that you think might be related to your current version of Mongo, feel free to [raise an issue](https://github.com/overleaf/overleaf/issues) (CE users) or contact support (SP users).


### Checking your mongo version

Opening the `mongo` shell should immediately print the current version.

```
➤ docker-compose exec mongo mongo -version
MongoDB shell version v4.4.1
```

## Update Process

Updating the version of Mongo during an upgrade of your Overleaf CE/SP instance looks as follows:

1. Decide the version of Overleaf CE/SP you plan to upgrade to.
1. Find the version of mongo recommended by that specific Overleaf CE/SP release.
1. Follow the instructions to upgrade Mongo to the target version.
1. Upgrade Overleaf CE/SP image version and restart the instance.

Our recommendation is to always upgrade Overleaf CE/SP **to the latest version available**, since it's always guaranteed to be supported (Server Pro Users only). In case you decide to go to an earlier version, this table shows the recommended version of Mongo for earlier releases of Overleaf CE/SP.

| CE/SP Version | Mongo Version |
| ------------- |---------------| 
| >=2.0.0       | 3.4           | 
| >=2.1.0       | 3.6           | 
| >=2.5.0       | 4.4           | 

### Upgrading Mongo

Mongo requires **step-by-step upgrades**. That means you can't go straight from, let's say `3.2` to `3.6`. You need first to update `3.2` to `3.4`, and then `3.4` to `3.6` (Mongo uses even numbers for their stable versions).

**Update instructions**

- [From `3.2` to `3.4`](https://docs.mongodb.com/manual/release-notes/3.4-upgrade-standalone)
- [From `3.4` to `3.6`](https://docs.mongodb.com/manual/release-notes/3.6-upgrade-standalone)
- [From `3.6` to `4.0`](https://docs.mongodb.com/manual/release-notes/4.0-upgrade-standalone)
- [From `4.0` to `4.2`](https://docs.mongodb.com/manual/release-notes/4.2-upgrade-standalone)
- [From `4.2` to `4.4`](https://docs.mongodb.com/manual/release-notes/4.4-upgrade-standalone)

In most cases the update just requires setting up a compatibility setting before actually updating the version. Let's see an example.

### Example: upgrading Mongo from `3.4` to `3.6`

Let's start by making sure we're running Mongo `3.4`: 

```
➤ docker-compose exec mongo mongo -version
MongoDB shell version v3.4.24
```

According to the [upgrade instructions](https://docs.mongodb.com/manual/release-notes/3.6-upgrade-standalone/#upgrade-version-path), the only requirement is to have `featureCompatibilityVersion` set to `3.4`. We do so by opening a mongo shell and running the indicated command:

```
➤ docker-compose exec mongo mongo
MongoDB shell version v3.4.24
connecting to: mongodb://127.0.0.1:27017
MongoDB server version: 3.4.24
Welcome to the MongoDB shell.
For interactive help, type "help".
> db.adminCommand( { setFeatureCompatibilityVersion: "3.4" } )
{ "ok" : 1 }
> exit
bye
```

We'll then stop the Overleaf CE/SP and Mongo instances (`docker-compose down`), update [`docker-compose.yml`](https://github.com/overleaf/overleaf/blob/master/docker-compose.yml) to use `image: mongo:3.6`, and then restart mongo (`docker-compose up mongo`) to verify the update went smoothly.

Finally, we'll update Overleaf CE/SP image version to our target version and restart all the services (`docker-compose up`).