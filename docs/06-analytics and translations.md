# Analytics and translations

## Introduction

ngStartup includes [angularytics](https://github.com/mgonto/angularytics), and [angular-translate](https://github.com/angular-translate/angular-translate) in order to help you with stats management and app localization.

This modules let you to

* automatically tracks a new page view with GA and others analytics provider once you change the URL in your SPA
* track events either by calling the Angularytics service
* integrate translations from json files for different languages and use it in views and controller with specific filter
* switch to a different translation whenever you want

## Angularytics

Angularytics is the solution to tracking PageViews and Events on your Single Page App.

All analytics frameworks are based on browser refreshes to track page views. I've created this service wrapper so that we can track page views in our WebApp without a page refresh and without too much configuration.

### Configuration

As prerequisite you need an account on your analytics provider (for example Google Analytics);

To **enable** trackying add this codes to the `/src/app/app.js` file in **config** and **run** section.

``` javascript
.config(['AngularyticsProvider', function(AngularyticsProvider) {
    AngularyticsProvider.setEventHandlers(['Console', 'GoogleUniversal']);
}])
  .run(['Angularytics', function(Angularytics) {
    Angularytics.init();
  }]);
```

Just it. Stats will be tracked. If you need to disable it for any reasons:

``` javascript
.config(['AngularyticsProvider', function(AngularyticsProvider) {
  AngularyticsProvider.setEventHandlers(['Console', 'GoogleUniversal']);
  AngularyticsProvider.disablePageViewTracking();
}])
```

For advanced usage please read official [documentation](https://github.com/mgonto/angularytics) for Angularytics.

### Google prerequisite
You need to add the needed Analytics code (Google Analytics) in your app. I though of adding this in the service, but if you just add it in the body, it'd load MUCH faster than if we loaded this in the init. If everybody wants to move this to the service's init, then please submit an issue.

Sessiom data are stored into "default" namespace. You can define and use seprated namspace. If you want for example store product into a shopping cart, you can store it into a dedicated namespace.

## Translations
If you have to release an application for different language, you need a translation strategy. Angular-Translate helps you making it very easy.
ngStartup includes also a module to read remote json with translations (for example en_En.json) that will be included in your app.

### Configuration
ngStartup set the en_EN as default language getting it by the app configuration. You'll find a `/src/i18n/en_EN.json` file where put your translations. Of course you can add more files for different languages.



``` javascript
.config(['$translateProvider', function($translateProvider) {
  //set translation preferences
  $translateProvider.useStaticFilesLoader({
      files: [{
          prefix: 'i18n/',
          suffix: '.json'
      }]
  });
  $translateProvider.preferredLanguage(appConf.defaultLanguage);
  $translateProvider.useSanitizeValueStrategy('escaped');
}])
```

### Language files and usage
Here an example of a translation file as `/src/i18n/en_EN.json`:
```
{
  'hello': 'Hello World!!!!',
  'logout': 'Click here to logou!'
}
```

Use the **translate filter** for translations:
``` html
<p>{{ 'hello' | translate }}</p>
<p ng-bind="{{ 'hello' | translate }}"></p>
```

### Change language
When you need to change language, just use $translate (in controller or services):


``` javascript
  $translate.use('fr_Fr');
```

It will load the `/src/i18n/fr_Fr.json` file changing all translations in the app.
