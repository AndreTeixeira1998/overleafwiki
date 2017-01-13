## Logs overview

ShareLaTeX is composed of multiple different processes which all write log files to /var/log/sharelatex/ it is possible to mount this directory or any file inside this directory outside of the container for easier access.

## Error logs
If an error occurs in any of the processes it will be written to the respective log file such as /var/log/sharelatex/web.log, there is a global log file where these errors are also logged out at /var/log/sharlatex-errors.log which can be mounted outside the docker container.