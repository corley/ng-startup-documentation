# App structure and config

![foldering](http://ngstartup.corleycloud.com/assets/images/ngstartup02.png)


## Foldering
```
.
├── bower.json                      // bower metadata
├── build.config.js                 // list of files (css/js/less/sass..) using during build and compile
├── configuration.json.dist         // Template for your external config
├── config.xml                      // Cordova configuration
├── Gruntfile.js                    // Grunt configuration file
├── karma                           // karma configuration
│   └── karma-unit.tpl.js           // karma configuration template
├── LICENSE                         // license
├── module.prefix                   // Wrapper of compiled application
├── module.suffix                   // Wrapper of compiled application
├── package.json                    // npm metadata
├── README.md                       // presentation file
├── src                             // source of your application
│   ├── app                         // app module
│   ├── assets                      // images, video, icon..
│   ├── i18n                        // translations
│   ├── index.html                  // entry point
│   ├── less                        // less files
│   └── sass                        // sass files
└── tpl                             // templates directory
    └── conf.tpl.js                 // configuration template, grunt use it to create your custom conf.js file
```

## Package.json


## App configuration


The code generated is optimized for production:
* every js are minified and concat in only one file
* compile less, sass and all css in only one file

## Manage your dependencies
Every application requires a way to include javascript/css files into the index.html, we use `build.config.js` to describe all
dependencies.
This file contains in first **build** and **compile** destination directory and a lot of roles to search and include different files.
If you install bower packages for example you should add the new files here.
