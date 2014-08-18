
First, check out a local copy of this repository:

```bash
$ git clone https://github.com/sharelatex/sharelatex.git
$ cd sharelatex
```

Next install all the node modules and ShareLaTeX services:

```bash
$ npm install
$ grunt install
```

This will create a config file in `config/settings.development.coffee`. You should open
this now and configure your AWS S3 credentials, and other custom settings.

Now check that your system is set up correctly to run ShareLaTeX (checks that you have
the required dependencies installed.) Watch out for any failures.

```bash
$ grunt check --force
```

When that has finished, run ShareLaTeX with

```bash
$ grunt run
```

ShareLaTeX should now be running at http://localhost:3000.

Config
------

When you run `grunt install`, it should copy the example config into `config/settings.development.coffee`. This file is ignored by git, so you can customise it for your local set up without causing conflicts. Make sure not to check it in though!

Recompiling coffeescript -> js
------------------------------

When you run `grunt install`, we automatically compile all the coffeescript in each repository to javascript. However, when developing, you will no doubt make modifications to the coffeescript files. To recompile the js, you need to `cd` into the directory of the service you are working on, and then run `grunt compile`. For example:

    $ cd web/ # or document-updater/, filestore/ or clsi/
    $ grunt compile

You'll need to do this each time you make any changes. In the case of changes to the serverside files, you'll also need to restart the server.

Unit Tests
----------

Every ShareLaTeX service has a suite of unit tests which we are militant about keeping passing at all times. To run the tests, change into the corresponding service directory and run `grunt test:unit`. Eg.

    $ cd web/
    $ grunt test:unit

The Gruntfiles can be inconsistent sometimes, so check `grunt help` for the possible test tasks. You can specify individual tests to run with:

    $ grunt test:unit --grep="<expression>"

where `<expression>` matches the test description. This will speed up things in repositories like web-sharelatex, with a large test suite.

New features and code should come with corresponding unit tests. See the existing tests in the `test/unit/coffee` directories of any service for an understanding of how to lay out new tests. We use a module called `sandboxed-module` to inject mock dependencies into each file when unit testing.

What to work on
---------------

If you want to help out with ShareLaTeX but aren't sure what to work on, then have a look at our [[Development Roadmap]], or the open issues in each repository under the [ShareLaTeX organisation](https://github.com/sharelatex)