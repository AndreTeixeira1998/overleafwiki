ShareLaTeX is supported on OS X and Linux. You will need the following dependencies installed:

* [Node.js](http://nodejs.org/) 0.10 or greater. [nvm](https://github.com/creationix/nvm) provides an easy way to install and manage Node.js in a development environment.
* [Grunt](http://gruntjs.com/) command line tools (Run `npm install -g grunt-cli` to install them)
* [Redis](http://redis.io/topics/quickstart) (version 2.6.12 or later)
* [MongoDB](http://docs.mongodb.org/manual/installation/) (version 2.4 or later)
* [TeXLive](https://www.tug.org/texlive/) 2013 or later with the `latexmk` program installed.
* [Aspell](http://aspell.net/) for spell checking, with appropriate dictionaries installed.

## Installing dependencies on Ubuntu

These instructions are appropriate for Ubuntu 12.04, but should also work on more recent versions.

### Basic build system

Make sure that you've got the basic development tools installed like `git` and `make`:

```sh
$ sudo apt-get update
$ sudo apt-get install git build-essential curl python-software-properties
```

### Node.js

For development purposes, you can use [nvm](https://github.com/creationix/nvm) to easily manage and install Node.js.

For production, you will need a system wide Node.js installation. The Node.js version provided by Ubuntu 12.04 is too old for ShareLaTeX so you will need to install a newer custom version:

```sh
$ sudo apt-add-repository ppa:chris-lea/node.js
$ sudo apt-get update
$ sudo apt-get install nodejs
```

Once Node.js is installed, you'll also need to install the grunt command line tools (use sudo if Node.js is installed system wide):

```sh
$ npm install -g grunt-cli
```

### Redis

The Redis version provided by Ubuntu 12.04 is also too old for ShareLaTeX so you will need to install a newer custom version:

```sh
$ sudo apt-add-repository ppa:chris-lea/redis-server
$ sudo apt-get update
$ sudo apt-get install redis-server
```