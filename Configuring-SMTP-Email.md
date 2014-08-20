When Sharelatex sends email, it can do so through a local or remote SMTP server, using the nodemailer options.

In the config file there are a number of email options which can be specified, the following should work for most email servers.
There is more info at the nodemailer repository: https://github.com/andris9/Nodemailer

```
        email:
                fromAddress: "from@yourdomain.com"
                replyTo: "sharelatex_support@yourdomain.com"
                transport: "SMTP"
                parameters:
                        host: "mailserver.yourdomain.com"
                        port: "25"
                        # Authentication
                        authMethod: "PLAIN"
                        auth: {
                                user: 'mailer@yourdomain.com',
                                pass: 'password'
                              }
                        # Secure connections:
                        # NB: nodemailer can have some issues with SSLv3
                        secureConnection: false
        #               secureProtocol: "SSLv3_method"
        #               tls:  {
        #                       ciphers: 'SSLv3'
        #                     }
```

You may also like to edit the file: /web/app/coffee/Features/Email/EmailBuilder.coffee
This will allow you to format emails to your liking.