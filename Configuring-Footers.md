It is possible to customise both the left and right footer which is found on pages like `/project` using the environmental variables `SHARELATEX_LEFT_FOOTER` and `SHARELATEX_RIGHT_FOOTER`

Both expect an array of JSON which will be inserted.

	[
		{
			text: "Powered by <a href='https://www.sharelatex.com'>ShareLaTeX</a> © 2016"
		},{
			text: "Another page I want to link to can be found <a href='here'>here</a>"
		},{
			text: "I want the entire text to be a link",
			url: "http://all-text-is-url.com"
		}
	]