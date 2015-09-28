From your sharelatex directory run:

```
sudo git pull 
sudo npm install
sudo grunt install
sudo grunt update:all
```

If the option `useMinifiedJs` is set to `true` in your `settings.coffee` file, you may need to run `sudo grunt compile:minify` in the sharelatex/web directory again.