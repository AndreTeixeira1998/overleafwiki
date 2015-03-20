## Creating the first admin user

In the sharelatex/web directory, run the following command to create your first user and make them an admin:

```
$ grunt create-admin-user --email joe@example.com
```

This will create a user with the given email address if they don't already exist, and make them an admin user. You will be given a URL to visit where you can set the password for this user and log in for the first time.

## Creating normal users

Once you are logged in as an admin user, you can visit `/admin/register` on your ShareLaTeX instance and create a new users. If you have an email backend configured in your settings file, the new users will be sent an email with a URL to set their password. If not, you will have to distribute the password reset URLs manually. These are shown when you create a user.