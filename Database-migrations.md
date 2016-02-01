Some times we need to change the schema of data in the database as we evolve ShareLaTeX, migration scripts are used to automate this process. They will have been run on ShareLaTeX.com first which is the largest instance of ShareLaTeX in the world so most eventualities will have been encounter already, however we make no guarantees over your data. Please backup before running any migrations.

### BEFORE RUNNING ANY MIGRATIONS BACKUP YOUR DATA!!!



##### Using ShareLaTeX in official Docker Release
When upgrading to a new docker image any migrations which have not yet been run will be automatically executed, this may take some time depending on the size of your dataset, tailing the logs will let you know how it is going.

##### Running your own instances of ShareLaTeX
If you do not use the recommended docker install and instead choose to run the processes yourself the the grunt command `grunt migrate` will run the migrations for you.