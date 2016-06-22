Sharelatex LDAP
==============
Available in ShareLaTeX Server Pro is the ability to use a LDAP server to manage users. It is also possible to use with Active Directory systems.


Config
---

* `SHARELATEX_LDAP_HOST`: The host where your LDAP server is located. e.g. 'ldap://ldap.mydomain.com:389'
* `SHARELATEX_LDAP_DN`: The DN for your LDAP. e.g. 'root' or more complex such as 'uid=:userKey,ou=people,dc=mydomain,dc=com'
* `SHARELATEX_LDAP_BASE_SEARCH`: e.g. 'ou=people,dc=mydomain,dc=com'
* `SHARELATEX_LDAP_FILTER`: LDAP search filter e.g '(uid=:userKey)'
* `SHARELATEX_LDAP_ANONYMOUS`: Set to true for anonymous LDAP search
* `SHARELATEX_LDAP_EMAIL_ATT`: The email attribute the LDAP server will return, defaults to `mail`
* `SHARELATEX_LDAP_NAME_ATT`: The property name holding the name of the user which is used in the application.  
* `SHARELATEX_LDAP_LAST_NAME_ATT`: If your LDAP server has a first and last name then this can be used in conjutor with `SHARELATEX_LDAP_NAME_ATT`
* `SHARELATEX_LDAP_PLACEHOLDER`: The placeholder for the login form, defaults to email@example.com

* `SHARELATEX_LDAP_ADMIN_DN`: Used if an admin DN is needed e.g. 'cn=read-only-admin,dc=example,dc=com'
* `SHARELATEX_LDAP_ADMIN_PW`: Password used for an Admin DN. 



* `SHARELATEX_LDAP_TLS`: Used to turn on starttls, defaults to false
* `SHARELATEX_LDAP_TLS_OPTS_REJECT_UNAUTH`: If true, the server certificate is verified against the list of supplied CAs.

* `SHARELATEX_LDAP_TLS_OPTS_CA_PATH`: A JSON array of paths to the CA file for TLS, must be accessible to the docker container. E.g. `-env SHARELATEX_LDAP_TLS_OPTS_CA_PATH='["/var/one.pem", "/var/two.pem"]'`



LDAP Config Example
----------------
The following is an example basic config using the public accessible forumsys.com server. You can test LDAP is setup with this config, login using a username of `einstein` and password of `password`

```
--env SHARELATEX_LDAP_HOST='ldap://ldap.forumsys.com' 
--env SHARELATEX_LDAP_DN='uid=:userKey,dc=example,dc=com' 
--env SHARELATEX_LDAP_BASE_SEARCH='dc=example,dc=com' 
--env SHARELATEX_LDAP_FILTER='(uid=:userKey)' 
--env SHARELATEX_LDAP_ADMIN_DN='cn=read-only-admin,dc=example,dc=com' 
--env SHARELATEX_LDAP_ADMIN_PW='password' 
```


### Testing config & Debugging

As LDAP is heavily configurable and flexable by nature it can be a good starting point to have a working example with ldapsearch or even used by another applicaiton.

```
#!bash
ldapsearch -H ldap://ad.mydomain.com:389 -x -D ENUMuser@mydomain.com -w ENUMpass  -b ou=people,dc=mydomain,dc=com "CN=\*ENUMuser\*" mail
```

License
-------

Copyright (c) ShareLaTeX, 2015.