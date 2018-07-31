## Data Security
No data is sent to 3rd parties, including ShareLaTeX.com.

## Sending messages to all users
The admin panel allows for a banner to set for all users

## Deleting users
ShareLaTeX Server Pro's admin panel has the option to delete users, it can also be done via mongo command line

`db.users.remove({email:"delete_this_user@gmail.com"})`

If you are running MongoDB under docker, you can get a mongo shell with:
```
docker-compose exec mongo bash
mongo
use sharelatex
```

## Maximum size of projects in ShareLaTeX
* The maximum amount of editable data in ShareLaTeX is 5mb, this does not include images or other large non editable files.
* The maximum number of entities is 2000 per project (folder, docs, images)
* The maximum size of an non editable entity is 50mb

## Maximum size of uploads
Uploads are limited to 50mb per entity.

## What is a public project?
A public project is a project which has had all authentication removed from it so anyone can access it. It will not show up on other users project lists.

## Is reference search and mendeley available?
The advance reference features  in ShareLaTeX are not currently available. We do have plans to add them into server pro in the future but they require increased hosting requirements and complexity so have held back from including them to date.