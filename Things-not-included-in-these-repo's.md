ShareLaTeX was not built to be open source from the start, its history is quite organic, there are still some minor parts which are available on sharelatex.com:

# /templates 
We can not include this at the moment because we use a theme we purchased that we are unable to redistribute. Once we redesign this site with our own styles we will likely open source it. Actual template files are not stored inside of the git repo so they will not be included. We can look at either offering a mass export of templates or allow other instances of /templates to pull from our central templates server.

# /learn
This is our wiki, it is just an instance of media wiki. There are no plans to allow people to copy this at the moment.

# /dropbox
we are planning a big rewrite of dropbox soon to work with there real time api, we will likely open source it when development starts on this.

# chat and spellcheck

These are two separate APIs. Both have a complicated set up and need a bit of TLC before we comfortable supporting them as open-source projects. They should be available soon though.

# snapshots - history
The new version of history is [open sourced](https://github.com/sharelatex/track-changes-sharelatex) with the previous version being deprecated.

# artwork
We do not own the rights to redistribute a lot of the artwork so different versions are included in the github repos.

# static pages
Pages like the home page are not included any more, this is because they are generally not needed by sites other than sharelatex.com, it also reduces confusion for our users and minimises and SEO issues. We may look into having a different home page which can be customised to your needs in the future.
