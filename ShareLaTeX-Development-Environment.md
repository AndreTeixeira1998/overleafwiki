When you checkout and install ShareLaTeX using the instructions in the README of sharelatex/sharelatex, you've successfully set up a development environment to use. There are a few things that you need to know though, so read on!

Config
------

When you run `grunt install`, it should copy the example config into `config/settings.development.coffee`. This file is ignored by git, so you can customise it for your local set up without causing conflicts. Make sure not to check it in though!

Recompiling coffeescript -> js
------------------------------

When you run `grunt install`, we automatically compile all the coffeescript in each repository to javascript. However, when developing, you will no doubt make modifications to the coffeescript files. To recompile the js, you need to `cd` into the directory of the service you are working on, and then run `grunt compile`. For example:

    $ cd web/ # or document-updater/, filestore/ or clsi/
    $ grunt compile

You'll need to do this each time you make any changes. In the case of changes to the serverside files, you'll also need to restart the server.