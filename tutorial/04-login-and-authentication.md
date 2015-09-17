# Add Authentication

## Overview
Image if the Dashboard that we defined before were a protected area where the user can access only after a successful login.
To support this feature, we'll use [Satellizer](https://github.com/sahat/satellizer) a service to manage authentication,  [crSession](https://github.com/ngutils/cr-session) to store user session, and [crAcl](https://github.com/ngutils/cr-acl) to check the role of the users.
We're going to create a Signin module to let authorized users to log into the reserved dashboard.




## Acl
[crAcl](https://github.com/gianarb/cr-acl) is a module available in ngStartup that lets you to define roles and visibility in your application.
By default is supports 2 (expandables) roles:

* ROLE_GUEST for users not authenticated
* ROLE_USER for users authenticated

The first step is change the configuration in `src/app/app.js` writing the state for unauthorized roles that try to access to reserved area:

```javascript
.run(['$rootScope', 'crAcl', 'crSession', 'crRemoteHttp', 'crIdentity', '$state', '$log',
function run($rootScope, crAcl, crSession, crRemoteHttp, crIdentity, $state, $log) {

    //set default login state for unauth users
    crAcl.setRedirect("signin");

    //what append on user successful login
    $rootScope.$on('auth:identity:success', function(event, data) {
      $state.go("dashboard");
    });

    //...

}]);
```

Two interesting thing on code above:

1. all users with no authorizaton that try to access to a protected area will be redirected to the `signin` state
2. when an event name `auth:identity:success` will be broadcasted (probably after a successful login) the user will be redirected to the `dashboard` state

Now edit the `src/app/dashboard/dashboard.js` file making the dashboard state private. Add ROLE_USER to state definition:


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
        data:{ pageTitle: 'Dashboard', is_granted:['ROLE_USER'] }
    });
})
```

Now if you try to go to the dashboard you'll be redirect to a invalid `signin` state. You have to create it.


## $auth configuration

The next step is create a signin module, but before we have to config our $auth service provided by Satellizer. This module is very easy to use and helps you to create a simple login service with several providers: from Google to Facebook and of course your own authentication service.

As for News resource, the endpoint that we are using serves a login system on the `http://rest.test.corley.it/login` endpoint. Sending to it a POST request with `username` and `password` in the body, we'll obtain a 200 OK response in case of successful authentication, or a 401 UNAUTHORIZED response otherwise. A valid username is `user1` and a valid password `password1`.

So set $auth method in the `src/app/app.js` file:

```javascript
.config(['$urlRouterProvider', '$translateProvider', '$authProvider', 'cfpLoadingBarProvider', 'crRemoteProvider', 'appConf', '$logProvider',
  function myappConf($urlRouterProvider,  $translateProvider, $authProvider, cfpLoadingBarProvider, crRemoteProvider, appConf, $logProvider) {

    // ....

    $authProvider.loginUrl = 'http://rest.test.corley.it/login';
    $authProvider.authToken = 'Basic';

    // ....

}]);
```

In the example above, we are setting the correct endpoint url and the authorization method that will be setted after a successful call to the login endpoint. All $http requests will be firmed with the Basic Auth header.

## Signin Module

Now you can create the signin module and add it to the project. Add a folder with two files inside:
```
.
├── app.js
├── app.spec.js
├── home
├── dashboard
└── signin
    ├── signin.js
    └── signin.tpl.html
```

The `signin.tpl.html` with a simple form:

```html
<div class="row">
  <div class="col-lg-4 col-lg-offset-4 col-sm-6 col-lg-offset-6">
    <div class="well margin_50_0">
      <form ng-submit="doLogin(username, password)">
        <div class="form-group">
          <input type="text" ng-model="username" class="form-control" placeholder="username" required>
        </div>
        <div class="form-group">
          <input type="password" ng-model="password" class="form-control" placeholder="password" required>
        </div>
        <p>
          <button type="submit" class="btn btn-success">Click to sign in</button>
        </p>
        <p>
          <small>
            Use <i>user1</i> and <i>password1</i> to sign in.
          </small>
        </p>
      </form>
    </div>
  </div>
</div>
```

And the `signin.js` with the module and its controllers:

```javascript
angular.module( 'ng-startup.signin', ['ui.router.state'])
.config(function config( $stateProvider ) {
    $stateProvider.state( 'signin', {
        url: '/signin',
        views: {
            "main": {
                controller: 'SigninCtrl',
                templateUrl: 'signin/signin.tpl.html'
            }
        },
        data:{ pageTitle: 'Signin' }
    });
    $stateProvider.state( 'signout', {
        url: '/signout',
        views: {
            "main": {
                controller: 'SignoutCtrl'
            }
        },
        data:{ pageTitle: 'Signout' }
    });

})
/**
 * Signin controller
 */
.controller( 'SigninCtrl', ['$rootScope', '$scope', '$stateParams', '$auth', function SigninController( $rootScope, $scope, $stateParams, $auth ) {
    $scope.doLogin = function(username, password) {
        $auth.login({
          username: username,
          password: password
        }).then(function(response) {
            $rootScope.$broadcast("auth:login:success", {'role': response.data.role, 'provider': 'fakelogin', 'user': response.data.identity});
        });
    };

}])
/**
 * Signout controller
 */
.controller( 'SignoutCtrl', ['$rootScope', '$scope', '$state', '$auth', function SigninController( $rootScope, $scope, $state, $auth ) {
    $auth.logout().then(function() {
        $rootScope.$broadcast("auth:logout:success");
    });

}]);
```

There're two new states:

`Signin` that shows a signin form and, in case of successful $auth request, broadcasts the `auth:login:success` event with some data:  a role got by endpoint (in this case ROLE_USER), a provider name, and user's data that will be stored into session. This event will be catched by crIdentity, a module available by default in ngStartup, that will use crSession to store user data and will set the role in crAcl, then will broadcasts the `auth:identity:success`. This event is catched by the $rootScope in `src/app/app.js` that redirects the user to the dashboard state.

`Signout` that has not a template but just broadcasts the `auth:logout:success`: crIdentity will catch it, destroying user's session and role. Then it will broadcasts the `identity:purge:success` that you can catch in the app.js adding your logic:


```javascript
.run(['$rootScope', 'crAcl', 'crSession', 'crRemoteHttp', 'crIdentity', '$state', '$log',
function run($rootScope, crAcl, crSession, crRemoteHttp, crIdentity, $state, $log) {

    // ...

    //what append on user logout
    $rootScope.$on("auth:purge:success", function(event, data){
      $state.go("home");
    });

    // ...

}])
```

Remember that you **have to add the ng-startup.signin module to the app** in `src/app/app.js` file.

Now your login system is completed, try it!
