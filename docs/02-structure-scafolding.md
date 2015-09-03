# App structure and config

## Introduction

This is the structure of ngStartup:


```bash
.
├── bower.json                      # Grunt configuration file
├── bower_components                # folder (out of git) that contains external libraries
├── config                          # folder with app and project configuration
├── dist                            # folder (out of git) created during compiling
├── Gruntfile.js                    # Grunt configuration file
├── LICENSE                         # license
├── node_modules                    # folder (out of git) that contains npm modules
├── package.json                    # npm metadata and project description
├── README.md                       # presentation file and documentation
└── src                             # source of your application: you code here
```

As you can see the original project (before installation) has just `/src` and `/config` folders. Other folders (`/bower_components`, `/dist`, `/node_modules`) will be created by npm install but are ignored by git.




## The /src structure

The `/src` folder contains all your angular files, styles and assets. When you start working with ngStartupp, you can edit all these files adding your logic, images, and more.

```bash
.
└── src                          # source of your application
 ├── app                         # you modules (for example: news, home, login...)
 ├── common                      # common modules (for example: header, directives, helpers)
 ├── assets                      # images, videos, fonts and layou elements
 ├── i18n                        # translations in json format
 ├── index.html                  # the index of your single page application
 ├── less                        # less files (will be transformed into css)
 └── sass                        # sass files (will be transformed into css)
```

The `/src` folder is very important because contains all your codes. The most of you job will be inside this folder. Let's go to see that more in detail:

![foldering](http://ngstartup.corleycloud.com/assets/images/ngstartup02.png)

Under **Modules** chapter you can see how to add new modules in this folder.
When you run grunt build, watch and compile tasks, the code in `/src` is compiled and moved with externale dependencies (bower_components) into `/dist` folder. This strategy helps you to code better because the final result (in `/dist/compile`) will be a minified, uglified, compressed and optimized app ready for production.


## Project configuration
The config folder contains several configuration files:


```bash
.
└── config                          # folder with app and project configuration
    ├── app.config.tpl.js           # configuration that will be injected in your angular app
    ├── build.config.js             # list of js, css and assets files that will be used in compiling
    ├── configuration.json.dist     # conf out of git for security and private keys (phonegap token, AWS s3 keys...)
    ├── karma-unit.tpl.js           # test settings
    ├── module.prefix               # Wrapper of compiled application
    ├── module.suffix               # Wrapper of compiled application
    └── phonegap.config.xml         # phonegap/cordova configuration
```



You should configure you app changing:

1. `package.json` changing the name, description, version and other infos
2. `bower.json` with new dependencies
3. `/config/build.config.js` with new dependency files and switching less/sass (choose the best for you)
4. `/config/configuration.js` with reserved credentials for phonegap build and AWS S3 (out of git)

For points 2 and 3 go to *Grunt and Bower* chapter.

About point 4: this file is out of vcs (version contro system) because that it contains secret keys used by Grunt to compile your app con build.phonegap and to upload your app yo Amazon S3. In this way you don't push to git your passwords.

## App configuration

The `/config/app.config.tpl.js` file is a template for your configuration in the angular app. It you try to edit it, you'll understand that this template will be converted in a definitive angular module and injected in you app.js. So you'll be able to use the `appConf` constant inside your controllers, directives, services and other modules.

Thanks to a specific Grunt task, this file can be filled with different information for build and compile stages. Watch at an example of config file:
``` javascript
angular.module('ng-startup.appConf', [])
.constant('appConf', function() {

  var conf = {
    "endPoint": "http://test.rest.corley.it/",
    "defaultLanguage": "en_EN"
  };

  // env: build
  conf.debug = true;
  // endenv

  // env: compile
  conf.debug = false;
  // endenv

  return conf;
});
```

When you launch `grunt build:web`, `grunt build:mobile`, `grunt watch:web` and `grunt watch:mobile` the lines of code between `// env: compile` and `// endenv` will be dropped.

``` javascript
angular.module('ng-startup.appConf', [])
.constant('appConf', function() {
  var conf = {
    "endPoint": "http://test.rest.corley.it/",
    "defaultLanguage": "en_EN"
  };
  conf.debug = true;
  return conf;
});
/** it will return
{
 "endpoint": "http://test.rest.corley.it/",
 "defaultLanguage": "en_EN",
 "debug": true
}
*/
```

In `/src/app/app.js` and whenever you want, you can use the appConf constant:

``` javascript
angular.module(
  'ng-startup',
  [
    'ng-startup.appConf'
    // list of other dependencies
  ]
)
.config(['$logProvider', 'appConf', function myappConf($logProvider, appConf) {
  $logProvider.debugEnabled(appConf.debug);
}]);
```

At the other side, when you launch`grunt compile:web` and `grunt compile:mobile` tasks, the lines of code between `// env: build` and `// endenv` will be dropped. In the example above your app will enable $log debugging for build stages, and disable for compile stages.

## Phonegap configuration

Phonegap/cordova apps needs a config.xml file with list of plugins and other settings. Please refer to *Cordova/Phonegap* chapter to know how to edit `/config/phonegap.config.xml` file.
