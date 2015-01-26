# Week 5: Routing and Controllers

## Assignments

### Routing Guide

:book: http://emberjs.com/guides/routing/

Read the whole section of the Ember Guides on Routing.

### Controllers Guide

:book: http://emberjs.com/guides/controllers/

Read the whole section of the Ember Guides on Controllers.

## Bonus Point Assignments

### Cage Match - EmberJS vs. Angular

:tv: https://vimeo.com/68215606

Tom Dale and Rob Conery go head-to-head implementing features in Ember and Angular. Skip to the 13 minute mark for the relevant bit. You can stop watching after Tom's first coding session but the whole thing provides an interesting comparision between Ember and Angular.

### Mr Router Embraces the Controller

:tv: https://www.youtube.com/watch?v=Syv_OTzHOr0&list=PLE7tQUdRKcyaOyfBnAndJxQ9PNVmKva0d&index=5

Talk was given before query params were released. Discusses motivation for putting them in controllers instead of routes. Explains division of router and controller responsibilities.

## Notes

### Routing

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
    // Note that `params` provides the dynamic segments
    return this.store.find('post', params.post_id);
  }
});
```

Within your route subclass, you can define `setupController: function (controller) { }` to set attributes on your controller, for example. It gets the model as a second parameter so you can explicitly set `controller.model` if you need to for some reason.

The `model` that you return from the route gets set as the `model` property on the controller by default.

### Auto-generation of Classes

Routes, controllers, and templates are all automatically generated for you based on URL if not defined.

For example, to have a plain template, you can just modify `app/router.js` to:

```javascript
Router.map(function() {
  this.route('bananas');
});
```

and add some markup to `app/templates/bananas.hbs` for it to be rendered when you visit `/bananas`.

Defining your own `App.Controller`, `App.ObjectController` or `App.ArrayController` allows you to customize the auto-generated controllers.

### Rendering Templates

A route handler is responsible for rendering templates.

By default, it renders the template with the same name.

You can customize this by implementing the `renderTemplate` method to either render a specific template or to use a different controller.

You can render multiple templates into multiple outlets.

### Redirection

`afterModel` and `beforeModel` hooks can be defined to be executed after/before the model is available.

`transitionTo('template')` acts as a redirect.

### Query Parameters

Example of using query parameters to filter in the controller (straight from [the guides](http://emberjs.com/guides/routing/query-params/)):

```javascript
App.ArticlesController = Ember.ArrayController.extend({
  queryParams: ['category'],
  category: null,

  filteredArticles: function() {
    var category = this.get('category');
    var articles = this.get('model');

    if (category) {
      return articles.filterBy('category', category);
    } else {
      return articles;
    }
  }.property('category', 'model')
});
```

And linking with the query param specified: `{{#link-to 'articles' (query-params category="cats")}}Articles about cats{{/link-to}}`

Query parameters support several options, but the one I anticipate using most frequently is telling Ember to actually fetch data from the server when a query param changes:

```javascript
queryParams: {
  category: {
    refreshModel: true
  }
}
```

### Loading and Error States

You can implement either a `LoadingRoute` or a `loading.hbs` template to be displayed while a transition is happening.

You can define namespace-specific ones. For example, `posts/loading.hbs` for when you're loading a post and `loading.hbs` as a generic one.

You can handle `loading` and `error` events by definining methods with corresponding names in your route (or the application route to define global behavior).

### Controllers

Controllers decorate your models with display logic. Their properties aren't persisted while models' properties are. You can think of it as a stateful presenter.

"Templates know about controllers and controllers know about models"

`ArrayController`s are used to decorate collections of data.

`ObjectController`s are used to decorate a single model.

You can specify an `itemController` in an `ArrayController` to decorate each item you're iterating over with a given `ObjectController`.

For a nested resource, you can specify that its controller `needs` its parent controller. This could make, for example, `controllers.post` available in a comments controller. This would allow you to get information about the post when you're viewing its comments. You can also make a property on the comments controller: `post: Ember.computed.alias("controllers.post")`.
