# Grunt and Bower


## Grunt tasks

[Grunt](http://gruntjs.com) is a node tasks manager that help you to configure and reuse your application flow.

This is a list of the major tasks available ngStartup.

```bash
grunt appconf
```
This command reads `configuration.json` and creates `src/config.js` by `tpl/conf.tpl.js` template.

```bash
grunt watch
```
![watching](http://ngstartup.corleycloud.com/assets/images/ngstartup04.png)


This command build your application in development mode, create webserver (port 8081) and it expects a file update to re-build your app and refresh browser page.

```bash
grunt web-employee
```
This command build your application in development mode.

```bash
grunt compile
```
This command prepare your code for production environment: minify, concat files ...

```bash
grunt bump:major // minor or patch
```
This command increase package.json version key of one major/minor/patch release. **The version is used for names of compiled css and js files**.
Example:


```
Start  | Command          | End  |
-------| ---------------- | -------------
0.1.0  | grunt bump:major | 1.0.0
0.1.0  | grunt bump:minor | 0.2.0
0.1.0  | grunt bump:path  | 0.1.1
```


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
