# Week 1: Ecosystem

## Assignments

### ES6 syntax
:book: https://6to5.org/docs/learn-es6/

Ember CLI supports ECMAScript 6 by using a transpiler. You should become acquainted with the new syntax and features. You'll be using modules frequently; [read more about modules here](http://jsmodules.io/).

### Ember CLI documentation

:book: http://www.ember-cli.com/#overview

Read the following sections:

- [Getting Started](http://www.ember-cli.com/user-guide/#getting-started) (especially [Folder Layout](http://www.ember-cli.com/user-guide/#folder-layout))
- [Naming Conventions](http://www.ember-cli.com/user-guide/#naming-conventions)
- [Asset Compilation](http://www.ember-cli.com/user-guide/#asset-compilation)
- [Managing Dependencies](http://www.ember-cli.com/user-guide/#managing-dependencies)

## Bonus Point Assignments

### In Defense of Frameworks
:tv: https://www.youtube.com/watch?v=jScLjUlLTLI

Tom Dale and Yehuda Katz talk about the motivation behind building a framework and why it's a Good Thing.

:tv: https://www.youtube.com/watch?v=VI__nGPT9kk

Follow-up interview for extra bonus points.

### A Day in the Life of an Ember Developer
:tv: https://www.youtube.com/watch?v=NM4EyOmaPbE

Yehuda talking at BrazilJS 2014. Compares/contrasts approach to data bindings, shows use of ember debugger, and demos the composability and maintainability enabled by Ember.

## Notes

### Things Ember CLI Uses

You should know what these do and have a passing familiarity, mostly so you know what's responsible for what and can look into how to fix whatever problems you encounter.

#### broccoli
https://github.com/broccolijs/broccoli

Build tool; "A fast, reliable asset pipeline, supporting constant-time rebuilds and compact build definitions. Comparable to the Rails asset pipeline in scope, though it runs on Node and is backend-agnostic"

You will probably add configuration to Brocfile.js to deal with vendor asset management.

#### bower
http://bower.io/

"A package manager for the web"

#### JavaScript modules
http://jsmodules.io/

Ember CLI uses the ES6 Module Transpiler (https://github.com/esnext/es6-module-transpiler) to support ES6 module syntax.

#### Handlebars
http://handlebarsjs.com/

Template language. Files use the `hbs` extension.

#### Watchman
https://facebook.github.io/watchman/

Watches files for changes and triggers actions. Without it, Ember CLI will fall back to NodeWatcher.

#### Express
http://expressjs.com/

A web framework for Node.js. It's used by Ember CLI to run your app in development as well as for mocking endpoints for Ember Data.


### Set up

#### Install node
Necessary mostly because it comes with npm: http://nodejs.org/

#### Install Ember CLI

http://www.ember-cli.com/

```
npm install -g ember-cli
npm install -g bower
npm install -g phantomjs
```

#### Install watchman
https://facebook.github.io/watchman/docs/install.html

We want version 3.0.0, which is relatively recent, so update Homebrew first. It's telling me that it's installing 3.0.0 but `watchman version` still tells me 2.9.8, but this should work theoretically... If not, no worries.

```
brew update
brew install watchman
```

#### Creating a New App
```
ember new my-new-app
cd my-new-app
ember server
```

#### Things You Probably Want

##### SASS support
`ember install ember-cli-sass`

##### CoffeeScript support
`ember install ember-cli-coffeescript`

Note that you will have to escape `import` and `export` lines in your .coffee
files with beginning and ending ticks, e.g.

    `import Ember from 'ember';`

##### Bootstrap
`ember install:bower bootstrap`

Then, in Brocfile.js, add the following line before `module.exports = app.toTree();`:

`app.import('bower_components/bootstrap/dist/css/bootstrap.min.css');`

This is an example of using Broccoli for asset management, which will learn more about shortly.

##### Ember Inspector for Chrome
 https://chrome.google.com/webstore/detail/ember-inspector/bmdblncegkenkacieihfhpjfppoconhi
