# ngStartup

## What is it?

ngStartup is a project for development of robust single page applications with **AngularJS 1.3.x and other modern tools**.

> You can think about ngStartup as
> "a complete starting point for your apps".

ngStartup provides a `single page application structure` (SPA), where you have a single html file (the index.html) with `ng-app` initialized on the `<html>` tag.
Your controllers will be injected in the body working with easy to define routes.

Thanks to tools such as `Bower` & `GruntJS` you can work very quickly with ngStartup: you can define, download and integrate external components and libraries with Bower, and Grunt will compile your app both for web and mobile releases.

### The flow: you code, Grunt compiles

![intro](http://ngstartup.corleycloud.com/assets/images/ngstartup01.png)

## Installation
You can clone [ng-startup repository](https://github.com/corley/ng-startup) from github
```bash
git clone git@github.com:corley/ng-startup.git
cd ng-startup
npm install
```
Npm will install nodejs by package.json then bower dependencies (we'll talk about that later.)
The installation is completed.

## Getting Started
After installation run:
```
grunt watch
```
ngStartup (thanks to Grunt) compiles your app creating a development version that you can visit on `http://127.0.0.1:8081`.

### What happens?
The flow is very simple:

* your code is under `/src`
* grunt prepares the folder `/build`
* grunt parse javascript files, views, assets and less/sas under `/src` and compile & move them to `/build`
* grunt move bower dependecies (js and css) and move them to `/build`
* grunt run a local server that points to `/build` so you can visit the app to `http://127.0.0.1:8081`

> So remember that you work under /src and your app (compiled in development stage) is under /build

### The Single Page Application

As single page application, you have just a html file. The `/src/index.html` (**simplified to make it easier readable**) it's similar to this one:
``` html
<!DOCTYPE html>
<html ng-app="ng-startup" ng-controller="AppCtrl">
  <head>
  </head>
  <body>
    <div class="container">
        <div ui-view="main" ></div>
    </div>
  </body>
</html>
```

AppCtrl is the main controller that you find in `/src/app/app.js`:
``` javascript
angular.module(
  'ng-startup',
  [
    'my-first-module'
    // list of dependencies
  ]
)
.config([function myappConf() { }])
.run([function run() { }])
.controller('AppCtrl', ['$rootScope', function AppCtrl($rootScope) { }]);
```
Each module that you want to create will be placed under `/src/app/app.js` and will have a structure like this:

``` javascript
angular.module( 'my-first-module', ['ui.router.state'])
.config(function config( $stateProvider ) {
  $stateProvider.state( 'home', {
    url: '/home',
    views: {
      "main": {
        controller: 'HomeCtrl',
        templateUrl: 'home/home.tpl.html'
      }
    }
  });
})
.controller( 'HomeCtrl', ['$scope', '$stateParams', function HomeController( $scope, $stateParams ) {
  $scope.name = "Bruce Wayne";
}])
;
```
and a template `home/home.tpl.html` like this:
``` html
<p>Hello {{ name }}</p>
```


It's very easy: the module work on route `home`: it means that if somebody visits the `127.0.0.1:8081\#home` path ngStartup will run the controller `HomeCtrl` with the view `home/home.tpl.html` and put the result into `<div ui-view="main" ></div>` of the index.



## How to develop with ngStartup
Developement environment works into the `/build` directory. This is generated for web environment by
``` bash
grunt build # create the /build
# or
grunt watch # create the /build and wathc changes in /src, useful while coding
```
For cordova by
``` bash
grunt phonegapBuild
```
The code in this directory **is debuggable becuase it is not concaenated, compressed or minified**.

## How to release for production
Prodcution enviroment works inside `/bin` directory it is generated for web environment from grunt
``` bash
grunt compile # create the /bin
```

For cordova by
``` bash
grunt phonegapCompile
```
