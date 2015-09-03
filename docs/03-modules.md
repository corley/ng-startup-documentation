# Modules

## Introduction

In ngStartup you can easily add your code into your application. The app structure helps you to easily define **independent e reusable modules**.



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

The following inforpgrahic shows better the structure of a module:

![module](http://ngstartup.corleycloud.com/assets/images/ngstartup03.png)

## How to create a module

You can place your modules in two different folders:

* `/src/app` for main modules with a specific logic and goal (exp: a module to manage news)
* `/src/common` for reusable modules, (exp: layout components, modules rich of directives)

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



### Layout views
## Routing
In the example above we defined in the `.config` the rules for routing. In this way all calls to `/home` will be managed by the HomeCtrl with the view homte.tpl.html, and all urls like `/page/about-us` will be managed by PageCtrl with the vide page.tpl.html. Thanks to [ui-router](http://angular-ui.github.io/ui-router/site/) you can set your routes in a simple way. You have to define a state (such a name for the route) that must be unique in the app. When you create your module, remember that you can set different states with route associated that will join a controller and his view:

``` javascript
//...
.config(function config( $stateProvider ) {

  //will match #/search url
  $stateProvider.state( 'search', {
    url: '/search',
    views: {
      "main": {
        controller: 'SearchHomeCtrl',
        templateUrl: 'search/home.tpl.html'
    },
    data:{ pageTitle: 'Home' }
  });

  //will match url like #/search?q=google
  $stateProvider.state( 'search.results', {
    url: '/search?q',
    views: {
      "main": {
        controller: 'SearchResultsCtrl',
        templateUrl: 'search/results.tpl.html'
    },
    data:{ pageTitle: 'Results' }
  });

  //will match url like #/search/archive/123
  $stateProvider.state( 'search.archive', {
    url: '/search/archive/:id',
    views: {
      "main": {
        controller: 'SearchArchiveCtrl',
        templateUrl: 'search/archive.tpl.html'
    },
    data:{ pageTitle: 'Old search' }
  });
})
```

When you pass params to the controller, you should inject $stateParams and $state to manage data and redirect to states.

``` javascript
.controller('SearchArchiveCtrl', ['$scope', '$state', '$stateParams', function($scope, $state, $stateParams) {

  console.log($state.params.id); // 123
  console.log( $stateParams.id); // 123
    if( $stateParams.id == 100) {
    $state.go('search');
    // redirect to state 'search' (url #/search)
  }
  else {
    $state.go('search.archive', {'id': '100'});
    // redirect to state 'search.archive' with id 100 (url #/search/arcive/100)
  }

})

```


## Views and layouts

### Views

As wrote above, angular will match an url joining the correct controller and view. A view is just an html template with own $scope:

``` html
<div>
    <p>Hello {{ name }}</p>
</div>
```

The result will be compiled in the DOM of the index.html, showing the result on the browser of your visitors.
The `main` key is related to the directive `ui-view` in `/src/index.html`:

``` html
<body>
    <div class="container">
        <div ui-view="main" class="fading-view" ></div>
    </div>
</body>
```

### Multi-views

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

and in your modules:

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
