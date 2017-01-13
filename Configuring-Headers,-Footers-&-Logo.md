## Header

#### Site title

The nav title can be set with SHARELATEX_NAV_TITLE, this is used in the top left corner of navigation if a logo is not set.

```
--env SHARELATEX_NAV_TITLE='My ShareLaTeX Instance'
```

#### Header Navigation links

Extra navigation items can be added to the navigation header by setting the `SHARELATEX_HEADER_EXTRAS` option to a JSON array of objects. For example:

    [
      {
        "text": "Some link",
        "url": "http://example.com/somelink",
        "class": "subdued",
        "only_when_logged_out": true
      },
      {
        "text": "Help",
        "class": "subdued"
        "dropdown": [
          {
            "text": "Documentation",
            "url": "/learn"
          },
          {
            "text": "Contact Admin",
            "url": "http://example.com/contact"
          }
        ]
      }
    ]

    # As an --env option
    --env SHARELATEX_HEADER_NAV_LINKS='[{"text":"Somelink","url":"http://example.com/somelink","class":"subdued","only_when_logged_out":true},{"text":"Help","class":"subdued""dropdown":[{"text":"Documentation","url":"/learn"},{"text":"ContactAdmin","url":"http://example.com/contact"}]}]'


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




