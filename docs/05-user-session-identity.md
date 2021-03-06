# Sessions, ACL and identity

## Overview

ngStartup includes [crAcl](https://github.com/ngutils/cr-acl), [crSession](https://github.com/ngutils/cr-session) and [crIdentity](https://github.com/ngutils/cr-identity) from [ngUtils](https://github.com/ngutils) to let you to easily manage identities, authentication and sessions along the whole app.

This modules let you to

* define roles and visibility for each route
* manage and store data into session (localStorage and cookie)
* manage user's identity (guest, logged users...) along the whole app and restore it after a refresh or new tab
* make login with different auth providers (OAuth, OAuth2, Basic Auth, Facebook, Google, Linkeding, GitHub, Twitter and others)

> These modules help you to manage the user's data and flow along the app.

## Session

To set and get data into session, inject `crSession` in your controllers and services.

``` javascript
.controller('MyCtrl', ['$scope', 'crSession', function MyCtrl($scope, crSession) {
  crSession.set("test", "content")
  console.log(crSession.get("test")); // content
  crSession.delete("test");
}])
```

Session data are stored into "default" namespace. You can define and use seprated namespaces. If you want to store product into a shopping cart, you can use a dedicated namspace.

``` javascript
.controller('MyCtrl', ['$scope', 'crSession', function MyCtrl($scope, crSession) {
  crSession.set("products", [a, b], "shopping");
  crSession.set("total", 122, "shopping");
  console.log(crSession.get("products", "shopping")); // [a, b]
  console.log(crSession.get("total", "shopping")); // 122
  crSession.purgeNamespace("namespace-new");
}])
```

You can destroy the whole session with purge method:

``` javascript
.controller('MyCtrl', ['$scope', 'crSession', function MyCtrl($scope, crSession) {
  crSession.purge();
}])
```

Please read official [documentation](https://github.com/ngutils/cr-session) for crSession.

## Access Control List
crAcl lets you to define roles and rules around you routes. It works with events triggered by $state during route changes.

If the ACL role is not respected, the client is redirected to a customizable url.

### Default roles
By default `ROLE_USER` (for autheticated users) and `ROLE_GUEST` (for guest people) are available. If you want to restrict the access to a route to just authorized users, define the state in this way:

``` javascript
.config(function config($stateProvider ) {
  $stateProvider.state( 'reserved-area', {
        url: '/reserved',
        views: {
            "main": {
                controller: 'ReservedCtrl',
                templateUrl: 'reserved/dashboard.tpl.html'
            }
        },
        data:{
          is_granted: ["ROLE_USER"]
       }
    });
})
```

### Set the user's role
When you are ready (for example after a login), set the user role:

``` javascript
crAcl.setRole("ROLE_FREE");
```

From this moment the users will has access to routes restricted by `is_granted: ["ROLE_USER"]`.

Please read official [documentation](https://github.com/ngutils/cr-acl) for crAcl.


### Restrict html elements to a role
A directive is available to restrict the visibility of DOM elements to a specific role:

``` html
<div cr-granted="ROLE_USER">If you are GUEST you cannot see this div</div>
```

## Identity
The identity is **who-you-are** in the app along the whole life of your session.
crIdentity works with crAcl and crSession to let you to

* set the user role
* change this role after a successful login action (with $auth and any other service)
* maintain alive this role and user's data until logout (also after page refresh or stand-by)
* destroy the user's session after logout

### How to start the user's identity
After a successful login action, for example after a specific $http call to your authentication endpoint, you have to trigger a specific event:

``` javascript
.controller('MyCtrl', ['$scope', '$http', function MyCtrl($scope, $http) {
  //make a call to an auth service
  $http.post('/login-endpoint', {'username': 'bruce', 'password': 'batman'}).then(function(response)
    // trigger an event
    $scope.$broadcast('auth:login:success', {
      'role': 'ROLE_USER',
      'identity': {
        'username': 'bruce',
        'age': response.data.age
      },
      'provider': 'my-auth-provider'
    });
  );
}])
```

This event is catched by crIdentity that:

* it will assign ROLE_USER to the user
* it will store the the data into the session
* it will assign the data to $rootScope._identity object in order to let you get easily this data in the scopes
* it will broadcast the `auth:identity:success` event letting you to run your logic after the login

So in your `/src/app/app.js` you can manage the identity in this way:

``` javascript
.run(['$rootScope', 'crIdentity', function run($rootScope, crIdentity) {
  $rootScope.$on('auth:identity:success', function(event, data) {
    // your login after a login, for example a redirection
  });  

  crIdentity.restore().then(function(identity) {
    // your login after identity restore (after page refresh or when a new tab is openend)
  });
});
```

### Destroy the user's identity
In order to destroy the identity voiding the role and data stored by the session, trigger a specific event:

``` javascript
.controller('MyCtrl', ['$scope', '$http', function MyCtrl($scope, $http) {
  $scope.$broadcast('auth:logout:success');
}])
```

crIdentity will void the identity and will trigger the 'auth:purge:success' event letting you to add your logic after the purge.

Please read official [documentation](https://github.com/ngutils/cr-identity) for crIdentity.


## Authentication
[Satelizer](https://github.com/sahat/satellizer) is a simple to use, end-to-end, token-based authentication module for AngularJS with built-in
support for Google, Facebook, LinkedIn, Twitter, Yahoo, Windows Live authentication providers, as well as Email and Password sign-in.
You are not limited to the sign-in options above, in fact you can add any OAuth 1.0 or OAuth 2.0 providers by passing provider-specific information during the configuration step.

In our opinion this is a very good library and it's very easy to use into ngStartup.

```javascript
angular.module('MyApp')
  .controller('LoginCtrl', function($scope, $rootScope, $auth) {

    $scope.authenticate = function(provider) {
      $auth.authenticate(provider).then(function(response) {
        $rootScope.$broadcast('auth:identity:success', {role: response.role, provider: 'my-provider', identity: {}}); // the crIdentity event
      });
    };

});
```
