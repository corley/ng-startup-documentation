# App structure and scafolding

## Foldering
```
.
├── bower.json                      // bower metadata
├── build.config.js                 // list of files (css/js/less/sass..) using during build and compile
├── configuration.json.dist         // Template for your external config
├── config.xml                      // Cordova configuration
├── Gruntfile.js                    // Grunt configuration file
├── karma
│   └── karma-unit.tpl.js
├── LICENSE
├── module.prefix                   // Wrapper of compiled application
├── module.suffix                   // Wrapper of compiled application
├── package.json                    // npm metadata
├── README.md
├── src                             // source of your application
│   ├── app                         // app module
│   ├── assets
│   ├── i18n                        // translations
│   ├── index.html                  // entry point
│   ├── less                        // less files
│   ├── README.md
│   └── sass                        // sass files
└── tpl                             // templates directory
    └── conf.tpl.js
```

## Development environment
Developement environment works into the `build` directory. This is generated for web environment by
```
grunt web-eployee
```
For cordova by
```
grunt phonegapBuild
```
And it is called during watch process every file update.
The code in this directory is debuggable becuase it is not concat, compress or minified.

## Production environment
Prodcution enviroment works inside `bin` directory it is generated for web environment from grunt
```
grunt compile
```

For cordova by
```
grunt phonegapCompile
```

The code generated is optimized for production:
* every js are minified and concat in only one file
* compile less, sass and all css in only one file

## Loader and Inject
Every application requires a way to include javascript/css files into the index.html, we use `build.config.js` to describe all
dependencies.
This file contains in first **build** and **compile** destination directory and a lot of roles to search and include different files.
If you install bower packages for example you should add the new files here.
