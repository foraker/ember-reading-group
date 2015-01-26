# Week 7: Internals & Contributing

## Assignments

### Understanding Ember.js Guide

:book: http://emberjs.com/guides/understanding-ember/the-view-layer/

Read all the sections under "Understanding Ember.js" to get a deeper understanding of how Ember works beneath the hood.

### Contributing Guide

:book: http://emberjs.com/guides/contributing/adding-new-features/

## Bonus Point Assignments

### Contributing to Ember: The Inside Scoop

:tv: http://www.confreaks.com/videos/3299-emberconf2014-contributing-to-ember-the-inside-scoop

Robert Jackson of the Ember Release Management Team talks about contributing to Ember. Primarily interesting as it pertains to adding new features to Ember and how they hide them behind "feature flags" and the three release channels.

### Contributing to Big Bad Open Source

:book: http://robots.thoughtbot.com/contributing-to-big-bad-open-source

Narrative of starting to contribute to Ember.

### Developing an Ember CLI addon

:tv: https://www.youtube.com/watch?v=aNlxCxWs4O4

Talk about Ember CLI addons and creating one. (Slides: http://cl.ly/3s1A2K2Y0h3z)

## Notes

### Internals

#### The View Layer

Ember gets events from the application's root element (usually `body`) and then finds the appropriate view that handles the event when it occurs. This means there aren't a billion event listeners.

#### Managing Asynchrony

Ember avoids explicit forms of asynchronous behavior, favoring a higher level of abstraction. For example, we get to use computed properties now instead of explicitly listening for the change of the involved properties.

#### The Run Loop

Pretty much everything gets executed within the Run Loop. It batches and orders work to be efficient.

Work is placed in queues and run in priority order, which is as follows:

```javascript
Ember.run.queues
// => ["sync", "actions", "routerTransitions", "render", "afterRender", "destroy"]
```

#### Dependency Injection

##### Simple Examples

All route objects have the `router` property set on them during instantiation.

Routes and controllers have `store` injected into them by Ember Data.

##### Roll Your Own

```javascript
Ember.Application.initializer({
  name: 'logger',

  initialize: function(container, application) {
    var logger = {
      log: function(m) {
        console.log(m);
      }
    };

    application.register('logger:main', logger, { instantiate: false });
    application.inject('route', 'logger', 'logger:main');
  }
});
```

All routes will now have a `logger` property injected into them.

#### Service Lookup

Service lookup describes a dependency being created or fetched on demand. Primarily accomplished using `needs`. For example:

```javascript
var App = Ember.Application.create();
App.SessionController = Ember.Controller.extend({
  isAuthenticated: false
});
// The index controller may need access to that state:
App.IndexController = Ember.Controller.extend({
  needs: ['session'],
  // Using needs, the controller instance will be available on `controllers`
  isLoggedIn: Ember.computed.alias('controllers.session.isAuthenticated')
});
```

### Contributing

Ember-related repositories generally have a CONTRIBUTING.md file that you should read. Ember itself has some weird prefixing rules for commit messages and PR titles.

#### Ember.js

https://github.com/emberjs/ember.js

Ember is broken out into several packages, which you can see in [/packages](https://github.com/emberjs/ember.js/tree/master/packages). The contributing guide details how to run specs. Clicking on the pencil icon from an API page takes you to the relevant source. The API pages are generated from [this sweet 28,508 line YAML file](https://github.com/emberjs/ember.js/tree/master/packages).

There are three build lanes: Canary, Beta, and Release.

Features are hidden behind "feature flags". You can see current features that are being tested in [FEATURES.md](https://github.com/emberjs/ember.js/blob/master/FEATURES.md).

#### Ember's Website

https://github.com/emberjs/website

As you have been browsing the guides, you may have noticed a pencil icon next to the main header. Clicking it will take you to the relevant Markdown file on Github. Fix typos! Clarify things!

#### Ember Data

https://github.com/emberjs/data

#### Ember CLI

https://github.com/ember-cli/ember-cli
