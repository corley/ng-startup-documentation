# Tutorial

## Introduction

[ngStartup](https://github.com/corley/ng-startup) is an "app boilerplate" suited to create professional applications with AngularJS. It helps you to coding better and offers great tools to manage deploy and release stages.

In this tutorial we'll discover how to develop a single place application ready for production, base on a home page, a reserved area (protected by login), and an integration with RESTful resources. We'll use Access Control List to protect different areas of the app.
Starting by the *master* branch, we'll get step by step the final result discovering the major features of ngStartup.


## How to start
You need nodejs installed on your machine.
Download the ngStartup project on [GitHub](https://github.com/corley/ng-startup) and install it running the following commands on bash:
```bash
$ git  clone git@github.com:corley/ng-startup ./tutorial
$ cd tutorial
$ npm install
$ grunt watch:web
```
The last command runs a Grunt command that watches the `src/` folder (where the sources are) and compiles all files moving them in `dist/build/web`. At the same time, it runs a local server, so you can visit the app with your browser at [http://localhost:8081](http://localhost:8081) (or [http://127.0.0.1:8081](http://127.0.0.1:8081), it's the same).

> Rembember: you code in `src/` folder, and Grunt compiles it into `dist/build/web`

Now you can start to code following next steps.
