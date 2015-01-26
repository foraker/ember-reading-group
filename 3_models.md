# Week 3: Models

## Assignments

### Ember Data Model Maker

:rowboat: http://andycrum.github.io/ember-data-model-maker/

Play with model definitions and see difference between `DS.RestAdapter` and `DS.ActiveModelAdapter`

### The Object Model Guide

:book: http://emberjs.com/guides/object-model/classes-and-instances/

Read the whole "The Object Model" section of the Ember Guides.

### Ember Models Guide

:book: http://emberjs.com/guides/models/

Read the whole "Models" section of the Ember Guides. There's a lot of material here but take your time.

## Bonus Point Assignments

### RSVP.js Promises

:book: https://github.com/tildeio/rsvp.js

RSVP.js is the library Ember uses for promises. Read the README, particularly if you need a refresher on promises in general.

### Modeling the App Store and iTunes with Ember Data

:tv: https://www.youtube.com/watch?v=i1_aBX8web4

Presentation from EmberConf 2014. Dude builds an Ember App on top of the App Store and other APIs. Shows how powerful custom adapters can be.

## Notes

### Ember Data

Ember Data adds a `store` attribute to all your routes and controllers that is shared between them. You talk to it to get or persist data. It does automatic caching.

The store returns a promise from `find()` that resolves with the record.

### Ember CLI

The Ember Guides largely assume that you're using a global namespace. Using Ember CLI, you won't be setting `App.Person`, for example. Instead, you'll be `export`ing from `app/models/person.js`. [Here is an example to illustrate the difference with a bonus model definition in CoffeeScript.](https://gist.github.com/artfuldodger/6a9e1170a08b35583d43)

### Computed Properties

[Computed properties](http://emberjs.com/guides/object-model/computed-properties/) are defined to be calculated based on basic attributes and are updated when one of the attributes they depend on are updated.

Here's an example model definition:

```javascript
import DS from 'ember-data';

var attr = DS.attr;

var Person = DS.Model.extend({
  firstName: attr('string'),
  lastName: attr('string'),
  verified: attr('boolean', { defaultValue: false} ),
  createdAt: attr('string', {
    defaultValue: function() { return new Date(); }
  }),
  fullName: function() {
    return this.get('firstName') + ' ' + this.get('lastName');
  }.property('firstName', 'lastName')
});

export default Person;
```

Things to remember:
- The first parameter to `attr` is an optional type that will coerce what is returned from the server to the specified type. Without it, the type returned from the server will be used.
- The optional second parameter defines a default value either as a value or a function.
- A computed property is defined by calling `.property` from a function that defines what properties it is calculated from.

### Relationships

Relationships are defined as associations with `DS.belongsTo` and `DS.hasMany` (as opposed to `DS.attr`). How familiar!

If a relationship pair is ambiguous, you can specify which one you're matching by specifying the `inverse`.

### Persistence

`record.save()` persists the record. It returns a promise.

`store.createRecord()` creates things.

`record.deleteRecord()` marks it as deleted but it's not actually deleted until `save` is called.

`record.destroyRecord()` *really* deletes it.

`store.push()` allows you to push stuff into the store ahead of time allowing for clever super-eager-loading optimizations.

### Querying

`store.find('post')` fetches all record of type Post. It returns a `DS.PromiseArray` that fulfills to a `DS.RecordArray`.

`store.all('post')` returns all posts already in the store.

`store.find('post', 42)` fetches post with id 42.

`store.find('post', { title: 'Ember' })` fetches posts with title "Ember" (`GET /posts?title=Ember`)

### Modifying Attributes

`record.set('name', 'George')`

`record.incrementProperty('age')`

`record.get('name')`

`record.isDirty()` to check if it has unpersisted changes.

`record.changedProperties()` gives you an object where the keys are the changed properties and each value is of the form `[oldValue, newValue]`.

`record.rollback()` reverts unpersisted changes.

### A Note on JSON for Relationships

Relationships are represented as ids.

It's recommended to "sideload" your relationships to reduce number of HTTP requests required. Using the default RESTAdapter, that looks like:

```json
{
  "post": {
    "id": 1,
    "title": "Ember guides love the word omakase",
    "comments": [1, 2]
  },

  "comments": [{
    "id": 1,
    "body": "But what _is_ omakase?"
  },
  {
    "id": 2,
    "body": "Nerds, man."
  }]
}
```

Also to hang out with Rails, you might want to switch out the default RESTAdapter for the more Rails-friendly [ActiveModelAdapter](http://emberjs.com/api/data/classes/DS.ActiveModelAdapter.html). You would make your `app/adapters/application.js` look like this:

```javascript
import DS from 'ember-data';

export default DS.ActiveModelAdapter.extend({});
```

That's also where you could define a `namespace` or `host` for your API.
