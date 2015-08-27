# Deploy

ngStartup follows all step of your development, the deploy is tha last important task to move your code in production.

## Prepare for production
```bash
grunt compile
```
This command prepares `bin` directory, it contains your code ready to go in production. It is 

Javascript files are
* concat
* minified
* compressed

SASS and LESS files are compiled and they are concat and minify with all other CSS files.

```
bin/
├── assets
│   ├── FontAwesome.otf
│   ├── fontawesome-webfont.eot
│   ├── fontawesome-webfont.svg
│   ├── fontawesome-webfont.ttf
│   ├── fontawesome-webfont.woff
│   ├── fontawesome-webfont.woff2
│   ├── fonts
│   ├── ng-startup-0.0.2.css
│   ├── ng-startup-0.0.2.js
│   ├── phonegap
│   └── README.md
├── i18n
│   └── en_EN.json
└── index.html
```
This is an example of code ready to production. 
`bin/assets/ng-startup-0.0.2.css` is the result of compression and concat of all your style files.  
Same stuff for `bin/assets/ng-startup-0.0.2.js`, it contains all javascript files compiled and minified.

`0.0.2` is the current version of your application, this value is stored into the `package.json` underd the key `version`.

## S3
```
grunt s3
```
If a task that push bin directory into an AWS s3 bucket. You can configure it into the `configuration.json` file.
