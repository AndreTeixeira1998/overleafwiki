When you checkout and install ShareLaTeX using the instructions in the README of sharelatex/sharelatex, you've successfully set up a development environment to use. There are a few things that you need to know though, so read on!

Config
------

Rather than modify `config/settings.development.coffee` (which is in git but shouldn't be changed!), you can set the SHARELATEX_CONFIG environment variable to point to your own copy, which you can modify freely:

    $ export SHARELATEX_CONFIG=~/config/sharelatex.local.coffee # or wherever your file is

Put this line in your `.profile` or `.bashrc` file for ease of use.

Recompiling coffeescript -> js
------------------------------

When you run `grunt install`, we automatically compile all the coffeescript in each repository to javascript. However, when developing, you will no doubt make modifications to the coffeescript files. To recompile the js, you need to `cd` into the directory of the service you are working on, and then run `grunt compile`. For example:

    $ cd web/ # or document-updater/, filestore/ or clsi/
    $ grunt compile

You'll need to do this each time you make any changes. In the case of changes to the serverside files, you'll also need to restart the server.