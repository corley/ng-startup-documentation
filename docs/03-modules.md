# Modules

## Introduction

In ngStartup you can easily add your code into your application. The structure & foldering help you to easily define **independent e reusable modules**.

![module](http://ngstartup.corleycloud.com/assets/images/ngstartup03.png)


## Module structure

A module is a set of:

* the logic (js files with your services and controllers)
* views (.tpl.html files)
* styles (.less or .scss files that will be processed by Less or Sass)
* tests (spec.js with your test definitions)

For example, to create a module that manages controllers and views for the home page, you have just to create a folder into `/src/app`:

```
.
└── src
    └── app
        └── home
            ├── home.js       // controllers and services
            ├── home.tpl.html //  a view for a ctrl defined in shopping.js
            ├── page.tpl.html //  another view for a ctrl defined in shopping.js
            ├── home.less     //  styles for this module
            └── home.spec.js  //  tests for this module
```


## How to create a module

When you add a new module, probably you would to define controllers, filters and services related to it.
About **controllers** you can define the routing rules inside the module. In this way your module will be easily injeactable in your app without editing external files (exp the app configuration).

``` javascript
angular.module( 'ng-startup.home', ['ui.router.state'])
.config(function config( $stateProvider ) {
  $stateProvider.state( 'home', {
    url: '/home',
    views: {
      "main": {
        controller: 'HomeCtrl',
        templateUrl: 'home/home.tpl.html'
      }
    },
    data:{ pageTitle: 'Home' }
  });
  $stateProvider.state( 'page', {
    url: '/page/:pageId',
    views: {
      "main": {
        controller: 'PageCtrl',
        templateUrl: 'home/page.tpl.html'
      }
    },
    data:{ pageTitle: 'Info'}
  });

})
/**
 * Home controller
 */
.controller( 'HomeCtrl', ['$scope', '$stateParams', function HomeController( $scope, $stateParams ) {
  //do something
}])
/**
 * Page controller
 */
.controller( 'PageCtrl', ['$scope', '$stateParams', function PageController( $scope, $stateParams ) {
  $scope.pageId = $stateParams.pageId;
  //do something
}])
;
```

**Remember** to add the new module as dependency of ngStartup in `/src/app/app.js`

``` javascript
angular.module(
  'ng-startup',
  [
    'ng-startup.home',
    // other dependencies
```

### Routing definition
In the example above we define in the `.config` the ruls for the routing. In this way all calls to `/home` will be managed by the HomeCtrl with the view homte.tpl.html, and all urls like `/page/about-us` will be managed by PageCtrl with the vide page.tpl.html.


### Layout views
The `main` key is related to the directive `ui-view` in `/src/index.html`:

``` html
<body>
    <div class="container">
        <div ui-view="main" class="fading-view" ></div>
    </div>
</body>
```

To use a double-column layout, you have to do something like that:

``` html
<body>
  <div class="container">
    <div class="row">
      <div ui-view="lef"  class="col-lg-3" ></div>
      <div ui-view="main" class="col-lg-9 fading-view" ></div>
  </div>
</body>
```

and in your modules something like this:

``` javascript
//...
.config(function config( $stateProvider ) {
  $stateProvider.state( 'home', {
    url: '/home',
    views: {
      "main": {
        controller: 'HomeCtrl',
        templateUrl: 'home/home.tpl.html'
      },
      "left": {
        controller: 'LeftCtrl',
        templateUrl: 'left/left.tpl.html'
      }
    },
    data:{ pageTitle: 'Home' }
  });
})
```

## Views and layouts

### Views

As wrote above, you can add several views that will be used by controllers and directives of its module (or by external modules if you prefer). A view is just an html template with own $scope:

``` html
<div>
    <p>Hello {{ name }}</p>
</div>
```

### Less & Sass

When the app is build (both for development or production stages) all `.less` and `.scss` will be compiled to product the `.css` the application.
In `/src/app/less/` and `/src/app/sass/` folders you can find css scripts for your application. But if you want add specific styles for a module, just add less and scss files into the module folder.

A less example:

``` css
@home-color: #000;
.home-page {
  background: #fff;
  color: @home-color;
}
```

## Tests

To write tests for your module, create `*.spec.js` into module folder.
**All tests defined will be launched at build and compile tasks**, so you'll be sure that your application is ready to be released.

An example of stupid test:

``` javascript
describe( 'home section', function() {
beforeEach( module( 'ng-startup.home' ) );
  it( 'should have a dummy test', inject( function() {
    expect( true ).toBeTruthy();
  }));
});
```
