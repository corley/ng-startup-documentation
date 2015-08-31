# ngStartup

## Introduction

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
``` bash
git clone git@github.com:corley/ng-startup.git
cd ng-startup
npm install
```
Npm will install nodejs by package.json then bower dependencies (we'll talk about that later.)
The installation is completed.

## Getting Started
After installation run:
``` bash
grunt watch:web
```
ngStartup (thanks to Grunt) compiles your app creating a development version that you can visit on `http://127.0.0.1:8081`.

### What happens?
The flow is very simple:

1. your code is under `/src`. Here you find the index.html and your angular files
2. grunt prepares the folder `/dist/build/web`
3. grunt parse javascript files, views, assets and less/sas under `/src` and compile & move them to `/dist/build/web`
4. grunt copy bower dependecies (js and css) and pastes them in `/dist/build/web`
5. grunt run a local server that points to `/dist/build/web` so you can visit the app to `http://127.0.0.1:8081`

> Remember that you work under `/src` and your app (compiled in development stage) is under `/dist/build/web`

### The Single Page Application

As single page application, you have just a html file. The `/src/index.html` it's similar to this one:
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

When you are in development phase, edit files in  `/src` and run one of these commands:

``` bash
grunt build:web # it creates the web version in /dist/build/web (not minified or compressed) and run tests
grunt build:mobile # it creates the phonegap version in /dist/build/mobile (not min. or compressed) and run test
# or
grunt watch:web # it creates the /dist/build/web, starts the local server and watches changes in /src re-building dist at any change, useful while coding
grunt watch:mobile # the same of above but for mobile version
```

The app created in '/dist' **is debuggable because it is not concaenated, compressed or minified**.

## How to release for production
When you're ready, you can create the minified and compressed version
``` bash
grunt compile:web # it compiles in `/dist/compile/web`
grunt compile:mobile # it compiles in `/dist/compile/mobile`
```

Now you can try in on `http://127.0.01:8081` or deploy the code to your server.
