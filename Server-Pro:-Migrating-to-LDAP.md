# Server Pro: Migrating to LDAP

This document describes a process for migrating an existing ShareLaTeX Server Pro
system from Native Auth (the default auth system) to LDAP authentication.

By way of example, let's imagine we've been managing a ShareLaTeX system for a while, and we have
two users, Alice and Bob. Their ShareLaTeX accounts look something like this:

```
# Alice
{
  _id:        '123',
  email:      'alice@example.com'
  first_name: 'Alice',
  last_name:  'Jones'
}

# Bob
{
  _id:        '456',
  email:      'bob@example.com'
  first_name: 'Bob',
  last_name:  'Smith'
}
```


We would like to update our ShareLaTeX installation to use LDAP authentication rather than
the default username/password authentication.

Let's imagine that our LDAP system has two user entries, one each for both of Alice and Bob:

```
Alice:
  - uid:  'alicejones'
  - mail: 'alicejones@tech.example.com'
  - cn:   'Alice'

Bob:
  - uid:  'bobsmith'
  - mail: 'bobsmith@sales.example.com'
  - cn:   'Bob'
```

To summarize: Alice currently logs in by typing `'alice@example.com'` and her ShareLaTeX password,
but we want her to be able to log in by typing `'alicejones'` and her LDAP password instead.


## Step 1: Instruct Users to Update Their Emails

ShareLaTeX user accounts are tied to email addresses, regardless of the
authentication system being used, so the first step is to ask all of our users
to update their ShareLaTeX email addresses to match the email in their LDAP
accounts.

Alice should set her ShareLaTeX email to `'alicejones@tech.example.com'`, and Bob should set
his to `'bobsmith@sales.example.com'`.

While we're getting the users to update their email addresses, we (the Admins) should
update our own ShareLaTeX email address too, as this change applies to all user accounts,
not just those of non-admin users.


## Step 2: Enable the LDAP Module

Once the users have updated their email addresses, we can activate the LDAP module by [setting
the appropriate environment variables](https://github.com/sharelatex/sharelatex/wiki/Server-Pro:-LDAP-Config) and restarting the ShareLaTeX system.
This will replace the default authentication mechanism with LDAP. Users who want to log in will be
greeted by an LDAP credentials form, instead of the default Login form.


## Step 3: Users Log In Via LDAP

If Alice were to go to this new login page and enter her LDAP credentials, she will be logged in
to her existing account (because the email address matches the address in LDAP), and will have
access to all her existing projects, and so forth.
