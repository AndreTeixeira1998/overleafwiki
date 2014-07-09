_Note this page is a work in progress, and the dpkg is not quite ready for use_

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

Go through the interactive installer. We recommend using the `scheme-full` installation since this will give you a comprehensive LaTeX environment, but at over 3Gb it may be too large for some users. Whichever scheme you choose, make sure to also select the `TeX auxiliary programs` package as it contains the `latexmk` program which is needed by ShareLaTeX. 

Now make sure that LaTeX and latexmk are available in your path system-wide. Edit `/etc/environment` to include the TeXLive binary directory:

```sh
# cat /etc/environment 
PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/texlive/2013/bin/x86_64-linux/"
```

__There is still an issue with the compiler finding latexmk in the path__. Possibly the upstart file needs modified, but this is tricky because LaTeX may be installed somewhere else. A setting in the CLSI config perhaps?

## Installing ShareLaTeX

```sh
dpkg -i sharelatex_0.0.1_amd64.deb
```