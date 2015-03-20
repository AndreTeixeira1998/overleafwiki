These instructions are for someone who wants to set up ShareLaTeX in a development environment. If you just want to run ShareLaTeX as a server, see the [[Production Installation Instructions]]

## Dependencies

Make sure you have all of the [[Dependencies]] installed and configured.

## Downloading and installing ShareLaTeX

First, check out a local copy of ShareLaTeX and install the required modules:

```bash
$ git clone https://github.com/sharelatex/sharelatex.git
$ cd sharelatex
$ npm install
```

ShareLaTeX is made up of lots of separate services, and the main sharelatex/sharelatex repository is just a collection of scripts and tools for pulling these together. Run `grunt install` to download the install the actual ShareLaTeX services:

```bash
$ grunt install
```

## Config file

`grunt install` will also create a config file in `config/settings.development.coffee`. This will work out of the box if you have Redis and Mongo running on the standard ports, but you should take a look to familiarise yourself with the options, and set any custom settings for you system.

## System/Dependencies Check

You can check that your system is set up correctly to run ShareLaTeX with the following command:

```bash
$ grunt check --force
```

This doesn't test everything, but will highlight common problems.

## Running ShareLaTeX

ShareLaTeX is built from many different services, and you need all of these running for it to work properly. The main `sharelatex` repository comes with some useful Grunt commands to help with running the services.

To run all of the ShareLaTeX services with one command, use:

```bash
$ grunt run:all
```

ShareLaTeX should now be running at http://localhost:3000.

Individual services can be run with
```bash
$ grunt run:web
$ grunt run:docstore
$ # etc...
```

### Logging in for the first time

Before you can log in, you will need to create your first admin user to manage the site. See [[Creating and managing users]] for details.

Recompiling coffeescript -> js
------------------------------

When you run `grunt install`, we automatically compile all the coffeescript in each repository to javascript. However, in development you will make modifications to the coffeescript files. To recompile the js, you need to `cd` into the directory of the service you are working on, and then run `grunt compile`. For example:

    $ cd web/ # or document-updater/, filestore/ or clsi/
    $ grunt compile

You'll need to do this each time you make any changes. In the case of changes to the server-side files, you'll also need to restart the server.

Unit Tests
----------

Every ShareLaTeX service has a suite of unit tests which we try to keeping passing at all times. To run the tests, change into the corresponding service directory and run `grunt test:unit`. Eg.

    $ cd web/
    $ grunt test:unit

The Gruntfiles can be inconsistent sometimes, so check `grunt help` for the possible test tasks. You can specify individual tests to run with:

    $ grunt test:unit --grep="<expression>"

where `<expression>` matches the test description. This will speed up things in repositories like web-sharelatex, with a large test suite.

New features and code should come with corresponding unit tests. See the existing tests in the `test/unit/coffee` directories of any service for an understanding of how to lay out new tests. We use a module called `sandboxed-module` to inject mock dependencies into each file when unit testing.

What to work on
---------------

If you want to help out with ShareLaTeX but aren't sure what to work on, then have a look at our open issues in each repository under the [ShareLaTeX organisation](https://github.com/sharelatex), or jump in our [Developer Chat Room](http://www.hipchat.com/g1nJMcj7b)