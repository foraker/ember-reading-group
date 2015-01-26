# Week 6: Testing

## Assignments

### Testing Guide

:book: http://emberjs.com/guides/testing/

Read the whole section of the Ember Guides on Testing.

### Test-Driving Ember

:tv: http://blog.unspace.ca/post/107903324912/cory-forsyth-on-test-driving-ember

Introduction to testing in Ember that touches on the internals.

## Bonus Point Assignments

### Ember Data Model Tests

:book: https://github.com/emberjs/data/blob/master/packages/ember-data/tests/unit/model_test.js

Reading the tests for Ember Data Model shows a variety of unit tests and will probably teach you something new about models as a bonus.

### QUnit Cookbook

:book: http://qunitjs.com/cookbook/

A good (and relatively short) introduction to QUnit's abilities.

## Notes

[QUnit](http://qunitjs.com/) is the default testing framework for Ember.

When you generate something with Ember CLI, it will come a corresponding unit test. Ember CLI includes the relevant setup allowing you to start writing tests immediately. The generated tests just assert that the thing you generated exists.

Run your tests by going to http://localhost:4200/tests.

### Ember-included Helpers

#### Asynchronous

These wait for asynchronous stuff to finish.

`visit(url)`

`fillIn(selector, text)`

`click(selector)`

`keyEvent(selector, type, keyCode)` where `type` is `keypress`, `keydown`, or `keyup`. The keycode for enter is 13. The Internet has several options for listing all the others.

`triggerEvent(selector, type, options)` where examples of `type` include `blur` and `dblclick`.

#### Synchronous

These trigger immediately.

`find(selector, context)`

`currentPath()`

`currentRouteName()`

`currentURL()`

`andThen(function() { ... })` will wait until after preceding asynchronous calls are finished before the function is executed.

### Custom Test Helpers

You can write your own helpers with `Ember.Test.registerHelper` and `Ember.Test.registerAsyncHelper`. Here's an example from the Guides:

```javascript
Ember.Test.registerHelper('shouldHaveElementWithCount',
function(app, selector, n, context) {
  var el = findWithAssert(selector, context);
  var count = el.length;
  equal(n, count, 'found ' + count + ' times');
}
);
```

### Examples

These are mostly pulled from the Guides and provided here for reference.

#### Model Test

This is the only example where we'll use CoffeeScript and be in the context of an Ember CLI app (at least until I actually write some of the other varieties of tests and update this. For now, they're guide copypasta. Feel free to PR your own!)

Note the use of `moduleForModel` for Ember Data models.

For the model, `app/models/instructor.coffee`, which looks like this:

```coffeescript
`import DS from 'ember-data'`
`import Ember from 'ember'`

Instructor = DS.Model.extend {
  firstName: DS.attr('string'),
  lastName: DS.attr('string'),
  fullName: Ember.computed 'firstName', 'lastName', ->
    @get('firstName') + ' ' + @get('lastName')
}

`export default Instructor`
```

You could add a test that looks like this in `tests/unit/models/instructor-test.coffee`:

```coffeescript
`import { test, moduleForModel } from 'ember-qunit'`

moduleForModel 'instructor', 'Instructor', {
  needs: []
}

test 'it computes its full name', ->
  model = @subject(firstName: 'George', lastName: 'Baloney')
  equal(model.get('fullName'), 'George Baloney')

  ```

#### Integration Test

```javascript
test('simple test', function() {
  expect(1); // Ensure that we will perform one assertion

  visit('/posts/new');
  fillIn('input.title', 'My new post');
  click('button.submit');

  // Wait for asynchronous helpers above to complete
  andThen(function() {
    equal(find('ul.posts li:last').text(), 'My new post');
  });
});
```

#### Component Test

Will be set up using `moduleForComponent`.

Implementation:

```javascript
App.PrettyColorComponent = Ember.Component.extend({
  classNames: ['pretty-color'],
  attributeBindings: ['style'],
  style: function() {
    return 'color: ' + this.get('name') + ';';
  }.property('name')
});
```

Testing color changes updates rendered HTML's style attribute:

```javascript
test('changing colors', function(){
  var component = this.subject();

  // we wrap this with Ember.run because it is an async function
  Ember.run(function() {
    component.set('name','red');
  });

  // first call to $() renders the component.
  equal(this.$().attr('style'), 'color: red;');

  // another async function, so we need to wrap it with Ember.run
  Ember.run(function(){
    component.set('name', 'green');
  });

  equal(this.$().attr('style'), 'color: green;');
});
```

Testing template is rendered properly:

```javascript
test('template is rendered with the color name', function(){

  var component = this.subject();

  // first call to $() renders the component.
  equal($.trim(this.$().text()), 'Pretty Color:');

  // we wrap this with Ember.run because it is an async function
  Ember.run(function(){
    component.set('name', 'green');
  });

  equal($.trim(this.$().text()), 'Pretty Color: green');
});
```

#### Controller Test

Implementation:

```javascript
App.PostsController = Ember.ArrayController.extend({
  propA: 'You need to write tests',
  propB: 'And write one for me too',

  setPropB: function(str) {
    this.set('propB', str);
  },

  actions: {
    setProps: function(str) {
      this.set('propA', 'Testing is cool');
      this.setPropB(str);
    }
  }
});
```

Test:

```javascript
test('calling the action setProps updates props A and B', function() {
  expect(4);

  // get the controller instance
  var ctrl = this.subject();

  // check the properties before the action is triggered
  equal(ctrl.get('propA'), 'You need to write tests');
  equal(ctrl.get('propB'), 'And write one for me too');

  // trigger the action on the controller with the `send` method,
  // passing in params for the action
  ctrl.send('setProps', 'Testing Rocks!');

  equal(ctrl.get('propA'), 'Testing is cool');
  equal(ctrl.get('propB'), 'Testing Rocks!');
});
```

#### Route Test

May usually be better served via integration tests.

Implementation:

```javascript
App.ApplicationRoute = Em.Route.extend({
  actions: {
    displayAlert: function(text) {
      this._displayAlert(text);
    }
  },

  _displayAlert: function(text) {
    alert(text);
  }
});
```

Test:

```javascript
moduleFor('route:application', 'Unit: route/application', {
  setup: function() {
    originalAlert = window.alert;
  },
  teardown: function() {
    window.alert = originalAlert;
  }
});

test('Alert is called on displayAlert', function() {
  expect(1);

  var route = this.subject(),
    expectedText = 'foo';

  window.alert = function(text) {
    equal(text, expectedText,
      'expected ' + text + ' to be ' + expectedText);
  };

  route._displayAlert(expectedText);
});
```
