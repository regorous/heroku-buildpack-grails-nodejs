Heroku buildpack: Grails and Node.js
====================================

This is a [Heroku buildpack](http://devcenter.heroku.com/articles/buildpacks) for Grails apps that also requires NodeJS.

It uses [NPM](http://npmjs.org/) and [SCons](http://www.scons.org/).

Usage
-----

Create a Git repository for a Grails 1.3.7 or 2.0 app:

    $ cd mygrailsapp
    $ ls
    application.properties    lib        src               target    web-app
    grails-app                scripts    stacktrace.log    test
    $ grails integrate-with --git
    | Created Git project files..
    $ git init
    Initialized empty Git repository in /Users/jjoergensen/mygrailsapp/.git/
    $ git commit -m init
    [master (root-commit) 7febdd9] init
     58 files changed, 2788 insertions(+), 0 deletions(-)
     create mode 100644 .classpath
     create mode 100644 .gitignore
     create mode 100644 .project
     create mode 100644 application.properties
    ...
    
Create a Heroku app on the Cedar stack

    $ heroku create --stack cedar
    Creating vivid-mist-9984... done, stack is cedar
    http://vivid-mist-9984.herokuapp.com/ | git@heroku.com:vivid-mist-9984.git
    Git remote heroku added

Push the app to Heroku

    $ git push heroku master
    Counting objects: 73, done.
    Delta compression using up to 4 threads.
    Compressing objects: 100% (69/69), done.
    Writing objects: 100% (73/73), 97.82 KiB, done.
    Total 73 (delta 2), reused 0 (delta 0)

    -----> Heroku receiving push
    -----> Grails app detected
    -----> Grails 2.0.0 app detected
    -----> Installing Grails 2.0.0..... done
    -----> executing grails -plain-output -Divy.default.ivy.user.dir=/app/tmp/repo.git/.cache war

           |Loading Grails 2.0.0
           |Configuring classpath
    ...

To configure the Node.JS version, put a package.json file in the base directory of your project that lists project dependencies including Node.JS and NPM versions.

    {
      "name": "myapp",
      "version": "0.0.1",
      "engines": {
        "node": ">=0.4.7 <0.7.0",
        "npm": ">=1.0.0"
      }
    }

To list the available versions of Node.js and npm, see these manifests:

http://heroku-buildpack-nodejs.s3.amazonaws.com/manifest.nodejs
http://heroku-buildpack-nodejs.s3.amazonaws.com/manifest.npm

### Auto-detection

Heroku auto-detects Grails apps by the existence of the `grails-app` directory in the project root and the `application.properties`  file is also expected to exist in the root directory. 

### Using a Customized (Forked) Build Pack

This is the default buildpack repository for Grails. You can fork this repo and tell Heroku to use the forked version by passing the `--buildpack` option to `heroku create`:

    $ heroku create --stack cedar --buildpack http://github.com/jesperfj/heroku-buildpack-grails.git

## License

Licensed under the MIT License. See LICENSE file.

