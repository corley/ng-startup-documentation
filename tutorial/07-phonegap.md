# Phonegap

## Overview
Okay, your app is ready and works perfectly in web browser. You have a signin page, a reserved area and remote resources accessible via http.
It's the moment to create a mobile version of your website for Android and iOS devices. Please register on [Phonegap](http://build.phonegap.com), it's free.

## Configuring Phonegap

ngStartup has a Grunt task named `grunt compile:mobile` that compiles your application then uploads it to Phonegap to get back the app sources for Android and iOS. To make it possible, you need to fill the `config/secret.json` file with some credentials that you can find on Phonegap. Remember that the secret file is out of version (listed in gitignore) in order to prevent accidental credentials distribution.

Go to your account area in [Phonegap](http://build.phonegap.com) and get the app key and the access token (from account label)
![phonegap1](http://ngstartup.corleycloud.com/assets/images/phonegap01.png)

![phonegap2](http://ngstartup.corleycloud.com/assets/images/phonegap02.png)


then add them to the `config/secret.json` file:

```
{
  "aws": {
      "accessKeyId":  "",
      "secretAccessKey": "",
      "bucket": "",
      "region": ""
  },
  "phonegap": {
      "appId": "123456789",
      "token": "xxxyyyzzz",
       "keys": {
          "ios": { "password": "" },
          "android": { "key_pw": "", "keystore_pw": "" }
      }
  }
}
```

You have also to change the `config/phonegap.config.xml` as specified on official [documentation](http://docs.build.phonegap.com/en_US/configuring_basics.md.html#The%20Basics). The default file is okay for a test, but remember to change at least the app name. Find and replace *CHANGEWITHYOURAPPNAME* in the file.

## Use ngCordova

ngStartup uses by default [ngCordova](http://ngcordova.com), a set of angular modules that wrap several Phonegap plugins making them easy to be used in your application. Whne you want to use a plugin, add it to `config/phonegap.config.xml`, and change the index.html. For example, if you want to use the [Globalization Plugin](http://ngcordova.com/docs/plugins/globalization/), add it to the `config/phonegap.config.xml`:

```
  <gap:plugin name="org.apache.cordova.globalization" version="0.3.1" />
```

then add the cordova javascript files in the `src/index.html`:
```html
<head>
  <!-- devcode: phonegap -->
  <script type="text/javascript" src="cordova.js"></script>
  <script type="text/javascript" src="globalization.js"></script>
  <!-- endcode -->
</head>
```

Phonegap/cordova will add these files to your project during building. Remember that the *devcode* comments reserves the lines of codes to `grunt build:mobile` and `grunt compile:mobile` releases, cutting them in web stage.

Now you can add your login inside your scripts, for example in `src/app/app.js`:

``` javascript
.run(['$rootScope', 'crAcl', 'crSession', 'crRemoteHttp', 'crIdentity', '$state', '$log', '$cordovaGlobalization',
function run($rootScope, crAcl, crSession, crRemoteHttp, crIdentity, $state, $log, $cordovaGlobalization) {

  //phonegap callback on device ready
  document.addEventListener("deviceready", function() {
    $cordovaGlobalization.getPreferredLanguage().then(
      function(result) {
        //ok
      },
      function(result) {
        //err
      }
    );


  });

}])
```


## Compile the app

When you're ready, run the `grunt compile:mobile` to create the version and upload it to Phonegap. The process can take few minutes, then Grunt will donwload the apps and install it on your Android device (if connected on usb). You can try your app on your phone and if you want watch logs running the task `grunt shell:androidLogs`
