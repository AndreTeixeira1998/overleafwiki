## Data Security
No data is sent to 3rd parties, including overleaf.com.

## Sending messages to all users
The admin panel allows for a banner to be set for all users

## Deleting users
Overleaf Server Pro's admin panel has the option to delete users, it can also be done via mongo command line

`db.users.remove({email:"delete_this_user@gmail.com"})`

If you are running MongoDB under docker, you can get a mongo shell with:
```
docker-compose exec mongo bash
mongo
> use sharelatex
```

## Counting users

Create a mongo shell and check the number of entries in the `users` collection:

```
docker-compose exec mongo bash
mongo
> use sharelatex
> db.users.count()
```

## Giving admin rights to an existing user.

Update the `isAdmin` field in the `users` collection:

```
db.users.updateOne({email:"<EMAIL_ADDRESS>"},{"$set": {isAdmin: true}})
```

```
docker-compose exec mongo bash
mongo
> use sharelatex
> db.users.updateOne({email:"user@example.com"},{"$set": {isAdmin: true}})
{ "acknowledged" : true, "matchedCount" : 1, "modifiedCount" : 1 }
```


## Maximum size of projects in Overleaf
Please see our [learn wiki](https://www.overleaf.com/learn/how-to/Uploading_a_project#Limitations_on_Uploads) for information on project size and upload limits.

## Maximum size of uploads
Please see our [learn wiki](https://www.overleaf.com/learn/how-to/Uploading_a_project#Limitations_on_Uploads) for information on project size and upload limits.

## What is a public project?
A public project is a project which has had all authentication removed from it so anyone can access it. It will not show up on other users' project lists.

## Is reference search and Mendeley available?
The advanced reference features in Overleaf are not currently available. We do have plans to add them into server pro in the future but they require increased hosting requirements and complexity so have held back from including them to date.