When ShareLaTeX sends email, it can do so through a local or remote SMTP server, using the nodemailer options.

### Installing an MTA locally

If you do not have a mail server running, you have various choices of installing an e-mail MTA locally, such as [Exim](https://help.ubuntu.com/lts/serverguide/exim4.html). Make sure that the SMTP server is listening to connections on localhost, and that it is not publicly open (as this could make your server a spam relay). This should be the default setting, e.g. for Exim.

### Configuring Nodemailer
In the config file there are a number of email options which can be specified, the following should work for most email servers. There is [more info at the nodemailer repository](https://github.com/andris9/Nodemailer).

Important notes:
- ShareLaTeX only supports SMTP as transports. `sendmail` and others do not work. If an e-mail server is enabled on your localhost, and it's listening to local connections, using SMTP will also work.
- You have to specify the `parameters` (at least `host` and `port` will be recommended). Otherwise, e-mail will not be enabled.


```coffee
        email:
                fromAddress: "from@example.com"
                replyTo: "sharelatex_support@example.com"
                transport: "SMTP"
                parameters:
                        host: "mailserver.example.com"
                        port: "25"
                        # Authentication
                        authMethod: "PLAIN"
                        auth: {
                                user: 'mailer@example.com',
                                pass: 'password'
                              }
                        # Secure connections:
                        # NB: nodemailer can have some issues with SSLv3
                        secure: false
        #               secureProtocol: "SSLv3_method"
        #               tls:  {
        #                       ciphers: 'SSLv3'
        #                     }
```


The following configuration was successfully tested using `postfix/sendmail` where `postfix` was configured as a relay, i.e., using an external smtp and might work for different `postfix` configurations as well:


```coffee
        email:
                fromAddress: "from@example.com"
                replyTo: "sharelatex_support@example.com"
                transport: "Sendmail"
                parameters:
                        path: "/path/to/sendmail" # e.g. "/usr/sbin/sendmail"
```

### Editing templates

You may also like to edit the file: `/web/app/coffee/Features/Email/EmailBuilder.coffee`. This will allow you to format emails to your liking. A recompile will be needed after editing this file with grunt compile:server