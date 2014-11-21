This **tiny** howto will drive you to install and run *sharelatex* on Arch Linux. Presently, we have no success in use **systemd** services to run *sharelatex* daemons at startup, thus they are executed manually. The help of Arch Linux experts is welcome.

It is worth noting that this guide concerns with a *quick and dirty* installation: for a more correct instructions we are waiting Arch Linux experts.
### Dependencies

Make sure you have all the of the required [[Dependencies]] installed. Pay particular attention to the installation and setting of **mongodb** and **redis-server**.

Let us assume that sharelatex have been correctly installed following [[Production Installation Instructions]]

#### Installing and Setting MongoDB

MongoDB is present into the community repository. To install it type:
```bash
sudo pacman -S mongodb
```

After the installation few settings are necessary. It is worth noting that the installation creates the user **mongodb**. In our quick and dirty installation we must add the **sharelatex** group to the **mongodb** user: 

```bash
sudo usermod -a -G sharelatex mongodb
```

The installation of MongoDB should have created a config file  and service file. Let us now customize these files. With your preferred text editor (in the following the great *vim*) open `/etc/mongodb.conf`:
```bash
sudo vim /etc/mongodb.conf
```

Find the `dpath = ...` line and replace with `dbpath = /var/lib/sharelatex`. The config file should be similar to:
```
# See http://www.mongodb.org/display/DOCS/File+Based+Configuration for format details
# Run mongod --help to see a list of options

bind_ip = 127.0.0.1
quiet = true
dbpath = /var/lib/sharelatex
logpath = /var/log/mongodb/mongod.log
logappend = true
```

Now take a look at the MongoDB service file. It is placed in `/usr/lib/systemd/system/mongodb.service`. It should appear similar to:
```
[Unit]
Description=High-performance, schema-free document-oriented database
After=network.target

[Service]
User=mongodb
ExecStart=/usr/bin/mongod --quiet --config /etc/mongodb.conf

[Install]
WantedBy=multi-user.target
```

Now enable the service and start it:
```bash
sudo systemctl enable mongodb
sudo systemctl start mongodb
```

To check that all go smooth type: 
```bash
systemctl status mongodb.service
```
and if you are lucky, you should read:
```bash
● mongodb.service - High-performance, schema-free document-oriented database
   Loaded: loaded (/usr/lib/systemd/system/mongodb.service; enabled)
   Active: active (running) since Thu 2014-11-06 10:03:48 CET; 3h 39min ago
 Main PID: 46746 (mongod)
   CGroup: /system.slice/mongodb.service
           └─46746 /usr/bin/mongod --quiet --config /etc/mongodb.conf
```

#### Installing and Setting Redis-server

Redis is present into the community repository. To install it type:
```bash
sudo pacman -S redis
```

There is no need to edit the config and service files that are created after the installation of Redis. Just enable the service and start it:
```bash
sudo systemctl enable redis
sudo systemctl start redis
```

To check that all go smooth type: 
```bash
systemctl status redis.service
```
and if you are lucky, you should read:
```bash
● redis.service - Advanced key-value store
   Loaded: loaded (/usr/lib/systemd/system/redis.service; enabled)
   Active: active (running) since Thu 2014-11-06 10:07:58 CET; 3h 40min ago
 Main PID: 46901 (redis-server)
   CGroup: /system.slice/redis.service
           └─46901 /usr/bin/redis-server 127.0.0.1:6379
```

### Setting Up and Run the Sharelatex daemons

These are the bad news: who is writing this tiny howto is not and expert administrator of Arch Linux, thus he was not able to write and start a correct systemd unit file for starting the sharelatex daemons! In the following a quick and dirty way to run the daemons are presented.

Somewhere you like (I have chosen to create a custom directory into /var/www/sharelatex/) place the following script (named as `sharelatex_service_start.sh`):

```bash
#!/bin/bash
SERVICE=$1
echo $$ > /var/run/sharelatex/$SERVICE.pid
cd /var/www/sharelatex/$SERVICE
exec env SHARELATEX_CONFIG=/etc/sharelatex/settings.coffee NODE_ENV=production node app.js >> /var/log/sharelatex/$SERVICE.log 2>&1 &
```
Give it run permissions:
```
chmod +x sharelatex_service_start.sh
```
Log as sharelatex user:
```bash
sudo -su sharelatex
```
and use the script for executing sharelatex daemons:
```bash
sharelatex_service_start.sh chat
sharelatex_service_start.sh clsi            
sharelatex_service_start.sh docstore        
sharelatex_service_start.sh document-updater
sharelatex_service_start.sh filestore       
sharelatex_service_start.sh spelling        
sharelatex_service_start.sh tags            
sharelatex_service_start.sh track-changes   
sharelatex_service_start.sh web             
```
Obviously, it is preferable to automate the daemons execution by means of systemd, but my tentatives have been unsuccessful. 

### Setting Up and Run the Sharelatex with Systemd 
Here is a template for unit file service :
```bash
[Unit]
Description=ShareLatex-web-NAME
After=network.target
After=redis.service
After=mongodb.service
[Service]
Type=simple
User=sharelatex
Group=sharelatex
ExecStart=/usr/bin/node /PATH_TO_SHARELATEX_PARENT_FOLDER/sharelatex/NAME/app.js
Environment=NODE_ENV=production
Environment=SHARELATEX_CONFIG=/etc/sharelatex/settings.coffee
#Restart=always
[Install]
WantedBy=multi-user.target
```
**Be carrefull :** you need to change all NAME (2x) with the name of sharelatex service's and /PATH_TO_SHARELATEX_SUB_FOLDER/

Example for chat service:
`/etc/systemd/system/sharelatex-chat.service`
```bash
[Unit]
Description=ShareLatex-web-chat
After=network.target
After=redis.service
After=mongodb.service
[Service]
Type=simple
User=sharelatex
Group=sharelatex
ExecStart=/usr/bin/node /srv/webapps/sharelatex/chat/app.js
Environment=NODE_ENV=production
Environment=SHARELATEX_CONFIG=/etc/sharelatex/settings.coffee
#Restart=always
[Install]
WantedBy=multi-user.target
```

If you name units files service in `/etc/systemd/system/` with prefix `sharelatex-` you can start/enable all sharelatex services with : 

`sudo systemctl start $(ls /etc/systemd/system/sharelatex-*)`