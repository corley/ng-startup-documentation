# Analytics and translations

## Overview

ngStartup includes [Angulartics](https://github.com/angulartics/angulartics), and [angular-translate](https://github.com/angular-translate/angular-translate) in order to help you with states management and app localization.

This modules lets you to

* automatically tracks a new page view with GA and others analytics provider once you change the URL in your SPA
* track events either by calling the Angularytics service
* integrate translations from json files for different languages and use it in views and controller with specific filter
* switch to a different translation whenever you want

## Angularytics

Angularytics is the solution to tracking PageViews and Events on your Single Page App.

All analytics frameworks are based on browser refreshes to track page views. This service wraps analytics so that you can track page views in ngStartup without a page refresh and without too much configuration.

### Configuration

As prerequisite you need an account on your analytics provider (for example Google Analytics);

To **enable** trackying add this codes to the `/src/app/app.js` file in **dependencies list**.

``` javascript

angular.module(
  'ng-startup',
  [
    // other dependencies...
    'angulartics',
    'angulartics.google.analytics',
    // other dependencies...
  ]
)

```

Just it. Stats will be tracked. The snippet code provided by Google Analytics does an automatic pageview hit, but this is already done by Angulartics (unless you disable it) so make sure to delete the tracking line:

``` javascript
...
     ga('create', 'UA-XXXXXXXX-X', 'none'); // 'none' while you are working on localhost
     ga('send', 'pageview');  // DELETE THIS LINE!
   </script>
```

For advanced usage please read official [documentation](https://github.com/angulartics/angulartics) for Angulartics.

## Translations
If you have to release an application for different languages, you need a translation strategy. Angular-Translate helps you making it very easy.
ngStartup includes also a module to read remote json with translations (for example en_EN.json).

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
  'logout': 'Click here to logout!'
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

It will load the `/src/i18n/fr_Fr.json` file changing all translations in the app in realtime.
