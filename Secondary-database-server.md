Sharelatex does not need to run database services locally, you can have your database running on a separate server:

The main trick to do this is to correctly specify the mongo database connection string:

```
        mongo:
                url : 'mongodb://username:password@server_ip/sharelatex'
```
The other database interfaces just require changing the ip addresses:
```
        redis:
                web:
                        host: "server_ip"
                        port: "6379"
                        password: ""
```
Don't forget to make sure that your firewall is allowing access on these ports.