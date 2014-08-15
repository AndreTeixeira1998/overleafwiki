We will always have lots of little issues and bugs that need looking at, but this page documents some of the bigger features/improvements that have active focus from the core team at the moment. If you're interested in helping with any of these, please drop in our chat room at http://www.hipchat.com/g1nJMcj7b and let us know.

## Internationalisation

The front-end of ShareLaTeX needs to be converted to use an internationalization library like [i18n](https://github.com/mashpie/i18n-node) or [i18n-2](https://github.com/jeresig/i18n-node-2). Once this is done, it will allow volunteers to provide translations into their language of choice. See issue [sharelatex/sharelatex#22](https://github.com/sharelatex/sharelatex/issues/22)

## Allow filestore to use local storage

Replace the s3Wrapper module in the filestore with a module that writes and read from the local disk. See issue [sharelatex/filestore#3](https://github.com/sharelatex/filestore-sharelatex/issues/3)

## Tracking changes

We are currently developing a new history system that will record every keystroke and change made to a document, along with who made it. This will allow lots of fun features like:

* Rewind to an arbitrary point in time in the document
* Show a diff between any two points in time, and highlight who has inserted and deleted what
* Get notifications when you come back to a document and someone has made changes, and then highlight those changes