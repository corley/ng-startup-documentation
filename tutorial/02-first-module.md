# First Module

## Introduction
In cominciamo dalla creazione di un nuovo modulo, la dashboard, immaginiamola come un elenco di news sotto la rotta `/dashboard`.  
La prima cosa da fare è creare una cartella `./tutorial/src/app/dashboard` in modo da isolare tutto il codice relativo a questo modulo
ordinatamente al suo interno. Questo ci permetterà di lavorare in modo isolato senza rischiare di dimenticarci la collocazione dei files.

## Adding a new module
La struttura delle cartelle sarà simile a quella del modulo di default `home` ma per ora possiamo limitarci ad un solo file `dashboard.js` in cui andremo ad interagire con AngularJs, creando controlli,
direttive ed un file `dashboard.tpl.html` con il template della nostra pagina.
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

Creiamo il nostro primo controller.
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
.controller( 'DashboardCtrl', ['$scope', '$stateParams'', function DashboardCtrl( $scope, $stateParams) {
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
Ricordiamo di aggiungerlo all'interno del file `./tutorial/src/app/app.js`
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
        'ng-startup.dashboard',
        'ui.router',
        "..."
    ]
)
```
Come abbiamo scritto all'interno della funzione di configurazione del nostro modulo, `DashboardCtrl` risolve il template `dashboard.tpl.html` il nostro elenco di notizie
```
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
Siamo pronti per visitare la rotta /dashboard sul nostro browser e raccogliere i frutti del nostro lavoro
