# Grunt and Bower

## Introduction
[Grunt](http://gruntjs.com) and [Bower](http://bower.io) are two node modules that help you with development and dependency management.

## Grunt tasks

[Grunt](http://gruntjs.com) is a node tasks manager that help you to configure and reuse your application flow.

> With Grunt you can easily compile your source code (/src) for different stages (development and production) and platforms (web and mobile)

The first command you have to know is


```bash
grunt watch:web
```

This command will compile your `/src` creating a version  (in `/dist/build/web`) that you can visit on `http://127.0.0.1:8081`, then will wail for any change to codes in `src/` rebuilding again the app when you edit your files. **Launch this command when you are coding**.

![watching](http://ngstartup.corleycloud.com/assets/images/ngstartup04.png)


Another important command is

```bash
grunt compile:web
```

It compile your `/src` creating a version (in `/dist/compile/web`) ready for production, with js and css files minified an compressed, and with angular html templates injected in the app in order to enhance the performance. Then start a server (as the watch command) and you can visit you compiled app visiting `http://127.0.0.1:8081`.

### All commands

Command               | Description
----------------------| ----------------
build:web           | create the app (not minified or compressed) in the `/dist/build/web` folder and executes all tests
build:mobile           | the same of above, but in the `/dist/build/mobile` using files and instructions for mobile env, then create a zip with your `config/phonegap.config.xml` and push this archive to `/build.phonegap.com` (with credentials in `/config/configuration.json`), wait for building process ending, then download the apk and install it on device (is connected on usb)
watch:web           | the same of build:web, but doesn't execute the tests and starts the server (http://127.0.01:8081) watches&recompiling for any changes to the code. **best task when you're coding**
watch:mobile           | the same of above, but using files and instructions for mobile env
compile:web         | create the app (minified and compressed) in the `/dist/compile/web/` folder and executes all tests, then starts the server (http://127.0.01:8081) **best task to produce the app reado for deploy**
compile:mobile         | the same of above, but in the `/dist/build/mobile/` using files and instructions for mobile env, then create a zip with your `/config/phonegap.config.xml` and push this archive to `build.phonegap.com` (with credentials in `config/configuration.json`), wait for building process ending, then download the apk and install it on device (is connected on usb)
test:web | run tests for  `/dist/build/web`
test:mobile | run tests for  `/dist/build/mobile`
bump:patch      | edit the package.json increasing the app version, for example from `0.1.0` to `0.1.1`
bump:minor      | edit the package.json increasing the app version, for example from `0.1.0` to `0.2.0`
bump:major      | edit the package.json increasing the app version, for example from `0.1.0` to `1.0.0`


Plase read official [documentation](https://github.com/vojtajina/grunt-bump) for GruntJS.

> Grunt helps you to write better code, notifyng when you do js syntax error or wrong practices that could affect the minification of the app.


### List of Grunt dependencies
This is the list of our dependencies
```
grunt.loadNpmTasks('grunt-contrib-clean');
grunt.loadNpmTasks('grunt-contrib-copy');
grunt.loadNpmTasks('grunt-contrib-jshint');
grunt.loadNpmTasks('grunt-contrib-concat');
grunt.loadNpmTasks('grunt-contrib-watch');
grunt.loadNpmTasks('grunt-contrib-uglify');
grunt.loadNpmTasks('grunt-contrib-compress');
grunt.loadNpmTasks('grunt-bump');
grunt.loadNpmTasks('grunt-aws');
grunt.loadNpmTasks('grunt-contrib-less');
grunt.loadNpmTasks('grunt-contrib-sass');
grunt.loadNpmTasks('grunt-shell');
grunt.loadNpmTasks('grunt-karma');
grunt.loadNpmTasks('grunt-devcode');
grunt.loadNpmTasks('grunt-phonegap-build');
grunt.loadNpmTasks('grunt-ng-annotate');
grunt.loadNpmTasks('grunt-html2js');
grunt.loadNpmTasks('grunt-http-server');
```

## Bower

[Bower](http://bower.io) is a package manager, ngStartup uses it to download its frontend dependencies (for example angular.js and jQuery).
Bower.json contains metadata and requirements for your application. `install` command is already run after npm install but it supports few other commands.
In first this command updates your bower dependencies.

> You can think at 2 different groups of sources in the app: a part from `/src` (your sources) and a part from `/bower_components`  (external libraries) downloaded by bower

### Add a new dependency

To add a dependency, edit `/bower.json`:

```
  //...
  "dependencies": {
    "bootstrap": "~3.3",
    "add-your-dependecy-here": "version"
  }

```

Then run the command:

```bash
bower update
```

It will download the package in the `/bower_components` folder.
The last step is choose which css and js files you want to import into the app, editing the `build.config.js` file.
The `/config/build.config.js` contains the description of files to load in your application, you can add bower dependencies, different path for your javascript and a lot of other customization.
It manage also phonegap assets, you can decide to load a particular css only for phonegap or only for web version:

```bash
  vendor_files: {
    js: [
      'bower_components/angular/angular.js',
      'bower_components/jquery/dist/jquery.js',
      'bower_components/bootstrap/dist/js/bootstrap.js',
      'bower_components/your-dependecy/dependency.js'
      // ...
```
