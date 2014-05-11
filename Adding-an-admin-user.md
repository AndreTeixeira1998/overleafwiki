Admin users have the capability to access any project and can go to /admin which has the capability to put the site in maintenance mode. The user needs to be set to admin via the mongo command line


	mongo
	use sharelatex
	db.users.update({email:"INSERT_EMAIL_HERE"}, {"$set":{isAdmin:true}})

you will need to log out then log back in again after running this.