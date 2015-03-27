This page is in development, the backup process listed here is **experimental **and may break with updates.
Please check each step before using.
The sequence should also be applicable to migration.
Eventually it would be good to write a BASH script to run backups automatically.

## Locations
We will assume that your sharelatex installation is at `/opt/sharelatex`
and that your backups are stored in `/backup/sharelatex`
please adjust the instructions below if this is not your location.

## Shutdown Sharelatex
`sudo killall node`

## Backup your config files
`cd /opt/sharelatex
cp -a config /backup/sharelatex/
`

## Backup redis
 copy redis and mongo data to the new server

## Backup mongo


## Backup filesystem data 
The path where your files are stored is specified in your settings file.
so for example, this might be: `Path.resolve(__dirname + "/../user_files)`
which in our case would map to: `/opt/sharelatex/user_files`

`cd /opt/sharelatex
`

## restart Sharelatex