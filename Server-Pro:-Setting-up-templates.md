### Setting up the Templates User

A single Overleaf user is responsible to publishing the curated list templates which are visible on /templates. To set this use the env var `SHARELATEX_TEMPLATES_USER_ID`

`--env SHARELATEX_TEMPLATES_USER_ID=56a8865231faeb5f07d69959`

To obtain the user id of the user you wish to publish public templates, log in as the admin user you created when you set up Server Pro, and then go to `Admin` > `Manage Users`:

![Admin - Manage Users](https://raw.githubusercontent.com/wiki/overleaf/overleaf/admin_manage_users.png)

Then find the user by their email address and click through to their user admin page. There you will find the ID:

![User ID shown in admin panel](https://raw.githubusercontent.com/wiki/overleaf/overleaf/user_id_in_admin_panel.png)

### Publishing Templates

For each template you want to upload:

1. Log in as the templates user.
2. As the templates user, create a project containing the template's source code and make sure it compiles.
3. In the editor's left hand menu, choose Publish as Template:

   ![Publish as Template in the editor menu](https://raw.githubusercontent.com/wiki/overleaf/overleaf/publish_as_template.png)

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

![ShareLaTeX Template Links](https://raw.githubusercontent.com/wiki/overleaf/overleaf/images/new_project_template_links.png)
