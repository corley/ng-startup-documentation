# Working with REST

## Introduction
In un case reale non troveremo mai una lista di articoli all'interno di un array, è molto più probabile che saremo
costretti ad interagire con un API per scaricare le notizie aggiornate. In questo capitolo vedremo come utilizzare
[crRemote](https://github.com/ngutils/cr-remote) per interrogare un servizio api esterno.

## Configuring Endpoint
Il modulo è già inserito di default come dipendenza di ng-startup ma come prima cosa dovremmo configurarlo all'interno del file `./tutorial/src/app/app.js`.
Sarà necessario inizializzarlo con l'url delle nostre api.
```javascript
.config(['$urlRouterProvider', '$translateProvider', '$authProvider', 'cfpLoadingBarProvider', 'crRemoteProvider', 'appConf', '$logProvider',
  function myappConf($urlRouterProvider,  $translateProvider, $authProvider, cfpLoadingBarProvider, crRemoteProvider, appConf, $logProvider) {

    //set translation preferences
    $translateProvider.useStaticFilesLoader({
        files: [{
            prefix: 'i18n/',
            suffix: '.json'
        }]
    });
    $translateProvider.preferredLanguage("en_EN");
    $translateProvider.useSanitizeValueStrategy('escaped');

    //set api andpoint by app config
    crRemoteProvider.addEndpoint("default", "http://rest.test.corley.it/");

    //enable debug state
    $logProvider.debugEnabled(true);

    //set home state
    $urlRouterProvider.otherwise('/home');

    //choose between spinner and/or bar fixed to top
    cfpLoadingBarProvider.includeSpinner = false;
    // cfpLoadingBarProvider.includeBar = false;

 }])
```
Ora possiamo aggiungere all'interno del nostro modulo dashboard un servizio che ci permetta di interrogare la risorsa News dall'api.
```
.service('NewsRest', ['crRemoteHttp', function(crRemoteHttp){
    var service = crRemoteHttp.createService("news");
    return service;
}])
```

Adesso dobbiamo modificare il nostro `DashboardCtrl` eliminando l'array di notizie e implementare una soluzione più realistica.
```javascript
/**
 * Dashboard controller
 */
.controller( 'DashboardCtrl', ['$scope', '$stateParams'', function DashboardCtrl( $scope, $stateParams, NewsRest) {
    NewsRest.get().then(function(res) {
        $scope.results = res.data;
    });
}]);
```
Ha un aspetto migliore, facciamo refresh del nostro browser per leggere le ultime novità!
