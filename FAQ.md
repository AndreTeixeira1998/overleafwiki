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
use sharelatex
```

## Maximum size of projects in Overleaf
* The maximum amount of editable data in Overleaf is 5mb, this does not include images or other large non-editable files.
* The maximum number of entities is 2000 per project (folder, docs, images).
* The maximum size of a non-editable entity is 50mb.

## Maximum size of uploads
Check [Learn resources](https://www.overleaf.com/learn/how-to/Uploading_a_project#Limitations_on_Uploads) for information on limits.

## What is a public project?
A public project is a project which has had all authentication removed from it so anyone can access it. It will not show up on other users' project lists.

## Is reference search and Mendeley available?
The advanced reference features in Overleaf are not currently available. We do have plans to add them into server pro in the future but they require increased hosting requirements and complexity so have held back from including them to date.