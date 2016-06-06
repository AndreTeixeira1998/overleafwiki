## Data Security
No data is sent to 3rd parties, including ShareLaTeX.com.

## Sending messages to all users
The admin panel allows for a banner to set for all users

## Deleting users
ShareLaTeX Server Pro's admin panel has the option to delete users, it can also be done via mongo command line

`db.users.remove({email:"delete_this_user@gmail.com"})`

## Maximum size of projects in ShareLaTeX
The maximum amount of editable data in ShareLaTeX is 5mb, this does not include images or other large non editable files.