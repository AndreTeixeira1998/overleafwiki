Overleaf only supports SMTP and AWS SES. If an e-mail server is enabled on your localhost, and it's listening to local connections, using SMTP will also work.


## Configure

Email can be configured via environmental variables passed to the docker container.

##### Sender Configuration

* `SHARELATEX_EMAIL_FROM_ADDRESS`: **REQUIRED** - The from address e.g. `'support@mycompany.com'` 
* `SHARELATEX_EMAIL_REPLY_TO`: The reply to address e.g. `'noreply@mycompany.com'`

##### AWS SES
* `SHARELATEX_EMAIL_AWS_SES_ACCESS_KEY_ID`: If using AWS SES the access key
* `SHARELATEX_EMAIL_AWS_SES_SECRET_KEY`: If using AWS SES the secret key

##### AWS SES with Instance Roles
* `SHARELATEX_EMAIL_DRIVER`: When this is set to `ses`, the email system will rely on
  the configured instance roles to send email.

##### SMTP
* `SHARELATEX_EMAIL_SMTP_HOST`: SMTP Host, needs to be accessible from the docker container
* `SHARELATEX_EMAIL_SMTP_PORT`: SMTP port to use
* `SHARELATEX_EMAIL_SMTP_SECURE`: Boolean if SMTP secure should be used
* `SHARELATEX_EMAIL_SMTP_USER`: User to authenticate against the STMP server with
* `SHARELATEX_EMAIL_SMTP_PASS`: Password for SMTP User
* `SHARELATEX_EMAIL_SMTP_TLS_REJECT_UNAUTH`: Rejected unauthorized tls connections
* `SHARELATEX_EMAIL_SMTP_IGNORE_TLS`: turns off STARTTLS support if true

#### Customisation

* `SHARELATEX_CUSTOM_EMAIL_FOOTER` Custom HTML which is appended to all emails. e.g. `--env SHARELATEX_CUSTOM_EMAIL_FOOTER="<div>This system is run by department x </div> <div> If you have any questions please look at our faq <a href='https://somwhere.com'>here</a></div>"`
