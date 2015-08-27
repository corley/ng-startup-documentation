# Cordova/Phonegap

[Cordova](https://cordova.apache.org/) is a set of device APIs that allow a mobile app developer to access native device function such as the camera or accelerometer from JavaScript. Combined with a UI framework such as
jQuery Mobile or Dojo Mobile or Sencha Touch, this allows a smartphone app to be developed with just HTML, CSS, and JavaScript.

Angular is wrote in Javascript and we can use it to build Mobile Application. ngStartup help you to write and compile it!

## Coding with ngCordova

## Configuration
`config.xml` is the cordova manifest, this tool reads it and compile your application for different OS: Android, Windows 8, iOS..
Open this file and replace few custom configuration:

**Personal information**
```xml
    <name>AppName</name>
    <description>
        Description
    </description>

    <author href="http://" email="xyz@xyzxyzxyzxyz.xyz">
        Author
    </author>

    <preference name="permissions"                value="none"/>
```

**Platform and application preference**
```xml
    <preference name="phonegap-version"           value="3.7.0" />          <!-- all: current version of PhoneGap -->
    <preference name="orientation"                value="portrait" />        <!-- all: default means both landscape and portrait are enabled -->
    <preference name="target-device"              value="universal" />      <!-- all: possible values handset, tablet, or universal -->
    <preference name="fullscreen"                 value="true" />           <!-- all: hides the status bar at the top of the screen -->
    <preference name="webviewbounce"              value="false" />           <!-- ios: control whether the screen 'bounces' when scrolled beyond the top -->
    <preference name="prerendered-icon"           value="true" />           <!-- ios: if icon is prerendered, iOS will not apply it's gloss to the app's icon on the user's home screen -->
    <preference name="stay-in-webview"            value="false" />          <!-- ios: external links should open in the default browser, 'true' would use the webview the app lives in -->
    <preference name="ios-statusbarstyle"         value="black-opaque" />   <!-- ios: black-translucent will appear black because the PhoneGap webview doesn't go beneath the status bar -->
    <preference name="detect-data-types"          value="true" />           <!-- ios: controls whether data types (such as phone no. and dates) are automatically turned into links by the system -->
    <preference name="exit-on-suspend"            value="false" />          <!-- ios: if set to true, app will terminate when home button is pressed -->
    <preference name="show-splash-screen-spinner" value="false" />           <!-- ios: if set to false, the spinner won't appear on the splash screen during app loading -->
    <preference name="auto-hide-splash-screen"    value="true" />           <!-- ios: if set to false, the splash screen must be hidden using a JavaScript API -->

    <preference name="AutoHideSplashScreen" value="true" />
    <preference name="FadeSplashScreen" value="true" />
    <preference name="fade-splash-screen-duration" value="5" />

    <preference name="disable-cursor"             value="false" />          <!-- blackberry: prevents a mouse-icon/cursor from being displayed on the app -->
    <preference name="android-minSdkVersion"      value="14" />              <!-- android: MIN SDK version supported on the target device. MAX version is blank by default. -->
    <preference name="android-installLocation"    value="auto" />           <!-- android: app install location. 'auto' will choose. 'internalOnly' is device memory. 'preferExternal' is SDCard. -->
```

**Plugins**
```xml
    <gap:plugin name="com.adobe.plugins.gaplugin" version="2.4.0" />
    <gap:plugin name="nl.x-services.plugins.launchmyapp" version="3.2.0">
        <param name="URL_SCHEME" value="CHANGEWITHYOURAPPNAME" />
    </gap:plugin>
```
[See official documentation](http://cordova.apache.org/docs/en/5.0.0/config_ref_index.md.html#The%20config.xml%20File)

Another important service that we use to compile the application is [PhoneGapBuid](https://build.phonegap.com/). Its configurations are into the configuration.json file;
```json
{
  "phonegap": {
      "appId": "",
      "token": "",
       "keys": {
          "ios": { "password": "" },
          "android": { "key_pw": "", "keystore_pw": "" }
      }
  }
}
```

## Grunt Tasks
There are two important tasks to manage your mobile project:

```bash
grunt phonegapBuild
```
You this command to build a development version of your mobile app, it creates a `build` directory with your codebase.

```bash
grunt phonegapCompile
```
You this command to build a production version of your mobile app, it creates a `dir` directory with your codebase.
The code is minified, compiled and concat. It is ready to go in production.

Both build and compile::

* Will push your codebase into the PhoneGap Build service and start compilation
* Will wait that compilation to end.
* Will download the Android version and if your smartphone is connected at PC will install it on device.

```bash
grunt shell:androidLog
```
This command tail android device log if it is connected at PC

## Assets/Fonts/Images/Icons
`src/assets/phonegap` contains a lot of images, splashscreen and assets about your mobile application, they are the default phonegap assets, you can change them to customize your application.

## Dependencies
Your mobile application requires a way to include javascript/css files into the index.html, we use `build.config.js` to describe all
dependencies.
To manage your phonegap dependencies you should update the key `phonegap_files`.
