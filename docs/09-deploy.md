# Deploy

## Introduction

ngStartup follows all steps of your development, the deploy is the last important task to move your code in production.

![compiling](http://ngstartup.corleycloud.com/assets/images/ngstartup09.png)


## Prepare for production
```bash
grunt compile
```
This command prepares `bin` directory, it contains your code ready to go in production. It is

Javascript files are
* concat
* minified
* compressed

SASS and LESS files are compiled and they are concat and minify with all other CSS files.

```
bin/
├── assets
│   ├── fonts                   // fonts directory
│   ├── images                  // images direcotry
│   ├── ng-startup-0.0.2.css    // minified, concat and compressed css
│   ├── ng-startup-0.0.2.js     // minified, concat and compressed js
│   ├── phonegap                // phonegap assets
│   └── README.md               // Description file
├── i18n                        // translation
│   └── en_EN.json
└── index.html                  // entry point
```
This is an example of code ready to production.
`bin/assets/ng-startup-0.0.2.css` is the result of compression and concat of all your style files.  
Same stuff for `bin/assets/ng-startup-0.0.2.js`, it contains all javascript files compiled and minified.

`0.0.2` is the current version of your application, this value is stored into the `package.json` under the key `version`.

## S3
[S3](https://aws.amazon.com/s3/?nc1=h_ls) provides developers and IT teams with secure, durable, highly-scalable object storage. Amazon S3 is easy to use, with a simple web services interface to store and retrieve any amount of data from anywhere on the web.
```
grunt s3
```
If a task that push bin directory into an AWS s3 bucket. You can configure it into the `configuration.json` file.

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
