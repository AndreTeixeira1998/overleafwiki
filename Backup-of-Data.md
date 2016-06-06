There are 3 things which need to be backed up

* Mongodb
* ShareLaTeX Filesystem data
* Redis

It is not essential to backup Redis, it is only used for short term data.

## Mongodb
mongodb comes with a tool called [mongodump](https://docs.mongodb.com/manual/reference/program/mongodump/) which will export the dataset in a safe backup format. 

## Filesystem data 
The path where your files are stored is specified in your settings file. This might be: /var/lib/sharelatex`

## Redis
To backup redis you can copy the RDB to a secure location
