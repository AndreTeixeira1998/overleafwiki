Overleaf currently support the following versions of dependencies:

- **Docker 19.03**
- **MongoDB 3.6**
- **Redis 5**

`docker-compose` is generally installed with Docker.

MongoDB and Redis are automatically pulled by `docker-compose` when running Overleaf CE or Server Pro, unless configured to use a different installation.

Once docker is installed correctly, you should be able to run these commands without error:

```
docker --version
docker-compose --version
docker ps
```

Once you have Docker and `docker-compose` successfully installed, have a look at the [[Quick Start Guide]] to download the `docker-compose.yml` and start Overleaf.