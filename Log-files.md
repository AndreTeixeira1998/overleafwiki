## Logs overview

Overleaf is composed of multiple different processes which all write log files to `/var/log/sharelatex/`. It is possible to mount this directory or any file inside this directory outside of the container for easier access.

## Error logs
If an error occurs in any of the processes it will be written to the respective log file such as `/var/log/sharelatex/web.log`.

## Tracking project access
It is possible to see who and when a project is loaded with the following command 

`grep "join project request" /var/log/sharelatex/web.log`

This will give both the `timestamp`, `user_id` and `project_id`.