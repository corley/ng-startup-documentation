# Graphics and layout

## Overview

ngStartup helps you to create modern and responsive web and mobile apps. To do that, it includes some external libraries and tools, from Bootstrap to Font Awesome, from stunning Loading bars to scripting tools such as Less and Saas. Configure and use what's the best for you.

## Bootstrap and Font Awesome
ngStartup uses [Bootstrap 3.x](http://getbootstrap.com/) and [Font Awesome 4.x](https://fortawesome.github.io/Font-Awesome/) to let you to create high quality and responsive web applications. Of course, if you prefer, you can remove this dependencies from `bower.json` and you app.

### Bootstrap
Thanks to Bootstrap, you have a lot of different html, css and js components and a powerful and responsive grid system. You can find all documentation about it on [Bootstrap webiste](http://getbootstrap.com/).

However, as you know, javascript plugins of Bootstrap are written with jQuery. Thanks to another dependency injected in the app (`UI Bootstrap`) you can use Bootstrap's plugins directly with Angular.

Here an example of the usage of programmable alerts:

``` html
<div ng-controller="AlertDemoCtrl">
  <alert ng-repeat="alert in alerts" type="{{alert.type}}" close="closeAlert($index)">{{alert.msg}}</alert>
  <button type="button" class='btn btn-default' ng-click="addAlert()">Add Alert</button>
</div>
```

``` javascript
.controller('AlertDemoCtrl', ['$scope', function ($scope) {
  $scope.alerts = [
    { type: 'danger', msg: 'Oh snap! Change a few things up and try submitting again.' },
    { type: 'success', msg: 'Well done! You successfully read this important alert message.' }
  ];

  $scope.addAlert = function() {
    $scope.alerts.push({msg: 'Another alert!'});
  };

  $scope.closeAlert = function(index) {
    $scope.alerts.splice(index, 1);
  };
}]);
```

Please refer to official [documentation](http://angular-ui.github.io/bootstrap/) of UI Bootstrap.


### Font Awesome
Thanks to Font Awesome you have over 400 scalable vector icons ready to be used in your application:

``` html
<i class="fa fa-plus"></i>
```

Please refer to official [documentation](https://fortawesome.github.io/Font-Awesome/) of Font Awesome for the full list of icons.


## Sass/Less supports
ngStartup supports fantastic tools for scripteable css such as Less and SaaS. These tools help you to manage better your styles and quickly changes colors, dimensions, font and box values along the whole application.

Bootstrap supports both Less and SaaS, so choose the best for you when start a new project. **By default, ngStartup uses Less compiler to create .css files**.

### /src/less/base.less (and /src/saas/base.scss)
This file contains basic styles definitions and some helper classes. What the chapter `Helper classes and CSS tricks` below for more info. We suggest to don't edit this file, but of course you can do that fitting your needs.

### /src/less/variables.less (and /src/saas/variables.scss)
Edit this file to change Bootstrap (or others) variables to fit your design needs. For example, if you want change the "red" of "danger" buttons an texts in Bootstrap, you have to write:

``` css
@brand-danger: #ff0000;
```

If you're working on an application that could have different "design versions" (for example a network of similar websites with different fonts and color) we suggest to set here you variables.


### /src/less/main.less (and /src/saas/main.scss)
Edit this file adding your styles and classes. It works with `base` and `variables` files and will be compiled with them. This is the best place to store the majority of your css rules and settings.

### Modules .less and .scss files
Remember that all `.less` and `.scss` files in `/src/app/` and `/src/common/` files are added to the list of files that will be compiled during development and production.
If you had added a file to a module, you don't need to specify where is it. Grunt will compile it.

### External .less and .scss files
Instead, if you have to add a `.less` or `.scss` file from an external dependency, for example from **bower_components**,  then **you have to add it to `/build.config.js` in project root.

It contains all lists of files used by the application to produce the definitive app ready for testing and production. Add your own file to the list:



### Switching Less and SaaS
Edit the head of `/config/build.config.js` changin the key `css_parse` from `less` to `sass`.

## Css Tricks

The `/src/less/base.less` file adds some css classes and tricks.

### Margin and padding classes.
Several classes are available with this sytax:

``` css
.margin-5       // 5px margin on all sides
.margin-5-0-0-0 // 5px margin only on top, 0 on other sides
.margin-x-5-x-x // 5px margin on the right, other sides inherited
// etc etc
```

All these classes are from 5 to 200 and available **also for padding**, for example `padding-x-x-x-75 (75px padding on the left).

### Other tricks
Following are also available:

``` css
.no-cursor     // disable curson on a, button or other element
.text-white   // color #fff
.bg-white     // background-color #fff
.text-black   // color #000
.bg-black     // background-color #000
```

There are also some rules on button and a element to prevent outline and focus border on mobile devices.

## Loading Bar
ngStartup integrates the fantastic module `cfpLoadingBar`, an angularJS service that lets you to show a css (and fully stlable) loading bar on the top of the page and/or a spinner in the center of the page during routing.

It's activeted when:

* during $http calls (also nested from the first to the last)
* manually (attachable to the route changes)

### How to configure and use it
If you want use only the spinner or the loading bar, edit your  `/src/app/app.js` configuration:

``` javascript
.config(['cfpLoadingBarProvider', function(cfpLoadingBarProvider) {
  cfpLoadingBarProvider.includeSpinner = false; // to disable spinner
  cfpLoadingBarProvider.includeBar = false;     // to disable loading bar
}])
```

We suggest to activate the loading bar during the route changes. Always in the `/src/app/app.js` in the App controller (the main controller that works on <html> tag):

``` javascript
.controller('AppCtrl', ['$scope','$rootScope', '$location', '$state', 'cfpLoadingBar',
  function AppCtrl($scope, $rootScope, $location, $state, cfpLoadingBar) {
    $scope.$on('$stateChangeSuccess', function(event, toState, toParams, fromState, fromParams) {
        cfpLoadingBar.complete();
    });
    $scope.$on('$stateChangeStart', function(event, toState, toParams, fromState, fromParams) {
        cfpLoadingBar.start();
    });
}]);
```

### Styles
The **spinner**  and **bar** styles are defined in the loading-bar css, but you can easily rewrite them in your main.css.

``` css
#loading-bar .bar {
  -webkit-transition: width 350ms;
  -moz-transition: width 350ms;
  -o-transition: width 350ms;
  transition: width 350ms;

  background: #29d;
  position: fixed;
  z-index: 10002;
  top: 0;
  left: 0;
  width: 100%;
  height: 2px;
  border-bottom-right-radius: 1px;
  border-top-right-radius: 1px;
}

#loading-bar .peg {
  position: absolute;
  width: 70px;
  right: 0;
  top: 0;
  height: 2px;
  opacity: .45;
  -moz-box-shadow: #29d 1px 0 6px 1px;
  -ms-box-shadow: #29d 1px 0 6px 1px;
  -webkit-box-shadow: #29d 1px 0 6px 1px;
  box-shadow: #29d 1px 0 6px 1px;
  -moz-border-radius: 100%;
  -webkit-border-radius: 100%;
  border-radius: 100%;
}
#loading-bar-spinner {
    display: block;
    left: 46%;
    position: fixed;
    top: 33%;
    z-index: 10002;
    padding: 10px;
    border-radius: 3px;
    background: rgba(0,0,0,0.5);
    color:#fff;
}
#loading-bar-spinner .spinner-icon {
  color:#fff;
  border-top-color:  #fff;
  border-left-color: #fff;
  width:20px;
  height:20px;
}
```

### Spinner directive
Thanks to the `crLoading` module that works with the `angular-loading-bar` it's available a directive that you can use on button and form. That directive:

* if used on a button, disable it after user click and show a spinner inside the button (for at least 300ms and until the $http request ends)
* if used on a form, disable submit buttons after the submit action showing the spinner as for the button above



It's very easy to use:

``` html
<!-- button example -->
<button class="btn" cr-loading>Login</button>

