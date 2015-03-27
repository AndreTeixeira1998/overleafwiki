This page is in development, the backup process listed here is **experimental **and may break with updates.
Please check each step before using.
The sequence should also be applicable to migration.
Eventually it would be good to write a BASH script to run backups automatically.

## Locations
We will assume that your sharelatex installation is at /opt/sharelatex
please adjust the instructions below if this is not your location.

## Shutdown Sharelatex
`sudo killall node`

## Backup redis
 copy redis and mongo data to the new server then turn SL on. sent from my mobile.
…
@jpallen
Owner
jpallen commented on 8 Sep 2014

## Backup filesystem data 
The path where your files are stored is specified in your settings file.

`cd /opt/sharelatex
`