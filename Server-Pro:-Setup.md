The ShareLaTeX Server Pro image is used and configured in the same way as the open source ShareLaTeX Docker image. Instead use the image quay.io/sharelatex/sharelatex-pro.

```
$ docker run -d \
  -v ~/sharelatex_data:/var/lib/sharelatex \
  -p 5000:80 \
  --name=sharelatex \
  --privileged \
  quay.io/sharelatex/sharelatex-pro
```

