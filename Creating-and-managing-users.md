## Creating the first admin user

Once the Overleaf instance is running, visit the `/launchpad` page to set up your first admin user.


Alternatively, run the following command against your docker container to create your first user and make them an admin:

```
$ docker exec sharelatex /bin/bash -c "cd /var/www/sharelatex; grunt user:create-admin --email=joe@example.com"

```

This will create a user with the given email address if they don't already exist, and make them an admin user. In the command output, you will be given a URL to visit where you can set the password for this user and log in for the first time.

## Deleting Users
User can be deleted via the following command, projects will also be deleted so be careful with this.

```
$ docker exec sharelatex /bin/bash -c "cd /var/www/sharelatex; grunt user:delete --email joe@example.com"

```
## Creating normal users

Once you are logged in as an admin user, you can visit `/admin/register` on your Overleaf instance and create a new users. If you have an email backend configured in your settings file, the new users will be sent an email with a URL to set their password. If not, you will have to distribute the password reset URLs manually. These are shown when you create a user.