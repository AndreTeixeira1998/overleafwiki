The ShareLaTeX Server Pro image is used and configured in the same way as the open source ShareLaTeX Docker image. You will want to pass the `--privileged` flag through to the container if you plan on using the secure compiles feature. 

```
$ docker run -d \
  -v ~/sharelatex_data:/var/lib/sharelatex \
  -p 5000:80 \
  --name=sharelatex \
  --privileged \
  sharelatex-server-pro
```