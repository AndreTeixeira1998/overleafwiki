A single ShareLaTeX user is responsible to publishing the curated list templates which are visable of /templates. To set this use the env var `SHARELATEX_TEMPLATES_USER_ID`

`--env SHARELATEX_TEMPLATES_USER_ID=56a8865231faeb5f07d69959`

To obtain the user id of the user you wish to publish public templates with you can either go via the admin panel or via Mongo.

#### Admin panel
![User if shown in admin panel](https://raw.githubusercontent.com/wiki/sharelatex/sharelatex/user_id_in_admin_panel.png)


#### Via mongo
```
> mongo
> use sharelatex
switched to db sharelatex
> db.users.find({email:"sally@sharelatex.com"}, {_id:1})
{ "_id" : ObjectId("55d5d16ad0904571699304d4") }
>
```

#### Templates on index page
For the templates index page templates they are grouped by tags which the user puts the projects in, e.g. Journals, Reports etc. To see all templates add /all to the url `/templates/all` which can also be used as the default url if you do not wish to use the tags for grouping.
