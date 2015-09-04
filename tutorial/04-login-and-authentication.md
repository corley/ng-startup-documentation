# Authentication and Authorization

## Introduzione
In questa ultima parte del nostro tutorial vedremo come utilizzare una serie di moduli di terze parti che reputiamo ben supportati e scritti per implementare una soluzione di autentication e di gestione degli accessi.  
Ci serviremo quindi di una serie di moduli, [sallizer](https://github.com/sahat/satellizer) per il sistema di autenticazione, [crSession](https://github.com/ngutils/cr-session) per la sessione utente e [crAcl](https://github.com/ngutils/cr-acl) per controllare il livello di visibilità dei nostri utenti.
In sostanza creeremo un modulo login per effettuare l'accesso e nasconderemo la dashboard ai soli autorizzati.

## Configuration $auth
La prima cosa da fare è configurare `$auth` (satellizer) all'interno dell'`app.js` per comunicarci l'endpoint per eseguire l'autenticazione 
ed il tipo, nel nostro caso si tratta di Basic Authentication.

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

    // ....

    $authProvider.loginUrl = 'http://rest.test.corley.it/login';
    $authProvider.authToken = 'Basic';

    // ....

}]);
```

Sembre all'interno dell'app.js aggiungiamo qualche linea di codice in grado di spiegare alla nostra app cosa fare in caso di utente non autorizzato 
oppure in caso di corretta autenticazione.
```javascript
.run(['$rootScope', 'crAcl', 'crSession', 'crRemoteHttp', 'crIdentity', '$state', '$log',
function run($rootScope, crAcl, crSession, crRemoteHttp, crIdentity, $state, $log) {

    //what append on user successful login
    $rootScope.$on('auth:identity:success', function(event, data) {
      $state.go("dashboard", {'area': 'test'});
    });

    //what append on user logout
    $rootScope.$on("auth:purge:success", function(event, data){
      $state.go("home");
    });

}])
```

## Login Module
Aggiungiamo un altro modulo alla nostra applicazione che conterrà la pagina di login con la logica di autenticazione, creiamo una struttura come questa
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

Il andiamo a lavorare a due controller, uno per il login `SigninCtrl` e uno per il logout `SignoutCtrl`, apriamo il file sigin.js
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
Rispettivamente risponderanno alla rotta `/signin` e `/signout` ma solo una dei due necessità di un template, apriamo il file `signin.tpl.html` e creiamo il nostro modulo di login.
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
Guardatelo attentamente perchè tra l'html si nascondo un paio di funzioni interessanti, in primis la funzione `doLogin` che accetta nome utente e password e si preoccupa di eseguire l'autenticazione.
A questo punto se siamo stati bravi andando alla pagina `/signin` vedremo il nostro modulo di login, ma la potenza non è nulla senza il controllo, 
dobbiamo fare in modo che le nostre news diventino inleggibile per chi non è autorizzato.

## Acl
[crAcl](https://github.com/gianarb/cr-acl) è un modo che ci aiuta a gestire le regole dei nostri utenti e decidere cosa far vedere o no della nostra applicazione in base al loro ruolo.
Di default supporta due tipi di ruolo

* ROLE_GUEST = utente non autenticato
* ROLE_USER = utente autenticato

ma è possibile configurare i propri ruoli con un sistema gerarchico ed ereditario, per intenderci se volessimo creare un ruolo ROLE_CLIENT per gestire i clienti del nostro e-commerce,
potremmo partire estendendo ROLE_USER.

Aggiungiamo al nostro `app.js` una particolare configurazione per comunicare a crAcl cosa fare quando intercetta un tentativo di intrusione.
```javascript
.run(['$rootScope', 'crAcl', 'crSession', 'crRemoteHttp', 'crIdentity', '$state', '$log',
function run($rootScope, crAcl, crSession, crRemoteHttp, crIdentity, $state, $log) {

    //set default login state for unauth users
    crAcl.setRedirect("signin");



    //what append on user successful login
    $rootScope.$on('auth:identity:success', function(event, data) {
      $state.go("dashboard", {'area': 'test'});
    });

    //...

}]);
```

E poi configuriamo il nostro `DashboardCtrl` in modo che permetta ai soli autorizzati, quelli che soddisfano la ROLE_USER di accedervi, apriamo `dashboard.js`
e riconfiguriamo la rotta dashboard aggiungendo `is_granted:['ROLE_USER']`.
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
Ora proviamo ad accedere alla rotta `/dashboard` come utenti non collegati, per essere rimandati alla pagina di login.
