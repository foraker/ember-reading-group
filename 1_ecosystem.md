# Set Up

## Install node
Necessary mostly because it comes with npm: http://nodejs.org/

## Install Ember CLI

http://www.ember-cli.com/

```
npm install -g ember-cli
npm install -g bower
npm install -g phantomjs
```

## Install watchman
https://facebook.github.io/watchman/docs/install.html

We want version 3.0.0, which is relatively recent, so update Homebrew first. It's telling me that it's installing 3.0.0 but `watchman version` still tells me 2.9.8, but this should work theoretically... If not, no worries.

```
brew update
brew install watchman
```

## Creating a New App
```
ember new my-new-app
cd my-new-app
ember server
```

## Things You Probably Want

### SASS support
`npm install --save-dev broccoli-sass`

### CoffeeScript support
`npm install --save-dev ember-cli-coffeescript`

Note that you will have to escape `import` and `export` lines in your .coffee
files with beginning and ending ticks, e.g. `\`import Ember from 'ember';\``

### Bootstrap
`bower install --save-dev bootstrap`

Then, in Brocfile.js, add the following line before `module.exports = app.toTree();`:

`app.import('bower_components/bootstrap/dist/css/bootstrap.min.css');`

This is an example of using Broccoli for asset management, which will learn more about shortly.

### Ember Inspector for Chrome
 https://chrome.google.com/webstore/detail/ember-inspector/bmdblncegkenkacieihfhpjfppoconhi

# Things Ember CLI Uses

You should know what these do and have a passing familiarity, mostly so you know what's responsible for what and can look into how to fix whatever problems you encounter.

## broccoli - https://github.com/broccolijs/broccoli


Build tool; "A fast, reliable asset pipeline, supporting constant-time rebuilds and compact build definitions. Comparable to the Rails asset pipeline in scope, though it runs on Node and is backend-agnostic"

You will probably add configuration to Brocfile.js to deal with vendor asset management.

## bower
http://bower.io/

"A package manager for the web"

## JavaScript modules
http://jsmodules.io/

Ember CLI uses the ES6 Module Transpiler (https://github.com/esnext/es6-module-transpiler) to support ES6 module syntax.

## Handlebars
http://handlebarsjs.com/

Template language. Files use the `hbs` extension.

## Watchman
https://facebook.github.io/watchman/

Watches files for changes and triggers options. Without it, Ember CLI will fall back to NodeWatcher.

## Express
http://expressjs.com/

A web framework for Node.js. It's used by Ember CLI to run your app in development as well as for mocking endpoints for Ember Data.

# :rowboat: Assignments

## JavaScript modules
http://jsmodules.io/

You'll use this syntax in your Ember apps. Read this short page.

## Ember CLI documentation

http://www.ember-cli.com/#getting-started

Read the following sections:

- Getting Started (Really just the "Folder Layout" layout bit)
- Naming Conventions
- Asset Compilation
- Managing Dependencies

# Bonus Point Assignments

## In Defense of Frameworks
:tv: https://www.youtube.com/watch?v=jScLjUlLTLI

Tom Dale and Yehuda Katz talk about the motivation behind building a framework and why it's a Good Thing.

Follow-up interview for extra bonus points: https://www.youtube.com/watch?v=VI__nGPT9kk

## A Day in the Life of an Ember Developer
https://www.youtube.com/watch?v=NM4EyOmaPbE

Yehuda talking at BrazilJS 2014. Compares/contrasts approach to data bindings, shows use of ember debugger, and demos the composability and maintainability enabled by Ember.
