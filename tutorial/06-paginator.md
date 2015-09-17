# Custom paginator

## Overview
Watch at news listed: they are a lot! We're going to add a paginator that works with remote resouce. The `http://rest.test.corley.it/news` endpoint retuns all news (approx 200), but if you add the query param *page*, it will return only 10 news: `https://rest.test.corley.it/news?page=1`. The body response is always a list of news (but less then before) and in the headers response there are useful infos about the number of items returned:

* x-count: the number of items returned, 10 by default
* x-total: the number of total items, 200 by default
* x-page: the page number you required, for example 3
* x-from: the first item returned, for example 21
* x-to: the last item returned, for example 30
* x-next: the number of the next page (or 0 if the page required is the last one)
* x-prev: the number of the previous page (or 0 if the page required is the first)

The logic of this headers can change on different remote services. The goal is create a service that can adapt to the response in order to manage properly the pagination.

## Build the service
The paginator is a reusable module. So add a new folder to `src/common` with this files:

```
.
└── paginator
    ├── paginator.js
    └── paginator.tpl.html
```

The service inside `paginator.js` must contain two services: a directive that will show the paginator using the `paginator.tpl.html` and a function injected to crRemote as interceptor. In this way when your `NewsRest` service (created in previous tutorials) will make a call, the interceptor function will transform the data managing the pagination. This is the code:

``` javascript
angular.module('ng-startup.paginator', [
    "cr.remote"
])
.config(['crRemoteProvider', function paginatorConf(crRemoteProvider) {
  crRemoteProvider.addResponseInterceptorSuccess("paginator", function(data){
      var limit = 10;
      var headers = data.headers();
      var pager = false;
      if(headers && headers['x-count']) {
        pager = {
          pages: 0,
          page: 1,
          last: false
        };
        pager.count = parseInt(headers['x-count']);
        pager.total = parseInt(headers['x-total']);
        pager.from = parseInt(headers['x-from']);
        pager.params = data.config.params;
        pager.to = parseInt(headers['x-to']);
        pager.page = parseInt(headers['x-page']);
        pager.next = parseInt(headers['x-next']);
        pager.prev = parseInt(headers['x-prev']);
      }
      return {
        "data": data.data,
        "headers": data.headers(),
        "status": data.status,
        "pager": pager
      };
    });
}])
;


```
As you can see, the interceptor function read the response headers and return the original data and a new object named `pager` with pagination fields inside.

The directive will use this data:


``` javascript

.directive('paginator', ['$state', function($state){
    return {
        restrict: "E",
        templateUrl: 'paginator/paginator.tpl.html',
        replace: true,
        scope: {
          state: "=",
          stateParams: "=",
          pageInfo: "="
        },
        link: function (scope) {
            scope.$watch('headers', function (headers) {
              scope.go = function(page) {
                if(!scope.stateParams) {
                  scope.stateParams = {};
                }
                scope.stateParams.page = page;
                scope.pageInfo.page = page;
                $state.go(scope.state, scope.stateParams);
              };
            });
        }
    };
}]);

```

Now define the pagination template:

``` html
<nav>
  <ul class="pagination">
    <li class=""><a href="#">{{ pageInfo.total }} trovati</a></li>
    <li class="" ng-show="pageInfo.prev"><a ng-click="go(pageInfo.prev)">indietro</a></li>
    <li class="active"><a >pagina {{ pageInfo.page }}</a></li>
    <li class="" ng-show="pageInfo.next"><a ng-click="go(pageInfo.next)">avanti</a></li>
  </ul>
</nav>

```

## Use it

Go to `src/app/dashboard` folder. Here you have to change something to start usinn the new paginator.

Add the new interceptor to NewsRest:

``` javascript
.service('NewsRest', ['crRemoteHttp', function(crRemoteHttp){
  var service = crRemoteHttp.createService("news", {
    responseInterceptorSuccess: 'paginator',
    auth: true
  });
  return service;
})
```

Then change the Dashboard Controller and its state in order to add a page parameter in url.

``` javascript
angular.module( 'ng-startup.dashboard', ['ui.router.state', 'cr.remote'])
.config(function config( $stateProvider ) {
  $stateProvider.state( 'dashboard', {
    url: '/dashboard?page',
    views: {
      "main": {
        controller: 'DashboardCtrl',
        templateUrl: 'dashboard/dashboard.tpl.html'
      }
    },
    data:{ pageTitle: 'Dashboard', is_granted:['ROLE_USER'] }
  });

})
/**
 * Dashboard controller, reserved by Acl
 */
.controller( 'DashboardCtrl', ['$scope', '$stateParams', 'NewsRest', function DashboardCtrl( $scope, $stateParams, NewsRest ) {
  if ($stateParams.page === undefined) {
    $stateParams.page = 1;
  }

  NewsRest.get({params: {"page": $stateParams.page}}).then(function(res) {
    $scope.results = res.data;
    $scope.pager = res.pager;
  });

  $scope.state = "dashboard";

}])
```

As you can see, the new route accepts a page number (`/dashboard?page=3`) and this value is passed to the controller and used by the NewsRest service to make the GET call.
You should see less news in your browser, just 10 instead of 200. Call the paginator directive in your dashboard template to complete this tutorial:

``` html
<div class="row">
  <div class="col-md-12" ng-repeat="news in results | orderBy : order">
    <h2>{{ news.title }}</h2>
    <p>{{ news.summary }}</p>
    <p class="text-muted">
    published on {{ news.timestamp | date }}
    </p>
  </div>
</div>

<paginator ng-if="pager" page-info="pager" state="state" state-params="stateParams"></paginator>
```

Plase note that the directive accepts state and params in order to call the right route when user clicks on previous and next page links.
