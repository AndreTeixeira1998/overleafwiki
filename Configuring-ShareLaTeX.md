* `SHARELATEX_SITE_URL`: Where your instance of ShareLaTeX is publically available.
This is used in public links, and when connecting over websockets, so must be
configured correctly!
* `SHARELATEX_ADMIN_EMAIL`: The email address where users can reach the person who runs the site.
* `SHARELATEX_APP_NAME`: The name to display when talking about the running app. Defaults to 'ShareLaTex (Community Edition)'.
* `SHARELATEX_MONGO_URL`: The URL of the Mongo database to use
* `SHARELATEX_REDIS_HOST`: The host name of the Redis instance to use
* `SHARELATEX_REDIS_PORT`: The port of the Redis instance to use
* `SHARELATEX_REDIS_PASS`: The password to use when connecting to Redis (if applicable)




* `SHARELATEX_NAV_TITLE`: Set the tab title of the application
* `SHARELATEX_SESSION_SECRET`: A random string which is used to secure tokens, if load balancing this needs to be set to the same toke across boxes. If only 1 instance is being run it does not need to be set by the user.
* `SHARELATEX_BEHIND_PROXY`: Set to true if running behind a proxy like nginx/apache allowing it to correctly detect the forwarded IP address
* `SHARELATEX_SECURE_COOKIE`: Set this to something non-zero to use a secure cookie.
  Only use this if your ShareLaTeX instance is running behind a reverse proxy with SSL configured.
* `SHARELATEX_RESTRICT_INVITES_TO_EXISTING_ACCOUNTS`: If set to `true`, will restrict project invites to email addresses which correspond with existing user accounts.


* `SHARELATEX_ALLOW_PUBLIC_ACCESS`: If set to `'true'`, will allow non-authenticated users to view the site. The default is `false`, which means non-authenticated users will be unconditionally redirected to the login page when they try to view any part of the site. Note, setting this option does not disable authentication or security in any way. This option is necessary if your users intend to make their projects public and have non-authenticated users view those projects.

* `SHARELATEX_ALLOW_ANONYMOUS_READ_AND_WRITE_SHARING`: If set to `'true'`, will allow anonymous users to view and edit projects shared via the new [link-sharing](https://www.sharelatex.com/blog/2017/11/27/integration-update-link-sharing.html) feature.