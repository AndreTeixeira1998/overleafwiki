If you've successfully installed ShareLaTeX on Ubuntu 12.04, please edit this page to document your success!

## Installing Node

Do not use the packaged version as it is too old. Use `nvm` instead:

    cd sharelatex
    curl https://raw.github.com/creationix/nvm/master/install.sh | sh
    source ~/.nvm/nvm.sh
    nvm install "$(cat .nvmrc)"

Before you run ShareLaTeX, you must run once per shell:

    source ~/.nvm/nvm.sh
    nvm use "$(cat .nvmrc)"

If you develop ShareLaTeX often, add it to your `.bashrc`:

    echo "
    source ~/.nvm/nvm.sh
    nvm use $(cat .nvmrc)
    " >> ~/.bashrc

## Installing Grunt

Once you've installed Node via `nvm`, install Grunt via:

    npm install -g grunt-cli

## Installing Redis

The packaged version in 12.04 is too old. There are instructions [here](http://redis.io/topics/quickstart) on how to install the latest stable version. `build-essential` will provide all the packages you need to compile it

    sudo apt-get install -y build-essential
    wget http://download.redis.io/redis-stable.tar.gz
    tar zxf redis-stable.tar.gz
    cd redis-stable
    make

will compile `redis-server` and `redis-cli` - either use them directly, or follow the instructions in the above link to set up the config + init.d scripts

## Installing Mongodb

    sudo apt-get install -y mongodb

## Installing TexLive 2013

TeX Live installation 2013 is painful in Ubuntu 12.04 as there is no simple package install for it.

If you are just developing, a possible workaround is to simply grab the default provided TeX Live 2009 via:

    sudo apt-get install texlive-latex-base

since it should be mostly compatible, and then install `latexmk` with:

```bash
mkdir -p ~/bin 
curl http://mirror.physik-pool.tu-berlin.de/tex-archive/support/latexmk/latexmk.pl > ~/bin/latexmk
chmod a+x ~/bin/latexmk
export PATH=~/bin:$PATH
```
If you want to be a good boy / girl and install the correct version do:

    wget http://mirror.ctan.org/systems/texlive/tlnet/install-tl-unx.tar.gz
    tar xjf install-tl-unx.tar.gz
    cd install-tl-*
    echo i | sudo ./install-tl
    echo '
    # Texlive
    export PATH=$PATH:/usr/local/texlive/2013/bin/i386-linux
    export MANPATH=$MANPATH:/usr/local/texlive/2013/texmf-dist/doc/man
    export INFOPATH=$INFOPATH:/usr/local/texlive/2013/texmf-dist/doc/info
    ' >> ~/.profile

This will take a while.

# Vagrant install 

An experimental vagrant installer is developed at  

* https://github.com/laszewsk/vagrant-sharelatex

It includes the steps listed above and could serve as a template. It starts up the server, but I think it is still incomplete in some aspects for setting up REDIS and or MONGODB as well as the configuration files