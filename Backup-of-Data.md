There are 3 things which need to be backed up

* Mongodb
* ShareLaTeX Filesystem data
* Redis

It is not essential to backup Redis, it is only used for short term data.

## Backup Tips
Backups need to be on a separate server to the one ShareLaTeX running on, ideally in a different location entirely. Running multiple instances of mongodb is also not a backup as it will not prevent against corruption.

## Mongodb
mongodb comes with a tool called [mongodump](https://docs.mongodb.com/manual/reference/program/mongodump/) which will export the dataset in a safe backup format. 

## Filesystem data 
The path where your files are stored is specified in your settings file. This might be: `/var/lib/sharelatex`, copying this directory recursively is needed. rsync is a good tool for the job. 

## Redis
Redis stored short term data so is the least important thing to backup, however it is still worth doing. To backup redis you can copy the RDB file to a secure location.
