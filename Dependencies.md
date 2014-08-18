ShareLaTeX is supported on Linux and OS X. You will need the following dependencies installed:

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
$ sudo apt-get install git build-essential curl python-software-properties zlib1g-dev zip unzip
```

### Node.js

For development purposes, you can use [nvm](https://github.com/creationix/nvm) to easily manage and install Node.js.

For production, you will need a system wide Node.js installation. The Node.js version provided by Ubuntu 12.04 is too old for ShareLaTeX so you will need to install a newer version. You can do this directly from the [Node.js website](http://nodejs.org/), or via a custom PPA in Ubuntu:

```sh
$ sudo apt-add-repository ppa:chris-lea/node.js
$ sudo apt-get update
$ sudo apt-get install nodejs
```

Once Node.js is installed, you'll also need to install the grunt command line tools (use sudo if Node.js is installed system wide):

```sh
$ sudo npm install -g grunt-cli
```

### Redis

The Redis version provided by Ubuntu 12.04 is also too old for ShareLaTeX so you will need to install a newer version. The latest version of Redis can be downloaded and compiled from source via the [Redis website](http://redis.io/), or with a custom PPA on Ubuntu:

```sh
$ sudo apt-add-repository ppa:chris-lea/redis-server
$ sudo apt-get update
$ sudo apt-get install redis-server
```

### MongoDB

The latest version of MongoDB can be installed from the MongoDB repository (http://docs.mongodb.org/manual/tutorial/install-mongodb-on-ubuntu/):

```sh
$ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10
$ echo 'deb http://downloads-distro.mongodb.org/repo/ubuntu-upstart dist 10gen' | sudo tee /etc/apt/sources.list.d/mongodb.list
$ sudo apt-get update
$ sudo apt-get install mongodb-org
```

### Aspell

Aspell is a nice stable bit of software so can be got directly from Ubuntu's repositories:

```sh
$ sudo apt-get install aspell
```

There are lots of additional dictionaries available, which can be listed with:

```sh
$ apt-cache search aspell | grep aspell
```

## Installing Dependencies on Mac OS X

Redis, MongoDB and Aspell are easiest to install with [Homebrew](http://brew.sh/). Once Homebrew is installed:

```sh
$ brew install redis mongodb aspell
```

Node.js can be installed with [nvm](https://github.com/creationix/nvm) or direct from the [Node.js website](http://nodejs.org/).

## Installing TeXLive

ShareLaTeX needs a LaTeX backend to run the compiles, and you must have the `latexmk` program installed as part of this. We recommend installing the latest version of TeXLive manually (more detailed instructions at https://www.tug.org/texlive/quickinstall.html):

```sh
$ wget http://mirror.ctan.org/systems/texlive/tlnet/install-tl-unx.tar.gz
$ tar -xvf install-tl-unx.tar.gz
$ cd install-tl-*
$ sudo ./install-tl
```
Once TeXLive is installed, make sure that it is available in your path:

```sh
$ export PATH=/usr/local/texlive/2014/bin/i386-linux:$PATH # Linux
$ export PATH=/usr/local/texlive/2014/bin/x86_64-linux:$PATH # Linux 64-bit
$ export PATH=/usr/local/texlive/2014/bin/x86_64-darwin:$PATH # Mac OS X
```

and that `latexmk` is installed:

```sh
$ sudo tlmgr install latexmk
```