Overleaf has been translated into multiple languages. 

The language can be set via `SHARELATEX_SITE_LANGUAGE` with one of the following options:

* `en` - English (default)
* `es` - Spanish
* `pt` - Portuguese
* `de` - German
* `fr` - French
* `cs` - Czech
* `nl` - Dutch
* `sv` - Swedish
* `it` - Italian
* `tr` - Turkish
* `cn` - Chinese (simplified)
* `no` - Norwegian
* `da` - Danish
* `ru` - Russian
* `ko` - Korean
* `ja` - Japanese

Some of these are more complete than others, we always endeavour to have all supported languages fully translated, if you would like to help with our translations please get in touch!


## Multi Language support
Overleaf can support multiple languages per container. This is done via different domains configured to respond in a set language using the env variable `SHARELATEX_LANG_DOMAIN_MAPPING` with an escaped JSON string.

    SHARELATEX_LANG_DOMAIN_MAPPING = '
        {
          "www": {
            "lngCode": "en",
            "url": "http:\/\/www.mysite.com"
          },
          "fr": {
            "lngCode": "fr",
            "url": "http:\/\/fr.mysite.com"
          },
          "de": {
            "lngCode": "de",
            "url": "http:\/\/de.mysite.com"
          },
          "sv": {
            "lngCode": "sv",
            "url": "http:\/\/sv.mysite.com"
          },
          "zh": {
            "lngCode": "zh-CN",
            "url": "http:\/\/zh.mysite.com"
          },
          "es": {
            "lngCode": "es",
            "url": "http:\/\/es.mysite.com",
          }
        }'
