### What is systemd?
systemd has replaced SysV init and upstart in many Linux distributions, such as Fedora, Red Hat Enterprise Linux/CentOS and Arch.  It is the default system way of managing services, which includes a built in logging system that can work independent of, or together with, syslogger.

### Installation
The steps presented here represent a fully working example of how to setup ShareLaTeX to be managed by systemd. The comments will guide you to places where you can modify things to work with your particular environment. For more advanced features, please see the systemd documentation or your system's documentation as appropriate.  These instructions should work on Red Hat/CentOS 7 systems or any other system that uses the standard systemd specified paths.

It is also assumed that you created individual user and group for the ShareLaTeX installation appropriately named **sharelatex**. You will also likely need to be root or have **sudo** privileges for all these to work.

#### Directory structure
Create the following directories with the appropriate ShareLaTeX permissions (you will need to be root for this)
```
mkdir -p /etc/sharelatex
mkdir -p /var/log/sharelatex
chown sharelatex.sharelatex /var/log/sharelatex
touch /etc/sharelatex/sharelatex.env
touch /etc/sharelatex/start_module.sh
chmod +x /etc/sharelatex/start_module.sh
touch /etc/systemd/system/sharelatex@.service
```

Now create and edit the following files.
#### The Environment File
The environment file will setup the environment for running each ShareLaTeX system. It is easier to maintain this file (and is separate from the ShareLaTeX configuration file) than it is to modify systemd files at each change of the environment.

Using your favorite editor, edit the following file:
```
/etc/sharelatex/sharelatex.env
````
and paste into the file the following:
```
# Specify type of instances (i.e. production)
NODE_ENV=production

# Specify node executable (with absolute path)
NODE=/bin/node

# Specify the full path of the config file
SHARELATEX_CONFIG=/etc/sharelatex/settings.coffee

# Specify the full path of the LaTeX installation
LATEX_PATH=/usr/local/bin

# Specify full path to log directory
SHARELATEX_LOGDIR=/var/log/sharelatex
```

#### External Script to Start Individual ShareLaTeX services
On many systems, such as CentOS, systemd forwards logs to the syslogger, which may clog up your logs with the many optional messages ShareLaTeX spit out. It's best to have an external script that starts the service, and forwards output to the appropriate log file. Edit the following file:
```
/etc/sharelatex/start_module.sh
```
and paste the following inside that file:
```
#!/bin/sh -
${NODE} app.js &> ${SHARELATEX_LOGDIR}/${1}.log
```
This file should have been made executable if you followed the instructions in the directory structure. If not, go ahead an give it the execute flag
```
chmod +x /etc/sharelatex/start_module.sh
```

#### Create a systemd template
Now we create a template for systemd that we can then call on later to start, stop, and reload the individual ShareLaTeX services.  Edit the following file (notice and keep the **@** symbol, as it defines the template:
```
/etc/systemd/system/sharelatex@.service
```
and now paste the following onto that file:
```
[Unit]
Description=ShareLaTeX A web-based collaborative LaTeX editor
Documentation=https://github.com/sharelatex/sharelatex/wiki
After=syslog.target
After=network.target

[Service]
Type=simple

# Specify user/group to execute as (sharelatex is the default)
User=sharelatex
Group=sharelatex

# Specify the user configurable environment file
EnvironmentFile=/etc/sharelatex/sharelatex.env

# Specify path of installed instance of share latex (leave trailing /%i)
WorkingDirectory=/var/www/sharelatex/%i

# Start ShareLaTeX service: i.e. web, chat etc.. and stores output
# files in different log files. Comment out this line and uncomment
# the next if you prefer to use the systemd logger
ExecStart=/etc/sharelatex/start_module.sh %i
#ExecStart=/bin/node app.js

[Install]
WantedBy=multi-user.target
```
Anytime you edit this file, you might want to ask systemd to reload the latest changes by typing
```
systemctl daemon-reload
```

#### Manage system services
Now to manage services using systemd, you will use the syntax sharelatex@modulename.service, for example, to start the web module you would type
```
  systemctl start sharelatex@web.service
```
You can also instruct systemd to start the service at boot, like so
```
  systemctl enable sharelatex@web.service
```
Remember that you have to do this for each and every service you intend to start. As of release 1.3 the following services exist
* chat
* clsi
* docstore
* document-updater
* filestore
* spelling
* tags
* template
* track-changes
* web
* real-time
