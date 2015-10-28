# Working with REST

## Overview
In the real life probably we would consume remote resources instead of a list of content defined in the code. The most of angular apps work with API or other remote services.
In this tutorial we will use [crRemote](https://github.com/ngutils/cr-remote) (based on $http) to work with RESTful endpoints.

## Configuring Endpoint
crRemote is injected as dependency in the ngStartup app so it's immediatly available. The first step is set the endpoint. For this tutorial, the `http://rest.test.corley.it/news` url returns a list of news as defined in the code of previous tutorial.
A brutal way is using the `crRemoteProvider` in the  `src/app/app.js` file:
```javascript
.config(['$urlRouterProvider', '$translateProvider', '$authProvider', 'cfpLoadingBarProvider', 'crRemoteProvider', 'appConf', '$logProvider',
  function myappConf($urlRouterProvider,  $translateProvider, $authProvider, cfpLoadingBarProvider, crRemoteProvider, appConf, $logProvider) {

    //... other code

    //set api andpoint by app config
    crRemoteProvider.addEndpoint("default", "http://rest.test.corley.it/");

    //... other code

 }])
```

The best way is using the app configuration. Edit the `/src/common/conf/conf.js` file adding your endpoint in the `appConf` constant.
``` javascript

angular.module('ng-startup.appConf', [])
// env: build
.constant('appConf', {
  "endPoint": "http://rest.test.corley.it/",
  "defaultLanguage": "en_EN",
  "debug": true
})
// endenv
// env: compile
.constant('appConf', {
  "endPoint": "http://rest.test.corley.it/",
  "defaultLanguage": "en_EN",
  "debug": false
})
// endenv
;
```

As you can see, the constant is defined twice, because the `devcode` comments will be parsed by Grunt compiling a different configuration for build (under development) and compile (release for production) stages. Edit the endpoint as above, then edit the `/src/app/app.js` file:

```javascript
.config(['$urlRouterProvider', '$translateProvider', '$authProvider', 'cfpLoadingBarProvider', 'crRemoteProvider', 'appConf', '$logProvider',
  function myappConf($urlRouterProvider,  $translateProvider, $authProvider, cfpLoadingBarProvider, crRemoteProvider, appConf, $logProvider) {

    //... other code

    //set api andpoint by app config
    crRemoteProvider.addEndpoint("default", appConf.endPoint);

    //... other code

 }])
```


## Creating new services
Now all `crRemote` services will work with the *default* endpoint defined in configuration. We can create a service to work with remote news. Add it to your `/src/app/dashboard/dashboard.js` file or whenever you want inside your project:

``` javascript
.service('NewsRest', ['crRemoteHttp', function(crRemoteHttp){
    var service = crRemoteHttp.createService("news");
    return service;
}])
```

This snippet creates a service that works with remote resource *news* on url *enpoint + resource name = "http://rest.test.corley.it/news"*. The methods inherited by crRemote are:

* .get() (HTTP GET /news) or .get({'id': 10}) (HTTP GET /news/10)
* .post({'id': 10, {'data': {'title': 'a', 'summary': 'b'}}) (HTTP POST /news/10)
* .delete({'id': 10}) (HTTP DELETE /news/10)
* .patch({'id': 10, {'data': {'title': 'a', 'summary': 'b'}}) (HTTP PATCH /news/10)

Knowing these methods (each of them returns a promise) you can change the DashboardCtrl in `/src/app/dashboard/dashboard.js` removing the old list and using the new service:

```javascript
/**
 * Dashboard controller
 */
.controller( 'DashboardCtrl', ['$scope', '$stateParams', function DashboardCtrl( $scope, $stateParams, NewsRest) {
    NewsRest.get().then(function(res) {
        $scope.results = res.data;
    });
}]);
```

Refresh your browser and watch the news listed in your dashboard!
