## Installing Node.js

```sh
sudo apt-get install python-software-properties
sudo add-apt-repository ppa:chris-lea/node.js
sudo apt-get update
sudo apt-get install nodejs
```

## Installing Mongo

```sh
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10
echo 'deb http://downloads-distro.mongodb.org/repo/ubuntu-upstart dist 10gen' | sudo tee /etc/apt/sources.list.d/mongodb.list
sudo apt-get update
sudo apt-get install mongodb-org
```

## Installing Redis

```sh
sudo add-apt-repository ppa:chris-lea/redis-server
sudo apt-get update
sudo apt-get install redis-server
```

## Installing TexLive 2013

```sh
wget http://mirror.ctan.org/systems/texlive/tlnet/install-tl-unx.tar.gz
tar -xvf install-tl-unx.tar.gz
cd install-tl-*
sudo ./install-tl
```

Go through the interactive installer. Make sure to install the `binextra` package as it contains the `latexmk` program which is needed by ShareLaTeX. We recommend using the `scheme-full` installation since this will give you a comprehensive LaTeX environment, but at over 3Gb it may be too large for some users.
