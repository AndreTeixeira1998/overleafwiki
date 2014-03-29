This assumes that each individual service (web-sharelatex, document-updater, etc) have been deployed using [Capistrano](https://github.com/capistrano/capistrano) or [Chef](http://www.getchef.com/chef/), and so have their main directories in /var/www/document-updater/current.

	description "document-updater"
	author      "James Allen <james@sharelatex.com>"

	start on started mountall
	stop on shutdown

	respawn

	limit nofile 8192 8192
			
	script
		echo $$ > /var/run/document-updater.pid
		chdir /var/www/document-updater/current
		exec sudo -u www-data env NODE_ENV=production /opt/node-v0.8.19/bin/node app.js >> log/production.log 2>&1
	end script