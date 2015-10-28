# REST resources

## Overview

Web and mobile apps usually work with remote resources (webservices and REST endpoints). Angular gives you some great tool make HTTP calls, but we have add other modules so you can choose the best for you.

## $http
Of course you can use $http, a module by Angular to make HTTP calls to a specific endpoint.

Remember that cfpLoadingBar and crLoading work with $http in order to show spinners and loading bars. So any tool based on $http will activate these modules.

Please read official [documentation](https://docs.angularjs.org/api/ng/service/$http) for $http.

## crRemote

ngUtile includes [CrRemote](https://github.com/ngutils/cr-remote), a module based on $http useful to create services that work with RESTful and remote resources.

### Configuration
```javascript
.config(function myAppConfig(crRemoteProvider) {

  //set endpoint
  crRemoteProvider.setEndpoint("http://api1.test.it");

  //set interceptors

  //request interceptor to edit data befor the call
  crRemoteProvider.addRequestInterceptor("default", function(data){
    data.params.session = "mysessionid";
    return data;
  });

  //succesful response interceptor
  crRemoteProvider.addResponseInterceptorSuccess("default", function(data){
    data.now = new Date();
    return data;
  });

  //wrong response interceptor
  crRemoteProvider.addResponseInterceptorError("default", function(data){
    console.log("aaaargh!", data);
    return data;
  });

})
```

### Simple usage

You can user directly crRemote to make HTTP requests.

``` javascript
.controller('AppCtrl', function AppCtrl($scope, crRemoteHttp) {

    //retrieve product
    crRemoteHttp.get({
        resourceName: "/product"
    }).then(function(res) {
      console.log(res);
    }, function(error) {
      console.log(error);
    });

    //post a new entry
    crRemoteHttp.post({
        id: 22
        resourceName: "/product",
        data: {
          title: 'product',
          price: 22.5
        }
    }).then(function(res) {
      console.log(res);
    }, function(error) {
      console.log(error);
    });
});
```

You can use GET POST PUT PATCH and delete methods that return a promise.
Successful callbacks **return by default an object** that contains data (body response), headers() (function that return headers) and status (http status code, exp 200);

### Create injeactable modules

The best way is create dedicated module with specific methods for each remote resource.


``` javascript
  //define a new service
  .service('OrderResource', ['crRemoteHttp', function(crRemoteHttp){
   var service = crRemoteHttp.createService("order");

   service.changeStatus = function(id, status) {
     return service.patch({"id": id, data:{"status": status}});
   };

   return service;
  }])

  //use in a controller
  .controller('AppCtrl', function AppCtrl($scope, OrderResource) {
    OrderResource.changeStatus(123, 'shipped').then(function(res) {
     console.log(res);
    }, function(err) {
     console.log(err);
    })
  });
```

### Call options
When you make a call(get, post, put, patch and delete) with crRemoteHttp or a module created on it you can define different options:
```javascript
crRemoteHttp.post({
  id: 22 // id of resource, that will be used to create the endpoint url (see advanced settings below)
  resourceName: "/product", // the name of the resource, as the id is used to create the url
  data: { //data sent to endpoint in the body of call
    title: 'product',
    price: 22.5
  },
  cache: true //if the client has to cache response, false by default
  timeout: 5000 //milliseconds or a promise for the timeout, 0 by default (no timeout),
  params: { //object of Query params (?q=myname)
    q: 'myname'
  },
  headers: { //object of addiction request headers
    'x-myheader': '123'
  },
  auth: true //if false, an eventual Authorization header will be removed for the call; false by default
})

```

You can use the options also when you **create a module**:

``` javascript
  //define a new service
  .service('OrderResource', ['crRemoteHttp', function(crRemoteHttp){
   var service = crRemoteHttp.createService("order", {
     auth: true, // all calls by this module will use authorization header
     endpoint: 'test', // useful if you use different endpoint and you added another endpoint during configuration
     endpointBuilder: 'anotherbuild' // the function that creat the url of resource, see advanced settings for more info
   });
   return service;
  }])
```



### Manage authorization
We recommend to use [Satelizer](https://github.com/sahat/satellizer) to manage login and authetication. Satellizer adds Authentication header to http calls (and crRemote it's based on $http).

However, you case easily use directly $http to set authorization headers for your calls.

```javascript
//in a controller or service
$http.defaults.headers.common['Authorization'] = 'Basic ' + $rootScope.user.authtoken; // it's just an example

OrderResource.get({'id': 22, 'auth': true}); //make a call using the Authorization header
```

Remember that the **auth options is false by default**, so you have to set it when you make a call or when you define a new service.


### Advanced settings

During configuration you can set different endpoints, interceptors and builders that will be used by your service.


```javascript
.config(function myAppConfig(crRemoteProvider) {

  //set different endpoints
  crRemoteProvider.setEndpoint("http://production.mydomain.com/api/"); //add as default endpoint
  crRemoteProvider.setEndpoint("http://dev01.mydomain.com/api/", "test"); //add as default endpoint


  //set a new interceptor
  crRemoteProvider.addResponseInterceptorSuccess("paginator", function(response){
    //add to response an object with custom values, in this example a simply paginator and counter
    response.paginator = {
      total: response.data.length,
      page: (response.data.length - (response.data.length & 10) ) / 10 + 1
    };
    return data;
  });

  /**
  by default, crRemoteHttp build endpoint in this way: endpoint + resource name + / + id (if set) + query (if set)
  example https://api.mydomain.com/product/22?status=enabled
  you can rewrite it:
  */

  crRemoteProvider.addEndpointBuilder('newbuilder',  function(endpoint, resourceName, resourceId, params) {
    return endpoint + id + "/" + resourceName; //it will return https://api.mydomain.com/22/product
  });
})


//define a new service with previous settings
.service('OrderResource', ['crRemoteHttp', function(crRemoteHttp){
 var service = crRemoteHttp.createService("order", {
   endpointBuilder: 'newbuilder',
   endpoint: 'test',
   responseInterceptorSuccess: 'paginator'
 });

 service.changeStatus = function(id, status) {
   return service.patch({"id": id, data:{"status": status}});
 };

 return service;
}])

```
