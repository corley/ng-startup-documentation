# Deploy

ngStartup follows all step of your development, the deploy is tha last important task to move your code in production.

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

## S3
```
grunt s3
```
If a task that push bin directory into an AWS s3 bucket. You can configure it into the `configuration.json` file.
