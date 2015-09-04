# Dependency Injection

## Introduction
L'ecosistema di moduli e componenti AngularJS è veramente vasto, per rendere l'idea del quantitativo di componenti messi a disposizione dagli sviluppatori potete seguire questo link [awesome-angularjs](https://github.com/gianarb/awesome-angularjs).
Con ngStartup e gli strumenti messi a disposizione è ovviamente possibile integrare dipendenze di terze parti per poter usitizzare librerie e costruire applicazioni di qualità. Di seguito vedremo come integrare [pablojim/highcharts-ng](https://github.com/pablojim/highcharts-ng)
un componente per la costruzione di grafici.

## Bower
[Bower](http://bower.io) è un gestore di dipendenze, possiamo andare in console nella radice del nostro progetto ed eseguire questo comando

```
$ bower install highcharts-ng
```

## Loading
Dopo aver risolto la dipendenza dobbiamo comunicare alla nostra applicazione il path dei file utili per far funzionare il componenti, in questo caso
```
bower_components/highcharts-ng/dist/highcharts-ng.js
```

Apriamo il file `config/build.config.js` e aggiungiamo nella sezione corretta in questo caso, visto che stiamo lavorando in ambito web e non con phonegap andrà aggiunto
```javascript
  vendor_files: {
    js: [
      'bower_components/angular/angular.js',
      'bower_components/jquery/dist/jquery.js',
      'bower_components/bootstrap/dist/js/bootstrap.js',
      'bower_components/angular-ui-router/release/angular-ui-router.js',
      'bower_components/angular-bootstrap/ui-bootstrap.js',
      'bower_components/angular-bootstrap/ui-bootstrap-tpls.js',
      'bower_components/angular-loading-bar/build/loading-bar.js',
      'bower_components/angular-ui-utils/modules/route/route.js',
      'bower_components/highcharts-ng/dist/highcharts-ng.js',
      '..'
     ]}
```

A questo punto il file è caricato dalla nostra applicazione e possiamo utilizzare il componente all'interno dei nostri moduli.

## App.js
Per poter utilizzare il modulo all'interno dei nostri moduli è necessario caricarlo attraverso la dependency injection di angular, apriamo quindi il nostro file `app.js` ed aggiungia il nuovo componente.

```javascript
angular.module('templates-app', []);
angular.module('templates-common', []);
angular.module(
        'ng-startup',
        [
            'templates-app',
            'templates-common',
            'angulartics',
            'angulartics.google.analytics',
            'LocalStorageModule',
            'ngAnimate',
            'ngSanitize',
            'angular-loading-bar',
            'templates-app',
            'templates-common',
            'highcharts-ng',
            '...',
        ])
```
