## Header

#### Site title

The nav title can be set with SHARELATEX_NAV_TITLE, this is used in the top left corner of navigation if a logo is not set.

```
--env SHARELATEX_NAV_TITLE='My ShareLaTeX Instance'
```

#### Header Navigation links

By default, ShareLaTeX shows a dynamic navigation menu in the top-right of the screen, with links for Login, Register, etc... and a User Menu when the user is logged in.

For the rare cases where you want to control the contents of that navigation-bar, you can supply a JSON Array with the `SHARELATEX_HEADER_NAV_LINKS` option. The following example shows the default navigation in json form, and as it would be supplied as an environment option:

```
[
  {
    "text": "Register",
    "url": "/register",
    "only_when_logged_out": true
  }, {
    "text": "Log In",
    "url": "/login",
    "only_when_logged_out": true
  }, {
    "text": "Projects",
    "url": "/project",
    "only_when_logged_in": true
  }, {
    "text": "Account",
    "only_when_logged_in": true,
    "dropdown": [{
      "user_email": true
    },{
      "divider": true
    }, {
      "text": "Account Settings",
      "url": "/user/settings"
    }, {
      "divider": true
    }, {
      "text": "Log out",
      "url": "/logout"
    }]
  }
]

--env SHARELATEX_HEADER_NAV_LINKS='[{"text":"Register","url":"/register","only_when_logged_out":true},{"text":"LogIn","url":"/login","only_when_logged_out":true},{"text":"Projects","url":"/project","only_when_logged_in":true},{"text":"Account","only_when_logged_in":true,"dropdown":[{"user_email":true},{"divider":true},{"text":"AccountSettings","url":"/user/settings"},{"divider":true},{"text":"Logout","url":"/logout"}]}]'



```


#### Logo
You can set a custom logo rather than using text with the variable SHARELATEX_HEADER_IMAGE_URL this points to an externally hosted image

`
--env SHARELATEX_HEADER_IMAGE_URL='https://mysite.somewhere.com/img/logo.png'
`


## Footers

It is possible to customise both the left and smaller right footer which is found on pages like `/project` using the environmental variables `SHARELATEX_LEFT_FOOTER` and the smaller `SHARELATEX_RIGHT_FOOTER`

Both expect an array of JSON which will be inserted.

	[
		{
			"text": "Powered by <a href=\"https://www.sharelatex.com\">ShareLaTeX</a> © 2016"
		},{
			"text": "Another page I want to link to can be found <a href=\"here\">here</a>"
		}
	]

This data needs to be valid JSON to work, with quotes escaped when passed through as an environmental variable


`--env SHARELATEX_LEFT_FOOTER='[{"text": "Powered by <a href=\"https://www.sharelatex.com\">ShareLaTeX</a> © 2016"},{"text": "Another page I want to link to can be found <a href=\"here\">here</a>"} ]'`




