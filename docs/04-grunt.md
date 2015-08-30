# Grunt and Bower


## Grunt tasks

[Grunt](http://gruntjs.com) is a node tasks manager that help you to configure and reuse your application flow.

The first command that you have to know is:


```bash
grunt watch
```

This command will compile your `src/` creating a version that you can visit on `http://127.0.0.1:8081`, then will wail for any change to codes in `src/` rebuilding again the app when you edit your files. **Launch this command when you are coding**.

![watching](http://ngstartup.corleycloud.com/assets/images/ngstartup04.png)


Another important command is

```bash
grunt compile
```

It compile your `src/` creating a version ready for production, with js and css files minified an compressed, and with angular html templates injected in the app in order to enhance the performance. Then start a server (as the watch command) and you can visit you compiled app visiting `http://127.0.0.1:8081`.

### All commands

Command               | Description
----------------------| ----------------
watch           | create the app (not minified and compressed) in the `compile/build/web/` folder, then start the server and watch for any changes to the code
build           | the same of the wathc command, but doesn't start the server and doesn't watch code changes
compile         | create the app (minified an compressed) in the `compile/compile/web/` folder, then start the server
phonegapBuild   | create the app (not minified and compressed) in the `compile/build/mobile/` folder, then push the code to phonegap, wait for building process, then download the app and install it on device (is connected on usb)
phonegapCompile   | create the app (minified and compressed) in the `compile/compile/mobile/` folder, then push the code to phonegap, wait for building process, then download the app and install it on device (is connected on usb)
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

[Bower](http://bower.io) is a package manager, ngStartup uses it download its front end dependencies.
Bower.json contains metadata and requirments for your application. `install` command is already run after npm install but it supports few other commands.
In first this command updates your bower dependencies.

### Add a new dependency

To add a dependency, add it to `/bower.json`:

```

```

Then run the command:

```bash
bower update
```

It will download the package in the '/bower_component' folder.
The last step is choose which css and js files you want to import into the app, editing the `build.config.js` file.
The `build.config.js` contains the description of files to load in your application, you can add bower dependencies, different path for your javascript and a lot of other customization.
It manage also phonegap assets, you can decide to load a particular css only for phonegap or only for web version.
