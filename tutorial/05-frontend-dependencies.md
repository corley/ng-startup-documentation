# Frontend dependencies

## Overview
There are a lot of tools, modules and scripts that you can add to your project. Some of them are not related to AngularJS (such as jQuery), others yes (watch this list for useful modules:  [awesome-angularjs](https://github.com/gianarb/awesome-angularjs)).
Now we would discover how to add an external script (that shows beautiful charts) called Highcharts.js and its angular directive [pablojim/highcharts-ng](https://github.com/pablojim/highcharts-ng).

## Bower
[Bower](http://bower.io) is a dependency manager that helps you to define the external tools you want to integrate into your app. You have to download the packages `highcarts` and `highcharts-ng`. To do that add them to `bower.json`:

```
  "dependencies": {
    // ...
    "highcharts": "~4.1.8",
    "highcharts-ng": "~0.0.11",
    // ...
  }
```
The run this command in the shell:

```
$ bower update
```
Bower will download this files into `bower_components/` folder.


## Add the libraries to your projects

Now you're in a strange situation: the libraries are in `bower_components`, but you cannot use them because the app in `src/` doesn't know how to.
In `config/build.config.js` there are several lists that define which external js and css files must be added to the `src/index.html` during compiling and building phases.
Add the new dependencies to them:

```javascript
  vendor_files: {
    js: [
      // ...
      'bower_components/highcharts/highcharts.src.js',
      'bower_components/highcharts-ng/dist/highcharts-ng.js',
      // ...
     ]}
```

Now when you run `grunt build:web` (and all other tasks) this files are added to others already defined.

## Inject the new dependency

If you added an Angular module as highcharts-ng, you have to add it to the project dependencies `src/app/app.js`:

```javascript
angular.module('templates-app', []);
angular.module('templates-common', []);
angular.module(
        'ng-startup',
        [

            // ...
            'highcharts-ng',
            // ...
        ])
```

## Using the new module

Here a simple example of usage. Add it to your Dashboard Controller:

```javascript

  $scope.chartConfig = {
    options: {
        chart: {
            type: 'bar'
        }
    },
    series: [{
        data: [10, 15, 12, 8, 7]
    }],
    title: {text: 'Hello'},
    loading: false
  };
```

and call the directive in the dashboard template:

```html
<div class="row">
    <div class="col-md-12">
        <highchart id="chart1" config="chartConfig" ></highchart>
    </div>
</div>
```

Now your dashboard should show a list of news and a chart with fake data.
