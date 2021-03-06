### Setting up the Templates User

A single Overleaf user is responsible for publishing the curated list of templates that are visible on `/templates`. To set this, use the ENV var `SHARELATEX_TEMPLATES_USER_ID`, for instance:

`--env SHARELATEX_TEMPLATES_USER_ID=56a8865231faeb5f07d69959`

To obtain the user id of the user you wish to publish public templates, log in as the admin user you created when you set up Server Pro, and then go to `Admin` > `Manage Users`:

![Admin - Manage Users](https://raw.githubusercontent.com/wiki/overleaf/overleaf/admin_manage_users.png)

Then find the user by their email address and click through to their user admin page. There you will find the ID:

![User ID is shown in admin panel](https://raw.githubusercontent.com/wiki/overleaf/overleaf/user_id_in_admin_panel.png)

### Publishing Templates

For each template you want to upload:

1. Log in as the templates user.
2. As the templates user, create a project containing the template's source code and make sure it compiles.
3. In the editor's left-hand menu, choose **Publish as Template**:

   ![Publish as Template in the editor menu](https://raw.githubusercontent.com/wiki/overleaf/overleaf/publish_as_template.png)


### Unpublishing Templates

In order to unpublish a template you need to republish the template as described in the section above, then click the **Unpublish** button once the confirmation popup is displayed:

1. Log in as the templates user.
2. Open the previously existing project containing the template's source code.
3. In the editor's left-hand menu, choose **Publish as Template**:.
4. Once the confirmation popup is displayed, click **Unpublish**.

The template should have been removed from the templates list.

### Configuring Links to Templates

#### Templates on index page
On the templates index page, `/templates`, templates are nested under folders which the user puts the projects in, e.g. Journals, Reports etc. To see all templates add `/all` to the URL `/templates/all`, which can also be used as the default URL if you do not wish to use the folder for grouping.


#### Template links on project list page

When a user creates a new project they can be shown as linked to templates. These are set via the  `SHARELATEX_NEW_PROJECT_TEMPLATE_LINKS` variable:

`SHARELATEX_NEW_PROJECT_TEMPLATE_LINKS='[
   {"name":"All Templates","url":"/templates/all"},
   {"name":"All Categories","url":"/templates"},
   {"name":"reports","url":"/templates/reports"},  {"name":"External","url":"https://somewhere.com/templates/reports"}
]'`

![ShareLaTeX Template Links](https://raw.githubusercontent.com/wiki/overleaf/overleaf/images/new_project_template_links.png)
