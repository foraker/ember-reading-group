# Week 6: Testing

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

### Example Integration Test

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

### Example Unit Test

For the model, `app/models/post.js`, which looks like this:

```javascript
import DS from 'ember-data';

var attr = DS.attr;

export default DS.Model.extend({
  title: attr('string')
  uppercaseTitle: function() {
    return this.get('title').toUpperCase();
  }.property('title')
});
```

You could add a test that looks like this in `tests/unit/models/post-test.js`:

```javascript
import {
  moduleForModel,
  test
} from 'ember-qunit';
import Ember from 'ember';

moduleForModel('post', 'Post', {
  // Specify the other units that are required for this test.
  // needs: []
});

test('uppercaseTitle returns the uppercase title', function() {
  var model = this.subject();
  Ember.run(function() {
    model.set('title', 'i dunno some post title i guess');
    equal(model.get('uppercaseTitle'), 'I DUNNO SOME POST TITLE I GUESS');
  });
});
```

Without the `Ember.run`, it complained about asychronous tests when the run loop was disabled due to being in the test environment.


## Assignments

### Testing Guide

:book: http://emberjs.com/guides/testing/

Read the whole section of the Ember Guides on Testing.
