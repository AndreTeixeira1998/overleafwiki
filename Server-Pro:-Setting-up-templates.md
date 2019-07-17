### Setting up the Templates User

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

### Publishing Templates

For each template you want to upload:

1. Log in as the templates user.
2. As the templates user, create a project containing the template's source code and make sure it compiles.
3. In the editor's left hand menu, choose Publish as Template.

All templates published this way from the templates user's account will be listed as templates.

### Configuring Links to Templates

#### Templates on index page
On the templates index page, /templatex, templates are nested under folders which the user puts the projects in, e.g. Journals, Reports etc. To see all templates add /all to the url `/templates/all` which can also be used as the default url if you do not wish to use the folder for grouping.


#### Template links on project list page
When a user creates a new project they can be shown links to templates. These are set via the  SHARELATEX_NEW_PROJECT_TEMPLATE_LINKS variable

`SHARELATEX_NEW_PROJECT_TEMPLATE_LINKS='[
   {"name":"All Templates","url":"/templates/all"},
   {"name":"All Categories","url":"/templates"},
   {"name":"reports","url":"/templates/reports"},  {"name":"External","url":"https://somewhere.com/templates/reports"}
]'`

![ShareLaTeX Template Links](https://raw.githubusercontent.com/wiki/sharelatex/sharelatex/images/new_project_template_links.png)
