sharelatex-ldap
==============

A module encapsulating OpenLDAP and Active Directory authentication for ShareLaTeX Web

Dependencies
----------------
```
#!json
"ldapjs": "1.0.0"
```


LDAP Config Example
----------------
```
#!javascript
host: 'ldap://ldap.mydomain.com:389'
dn: 'uid=:userKey,ou=people,dc=mydomain,dc=com'
baseSearch: 'ou=people,dc=mydomain,dc=com'
filter: '(uid=:userKey)'
emailAtt: 'mail'
nameAtt: 'cn'
lastNameAtt: 'sn'
anonymous: false
adminDN: 'cn=read-only-admin,dc=example,dc=com'
adminPW: 'password'
starttls: true
tlsOptions:
   rejectUnauthorized: false
   ca: ['/etc/ldap/ca_certs.pem']
```

### test your LDAP server with ldapsearch (without TLS)
```
#!bash
ldapsearch -H ldap://ldap.mydomain.com:389 -D uid=ENUMuser,ou=people,dc=mydomain,dc=com -w ENUMpass -b ou=people,dc=ufrgs,dc=br "(uid=ENUMuser)" mail
```

AD Config Example
----------------
```
#!javascript
host: 'ldap://ad.mydomain.com:389'
dn: ':userKey@mydomain.com'
emailAtt: 'mail'
nameAtt: 'cn'
filter: 'CN=\*:userKey\*'
baseSearch: 'ou=people,dc=mydomain,dc=com'
```

### test your AD config with ldapsearch
```
#!bash
ldapsearch -H ldap://ad.mydomain.com:389 -x -D ENUMuser@mydomain.com -w ENUMpass  -b ou=people,dc=mydomain,dc=com "CN=\*ENUMuser\*" mail
```

License
-------

Copyright (c) ShareLaTeX, 2015.