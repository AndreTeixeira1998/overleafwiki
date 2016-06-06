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