<!-- form example -->
<form cr-loading>
  <input type="text" >
  <button class="btn" type="submit">Search</button>
</form>
```

You can edit the class of the <span> element that will be injected into the button (by default uses a spinner by Font Awesome)

``` javascript
.config(["crLoadingProvider", function(crLoadingProvider){
  crLoadingProvider.setClickSpinnerClass("my custom class");
});
```
Please read official [documentation](https://github.com/ngutils/cr-loading) about crLoading usage.

## Animations
ngStartup defines in `/src/less/base.less` some classes that you can use in your views to activate animations during route changes and ng-repeat, ng-show and ng-hide events.

### Example
For **fading** animations:

``` html
<!-- in your views -->
<p ng-repeat="article in articles" class="fading-item">{{ article.title }}</p>

<!-- in /src/index.html -->
<div ui-view="main" class="fading-view" ></div>
```

For **slide-out** animations:

``` html
<!-- in your views -->
<p ng-repeat="article in articles" class="sliding-item">{{ article.title }}</p>

<!-- in /src/index.html -->
<div ui-view="main" class="sliding-view" ></div>
```

### Add yours:
This is an example of code for new animation that you can add everywhere in you less/sass files.

``` css
/** fading animation, just add fading-item to ng-repeat and ng-show items */
.fading-item.ng-enter, .fading-item.ng-move {
  -webkit-transition:0.5s linear all;
  -moz-transition:0.5s linear all;
  -o-transition:0.5s linear all;
  transition:0.5s linear all;
  opacity:0;
}


.fading-item.ng-enter.ng-enter-active,
.fading-item.ng-move.ng-move-active {
  opacity:1;
}


.fading-item.ng-leave {
  -webkit-animation:0.5s fading_animation;
  -moz-animation:0.5s fading_animation;
  -o-animation:0.5s fading_animation;
  animation:0.5s fading_animation;
}

@keyframes fading_animation {
  from { opacity:1; }
  to { opacity:0; }
}

@-webkit-keyframes fading_animation {
  from { opacity:1; }
  to { opacity:0; }
}

@-moz-keyframes fading_animation {
  from { opacity:1; }
  to { opacity:0; }
}

@-o-keyframes fading_animation {
  from { opacity:1; }
  to { opacity:0; }
}


/** view animation when route changes (fading animation, to activate add fading-view to ui-vide div) */
[ui-view].ng-enter.fading-view {
  -webkit-transition:0.2s linear all;
  -moz-transition:0.2s linear all;
  -o-transition:0.2s linear all;
  transition:0.2s linear all;
  opacity:0;
}

[ui-view].ng-enter.fading-view {
  opacity: 0;
}

[ui-view].ng-enter-active.fading-view {
  opacity: 1;
}

[ui-view].ng-leave.fading-view {
  opacity: 0;
}

[ui-view].ng-leave-active.fading-view {
  opacity: 0;
}
```




## Assets/Fonts/Images


![assets](http://ngstartup.corleycloud.com/assets/images/ngstartup08.png)

Fonts, images, videos and other assets can be placed in `/src/assets/` folder.
This is the structure:

```
.
└── src
    └── assets
        ├── images            // images
        ├── videos            // videos and media
        ├── fonts             // ttf, eot and other fonts
        └── phonegap          // icons and splash screen for cordova/phonegap

```

Assets are automatically moved by Grunt to `dist` folders during building and compiling.
