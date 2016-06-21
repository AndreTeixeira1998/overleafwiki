ShareLaTeX only supports SMTP and AWS SES. If an e-mail server is enabled on your localhost, and it's listening to local connections, using SMTP will also work.


## Configure

Email can be configured via environmental vars passed to the docker container

* `SHARELATEX_EMAIL_FROM_ADDRESS`: The from address e.g. `'support@mycompany.com'` **REQUIRED**
* `SHARELATEX_EMAIL_REPLY_TO`: The reply to address e.g. `'noreply@mycompany.com'`

##### AWS SES
* `SHARELATEX_EMAIL_AWS_SES_ACCESS_KEY_ID`: If using AWS SES the access key
* `SHARELATEX_EMAIL_AWS_SES_SECRET_KEY`: If using AWS SES the secret key

##### SMTP
* `SHARELATEX_EMAIL_SMTP_HOST`: SMTP Host, needs to be accessible from the docker container
* `SHARELATEX_EMAIL_SMTP_PORT`: SMTP port to use
* `SHARELATEX_EMAIL_SMTP_SECURE`: Boolean if SMTP secure should be used
* `SHARELATEX_EMAIL_SMTP_USER`: User to authenticate against the STMP server with
* `SHARELATEX_EMAIL_SMTP_PASS`: Pssword for SMTP User