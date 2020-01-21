# Server Pro 2.1.1

### Bugfixes
- Fixed unable to share projects via email address.

# Server Pro 2.1.0

### New Features
- Email confirmation banner can be disabled with the new environment variable `EMAIL_CONFIRMATION_DISABLED: 'true'`
- [New Archival/trashing workflow](https://www.overleaf.com/blog/new-feature-using-archive-and-trash-to-keep-your-projects-organized)
- Project ownership can be changed from the admin panel.

### Other Changes

- Admin: improved search using regular expressions 
- Other minor improvements and bugfixes.

# Server Pro 2.0.3

2.0.3 is a Server Pro-only release, no Community Edition has been published.

### Bugfixes
- Fixed unable to share projects via email address.

### Other Changes
- Reenabled recaptcha, which was the root cause of the issue above.

# Server Pro 2.0.2

### Bugfixes
- Fixed link sharing for anonymous users when `SHARELATEX_ALLOW_PUBLIC_ACCESS: 'true'` and `SHARELATEX_ALLOW_ANONYMOUS_READ_AND_WRITE_SHARING: 'true'`.
- Fixed read-only access to shared projects.
- Fixed SAML logout (Server Pro only).
- Fixed sync between editor and pdf with `synctex` (Server Pro only).
- Fixed track changes slow to commit.

### Other Changes
- Disabled recaptcha (Server Pro only).
- Importing external files is disabled by default.
- **IMPORTANT**: A new `SYNCTEX_BIN_HOST_PATH` is required when using Sibling Containers. Check the setup instructions in the wiki: https://github.com/overleaf/overleaf/wiki/Server-Pro:-sandboxed-compiles#mapping-the-location-of-synctex-in-the-host.

# Server Pro 2.0.1

### Bugfixes
- Fixed ["Delete Forever" button has no effect](https://github.com/overleaf/overleaf/issues/644).
- Fixed [admin creation using CLI](https://github.com/overleaf/overleaf/issues/647).

### Known issues
 - SyncTeX not working on Sandboxed Compiles [(see workaround instructions)](https://github.com/overleaf/overleaf/wiki/Fixing-SyncTeX-errors-in-Server-Pro-2.0.0-and-2.0.1).

# Server Pro 2.0.0

A lot has changed in the last few years, with ShareLaTeX [joining forces with Overleaf](https://www.overleaf.com/blog/518-exciting-news-sharelatex-is-joining-overleaf) to create one incredible authoring platform. Now that the merge is complete it's time to release a new version of ~ShareLaTeX~ Overleaf Community Edition and Server Pro!


## New Features

This release brings Community Edition and Server Pro up to date with the cloud based version of [Overleaf](https://overleaf.com). A lot has changed since the last release, with too many bug fixes and small improvements to count, but here's a few highlights:

- A brand new user interface theme
- Improved project dashboard
- A new interface for creating (or uploading) files in a project
- Linked Files: import a file from another project.
- Improved History view


### Server Pro

This release of Server Pro includes a few new premium features:

- Rich Text mode: write your document like a rich-text document, backed by nice clean LaTeX
- Improved Sandboxed Compiles: no more fiddling with group permissions (see below)
- Improved Admin Interface: now you can inspect the state of a project from the Admin page, plus many more small improvements


## Changes to the Docker-Compose file format

This release adds a few new options to the docker environment. We recommend using our updated example [docker-compose.yml](https://github.com/overleaf/overleaf/blob/master/docker-compose.yml) file, and updating it to reflect your old configuration where necessary. 

The new variables are:

- `ENABLED_LINKED_FILE_TYPES`: a comma-separated list keys, which controls which types of "linked file" are available in the New File modal. Defaults to `'url,project_file'`, as in the example file.
- `ENABLE_CONVERSIONS`: If set to `'true'`, will enable on-the-fly file preview conversions using ImageMagick. Set to another value to disable this feature
- `REDIS_HOST`: should be set to the name of the redis host, same as the `SHARELATEX_REDIS_HOST` variable. In the case of a simple docker-compose based deployment, this will just be `'redis'`. This duplication is unfortunately necessary in the short term while users migrate to the new Community Edition and Server Pro images, and will be resolved in a later release.
- `REDIS_PORT`: In case we desire to use port for redis other than the default, we need to set the value in `REDIS_PORT` to be the same as in `SHARELATEX_REDIS_HOST`, for the same reason described above.

And for Server Pro:

- `DOCKER_RUNNER`: set to `'true'` when enabling Sandboxed Compiles with sibling containers. See the updated documentation on [Sandboxed Compiles](https://github.com/overleaf/overleaf/wiki/Server-Pro:-sandboxed-compiles).
- `SYNCTEX_BIN_HOST_PATH` (introduced in `2.0.2`): set to `<your_sharelatex_data_directory>/bin/synctex`. Check [Sibling Containers documentation](https://github.com/overleaf/overleaf/wiki/Server-Pro:-sandboxed-compiles#mapping-the-location-of-synctex-in-the-host) for extra context


## Changes to Sandboxed Compiles

We've made some changes to the way the [Sanboxed Compiles](https://github.com/overleaf/overleaf/wiki/Server-Pro:-sandboxed-compiles) work. In previous versions the administrators often needed to fiddle with user and group permissions on the docker socket to get Sibling Containers to work. In this release we've changed all that so it's handled automatically inside the Server Pro container, so it should just work in the majority of cases.

From this release onward we will no longer support the old "Docker-In-Docker" method of sandboxed compiles, as it has become more and more difficult to get this to work as time goes on. We strongly encourage admins to consider the newer Sibling Containers method as an alternative.


### Upgrading TexLive

You can now opt in to a new version of TexLive. The default is still TexLive 2017, but you can find instructions on how to get TexLive 2018 here: https://github.com/overleaf/overleaf/wiki/Server-Pro:-sandboxed-compiles#changing-the-texlive-image


### Database Migrations

The following database migrations have been added, and will run automatically:

- Alter the structure of the User model to support future work
- Change structure of `project.tokens`, to enforce uniqueness on the numeric part
  of a read-and-write sharing token

Administrators and end users should not notice any change in behaviour due to these
migrations.


## Support

If anything seems out of place, or you run into any problems while upgrading, please
get in touch with us via email at `support@overleaf.com`
