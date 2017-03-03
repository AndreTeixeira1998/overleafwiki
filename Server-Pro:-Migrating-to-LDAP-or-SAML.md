# Server Pro: Migrating to LDAP or SAML

This document describes a process for migrating an existing ShareLaTeX Server Pro
system from Native Auth (the default auth system) to use an external authentication
systems like [LDAP](https://github.com/sharelatex/sharelatex/wiki/Server-Pro:-LDAP-Config) or [SAML](https://github.com/sharelatex/sharelatex/wiki/Server-Pro:-SAML-Config).

This document will use LDAP as an example, but the same procedure applies for SAML too.

## Native -> LDAP Example

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


### Step 1: Instruct Users to Update Their Emails

ShareLaTeX user accounts are tied to email addresses, regardless of the
authentication system being used, so the first step is to ask all of our users
to update their ShareLaTeX email addresses to match the email in their LDAP
accounts.

Alice should set her ShareLaTeX email to `'alicejones@tech.example.com'`, and Bob should set
his to `'bobsmith@sales.example.com'`.

While we're getting the users to update their email addresses, we (the Admins) should
update our own ShareLaTeX email address too, as this change applies to all user accounts,
not just those of non-admin users.


### Step 2: Enable the LDAP Module

Once the users have updated their email addresses, we can activate the LDAP module by [setting
the appropriate environment variables](https://github.com/sharelatex/sharelatex/wiki/Server-Pro:-LDAP-Config) and restarting the ShareLaTeX system.
This will replace the default authentication mechanism with LDAP. Users who want to log in will be
greeted by an LDAP credentials form, instead of the default Login form.


### Step 3: Users Log In Via LDAP

If Alice were to go to this new login page and enter her LDAP credentials, she will be logged in
to her existing account (because the email address matches the address in LDAP), and will have
access to all her existing projects.

If, for whatever reason, we need to roll back to using Native Auth, then it's as easy as
removing the LDAP-related configuration and restarting the ShareLaTeX service. The users old
email/password will still be there for them to log in with.


## Going the Other Way: Migrating from LDAP to Native Auth

Let's imagine that we've been running our ShareLaTeX system with LDAP auth from the start, but
we now want to migrate to using ShareLaTeX's default "Native Auth" system.


### Step 1: Ensure All Users Can Use Their LDAP Email

Each user account will currently be associated with an email address,
the email from their LDAP profile. This will be the email address the user will use to log
in from now on.


### Step 2: Disable the LDAP Module

We start by [unsetting the LDAP configuration options](https://github.com/sharelatex/sharelatex/wiki/Server-Pro:-LDAP-Config), and restarting the ShareLaTeX service.


### Step 3: Ask Users To Perform a Password-Reset

When a user tries to log in now, they will be greeted by an email/password form, not the LDAP
credentials form they would have been familiar with. Each user should now click
the "Forgot your password?" link below the Login form. The user should fill in their email address
(again, the email from their LDAP profile), and submit the Password-Reset form.

The user should then receive an email with a link with which they can set a new password.

The user can then log in using their email address and this new password.
