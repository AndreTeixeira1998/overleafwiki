It is possible to enforce password restrictions on users when using the ShareLaTeX login system, not a SSO option such as LDAP.

* `SHARELATEX_PASSWORD_VALIDATION_MIN_LENGTH`: The minimum length required

* `SHARELATEX_PASSWORD_VALIDATION_MAX_LENGTH`: The Maximum length allowed

* `SHARELATEX_PASSWORD_VALIDATION_PATTERN`: is used to validate password strength

 - `abc123` – password requires 3 letters and 3 numbers and be at least 6 characters long
 - `aA` – password requires lower and uppercase letters and be 2 characters long
 - `ab$3` – it must contain letters, digits and symbols and be 4 characters long
 - There are 4 groups of characters: letters, UPPERcase letters, digits, symbols. Everything that is neigher letter, nor digit is considered to be a symbol.