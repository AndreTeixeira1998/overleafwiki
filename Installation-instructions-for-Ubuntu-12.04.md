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

## Installing TeX Live 2013

If you want to save time:

- download the TeX Live ISO via torrent:

    wget -O /tmp/texlive2013.torrent https://www.tug.org/texlive/files/texlive2013.iso.torrent

- put it in the current directory with exact name `texlive`

And then:

    if [ ! -f texlive2013.iso ]; then
      wget texlive2013.iso http://mirrors.linsrv.net/tex-archive/systems/texlive/Images/texlive2013.iso
    fi
    sudo mkdir -p /media/texlive2013
    sudo mount -t iso9660 -o ro,loop,noauto texlive2013.iso /media/texlive2013
    echo i | sudo /media/texlive2013/install-tl
    sudo umount /media/texlive2013
    sudo rmdir /media/texlive2013
    echo '
    # Texlive
    export PATH=$PATH:/usr/local/texlive/2013/bin/i386-linux
    export MANPATH=$MANPATH:/usr/local/texlive/2013/texmf-dist/doc/man
    export INFOPATH=$INFOPATH:/usr/local/texlive/2013/texmf-dist/doc/info
    ' >> ~/.profile

### 2009 workaround

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

# Vagrant install 

An experimental vagrant installer is developed at  

* https://github.com/laszewsk/vagrant-sharelatex

It includes the steps listed above and could serve as a template. It starts up the server, but I think it is still incomplete in some aspects for setting up REDIS and or MONGODB as well as the configuration files