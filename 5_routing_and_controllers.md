# Week 5: Routing and Controllers

## Notes

### General Routing Notes

Ember is *super* proud of its routing and places great importance in it.

Routes are responsible for returning the data rendered by the template.

`App.ApplicationRoute` is entered when your app first boots up. It renders the `application` template.

`App.IndexRoute` is the default route, and will render the `index` template when the user visits `/`.

You should use `this.resource` for URLs that represent a noun, and `this.route` for URLs that represent adjectives or verbs modifying those nouns.

### Example

```javascript
App.Router.map(function() {
  this.resource('posts', function() {
    this.resource('post', { path: '/post/:post_id' });
  });
});

```

```javascript
App.PostsRoute = Ember.Route.extend({
  model: function() {
    return this.store.find('post');
  }
});
```

Default behavior:

```javascript
App.PostRoute = Ember.Route.extend({
  model: function(params) {
    return this.store.find('post', params.post_id);
  }
});
```

### Auto-generation of Classes

Routes, controllers, and templates are all automatically generated for you based on URL if not defined.

For example, to have a plain template, you can just modify `app/router.js` to:

```javascript
Router.map(function() {
  this.route('bananas');
});
```

and add some markup to `app/templates/bananas.hbs` for it to be rendered when you visit `/bananas`.


## Assignments

### Routing Guide

:book: http://emberjs.com/guides/routing/

Read the whole section of the Ember Guides on Routing.

### Controllers Guide

:book: http://emberjs.com/guides/controllers/

Read the whole section of the EMber Guides on Controllers.

## Bonus Point Assignments

### Cage Match - EmberJS vs. Angular

:tv: https://vimeo.com/68215606

Tom Dale and Rob Conery go head-to-head implementing features in Ember and Angular. Skip to the 13 minute mark for the relevant bit. You can stop watching after Tom's first coding session but the whole thing provides an interesting comparision between Ember and Angular.
