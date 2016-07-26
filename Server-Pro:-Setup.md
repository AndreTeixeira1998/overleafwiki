The ShareLaTeX Server Pro image is used and configured in the same way as the open source ShareLaTeX Docker image. Instead use the image `quay.io/sharelatex/sharelatex-pro.`

```
$ docker run -d \
  -v ~/sharelatex_data:/var/lib/sharelatex \
  -p 5000:80 \
  --name=sharelatex \
  quay.io/sharelatex/sharelatex-pro
```

Basic configuration can be found on the [quickstart guide](https://github.com/sharelatex/sharelatex/wiki/Quick-Start-Guide) page, with customizations such as [LDAP](https://github.com/sharelatex/sharelatex/wiki/Server-Pro:-LDAP-Config) and [Sandboxed Compiles](https://github.com/sharelatex/sharelatex/wiki/Server-Pro:-sandboxed-compiles) available in the Server Pro image.