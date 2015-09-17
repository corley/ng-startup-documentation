# Creating a module

## Overview

The first step is the creation of a new AngularJS module, then inject it into our app. If you watch at `src/app/index.html` and `src/app/app.js`, you can easily understand that the `ng-startup` app run on all the html with the major controller `AppCtrl`.

All modules defined in folder under `src/app/` can be added to the ng-startup application. You can see an example in the `/src/app/home`, thats is the module that manage the home page of our project.

## Adding a new module

The goal now is create a new module called **Dashboard**, so create the new folder `src/app/dashboard`.

When you add a new module, take in mind that all logic and views must be added inside the folder (in this case *dashboard*).
Create two files:
1. dashboard.js that contains the module definition and the logic
2. dashboard.tpl.html that's the template for the new module.

Your folders should be like this one:

```
.
├── app.js
├── app.spec.js
├── dashboard
│   ├── dashboard.js
│   └── dashboard.tpl.html
└── home
    ├── home.js
    ├── home.less
    ├── home.spec.js
    ├── home.tpl.html
    └── page.tpl.html
```

Now we can create the module and its controller:

```javascript
angular.module( 'ng-startup.dashboard', ['ui.router.state', 'cr.remote'])
.config(function config( $stateProvider ) {
    $stateProvider.state( 'dashboard', {
        url: '/dashboard',
        views: {
            "main": {
                controller: 'DashboardCtrl',
                templateUrl: 'dashboard/dashboard.tpl.html'
            }
        },
        data:{ pageTitle: 'Dashboard'}
    });
})
/**
 * Dashboard controller
 */
.controller( 'DashboardCtrl', ['$scope', '$stateParams', function DashboardCtrl( $scope, $stateParams) {
    $scope.results = [
        {
            "id": 968849,
            "title": "Sunt neque aliquam sint non voluptate amet reprehenderit asperiores delectus recusandae.",
            "summary": "Minus sint delectus quo ipsam tenetur et corrupti explicabo porro itaque reprehenderit est debitis officiis id eaque aperiam laudantium rerum aperiam tenetur voluptatem sint doloribus et suscipit sunt eum provident harum iusto.",
            "timestamp": 1441273351000,
            "author": "user20"
        },
        {
            "id": 258238,
            "title": "Tenetur pariatur quis odit qui aliquid in.",
            "summary": "Iste temporibus dignissimos et ratione quod placeat dolorum fugiat et omnis perferendis modi dolor et officiis nam delectus perferendis vitae debitis accusamus dolorem ad totam.",
            "timestamp": 1440754951000,
            "author": "user9"
        },
        {
            "id": 717454,
            "title": "Eveniet est at hic non optio enim.",
            "summary": "Numquam possimus consequatur ullam amet ex enim assumenda molestiae natus placeat sit sunt ex aspernatur ad numquam saepe deleniti repellendus distinctio minima facilis nihil.",
            "timestamp": 1440668551000,
            "author": "user11"
        },
        {
            "id": 537380,
            "title": "In sit doloremque illo.",
            "summary": "Aut eveniet dolores saepe quas sequi quis minus ut fugit dolores ullam dolor qui reiciendis eveniet nostrum exercitationem ea consequatur magnam tempora quis in veniam nihil in provident totam a exercitationem eos quam possimus placeat consequatur quo non non ab ullam.",
            "timestamp": 1440582151000,
            "author": "user1"
        }
    ];
}]);
```

The logic of our first module it's very simple:
* we have a new module called `ng-startup.dashboard`
* it defines in the config a new route strategy: when you visit the #/dashboard
  * it run the `DashBoard` controller
  * it uses the `dashboard/dashboard.tpl.html` as template
  * it will inject the compiled view in the `<div ui-view='main'></div>` element in our index.html file
* in the controller there's a fake service that set a list of news

In order to make this module active, add it the main module in `src/app/app.js`:
```javascript
angular.module('templates-app', []);
angular.module('templates-common', []);
angular.module(
    'ng-startup',
    [
        "...",
        'templates-app',
        'templates-common',
        'ui.bootstrap',
        'ng-startup.dashboard', // add this line
        'ui.router',
        "..."
    ]
)
```

Now you can add a template to show the news provided by the controller. Open the `src/app/dashboard/dashboard.tpl.html` file and add this code (or your own):

```html
<div class="well">
  <p>
    <button class="btn" ng-class="{'btn-success': (order == 'timestamp')}" ng-init="order = 'timestamp'" ng-click="order = 'timestamp'">Order by date</button>
    <button class="btn"  ng-class="{'btn-success': (order == 'title')}" ng-click="order = 'title'">Order by title</button>
  </p>
</div>

<div class="row">
  <div class="col-md-12" ng-repeat="news in results | orderBy : order">
    <h2>{{ news.title }}</h2>
    <p>{{ news.summary }}</p>
    <p class="text-muted">
    published on {{ news.timestamp | date }}
    </p>
  </div>
</div>
```

We are ready to visit our first controller! Grunt is wathing changes in `src/` compiling the result in the `dist/build/web`, so visit [http://localhost:8081/#/dashboard](http://localhost:8081/#dashboard) with your browser.
