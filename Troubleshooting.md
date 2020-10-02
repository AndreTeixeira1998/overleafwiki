Here are some possible causes for errors in the logs.

## CLSI

Log location in the container: `/var/log/sharelatex/clsi.log`

### `(HTTP code 400) unexpected - OCI runtime create failed`

#### `...opt/synctex` ... `not a directory`

Full message reads roughly (after removing quite a lot of noise):
`<path> is not a directory; Are you trying to mount a directory onto a file (or vice-versa)? Check if the specified host path exists and is the expected type`

Check the value of the `SYNCTEX_BIN_HOST_PATH` environment variable --- please see [this documentation](https://github.com/overleaf/overleaf/wiki/Server-Pro:-sandboxed-compiles#mapping-the-location-of-synctex-in-the-host).