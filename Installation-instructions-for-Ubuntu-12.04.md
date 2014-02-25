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

    sudo apt-get install -y redis-server

## Installing Mongodb

## Installing TexLive 2013

