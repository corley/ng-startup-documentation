# Deploy

## Overview

ngStartup follows all steps of your development, the deploy is the last important task that moves your code to production.

![compiling](http://ngstartup.corleycloud.com/assets/images/ngstartup09.png)


## Prepare for production
```bash
$ grunt compile:web
$ grunt compile:mobile
```
This commands prepare `/dist/compile/web` and `/dist/compile/mobile` directories. The process:

* maintains lines of code between *devcode: compile*, *devcode: web* or *devcode: mobile* comments
* creates only one js file (named with project name and version) with all javascript (externals libraries and your angular files) miniefied and compressed
* creates only one js file (named with project name and version) with css compiled and compressed


```
bin/
├── assets
│   ├── fonts                   // fonts directory
│   ├── images                  // images direcotry
│   ├── ng-startup-0.0.2.css    // minified, concat and compressed css
│   ├── ng-startup-0.0.2.js     // minified, concat and compressed js
│   └── phonegap                // phonegap assets
├── i18n                        // translations
│   └── en_EN.json
└── index.html                  // the SPA index
```
This is an example of code ready to production.

`0.0.2` is the current version of your application, this value is stored into the `package.json` under the key `version`.

## S3
[S3](https://aws.amazon.com/s3) provides to developers and IT teams a secure, durable, highly-scalable object storage. Amazon S3 is easy to use, with a simple web services interface to store and retrieve any amount of data from anywhere on the web. It can used also to host a static website with own domain url. (For example the ng-startup documentation website is hosted on S3).

To upload the compiled version of the app to S3, edit the `/config/secret.json` file with your AWS keys, bucket name and region:
```
{
  "aws": {
      "accessKeyId":  "your-api-key",
      "secretAccessKey": "your-secret-key",
      "bucket": "ngstartup-bucket",
      "region": "eu-west-1" // Ireland
  }
}
```

then run:

```
grunt s3
```
