# Grunt
[Grunt](http://gruntjs.com) is a node tasks manager that help you to configure and reuse your application flow.

## Task
This is a list of more important task that we use in ng-startup.

```bash
grunt appconf
```
This command reads `configuration.json` and creates `src/config.js` by `tpl/conf.tpl.js` template.

```bash
grunt watch
```
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
This command increase package.json version key of one major/minor/patch release.
Example:

Start  | Command          | End  |
-------| ---------------- | -------------
0.1.0  | grunt bump:major | 1.0.0
0.1.0  | grunt bump:minor | 0.2.0
0.1.0  | grunt bump:path  | 0.1.1

[oroginal docs](https://github.com/vojtajina/grunt-bump)


## List dependencies
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
