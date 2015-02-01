# Week 4: Templates, Views, and Components

## Assignments

### Templates Guide

:book: http://emberjs.com/guides/templates/the-application-template/

Introduction to defining templates in Ember.

### Views Guide

:book: http://emberjs.com/guides/views/

Read the whole section of the Ember Guides on Views.

### Components Guide

:book: http://emberjs.com/guides/components/

Read the whole section of the Ember Guides on Components.

## Bonus Point Assignments

### HTMLBars

:book: http://colintoh.com/blog/htmlbars

Talks about what's upcoming as Ember transitions from Handlebars to HTMLbars.

### Awesome Ember.js Form Components

:book: http://alexspeller.com/simple-forms-with-ember/

Deals with using components to beautify your forms.

### New Component Patterns

:tv: https://www.youtube.com/watch?v=nvB8iAwc2QQ#t=6574

An excerpt from the NYC meetup, this talk introduces the approach to components that Ember will be taking as it moves toward 2.0. Touches on some things that will be introduced in the forthcoming 1.10 release.

### Using Ember to Make the Seemingly Impossible Easy

:tv: https://www.youtube.com/watch?v=JC8TOXe8TYM

The talk deals with solving several problems with clever uses of Ember. The relevant part deals with using Ember components to encapsulate D3 widgets starting at 7:40. The whole video's worth watching though, if you can deal with the feedback.

### Custom Elements

:book: http://www.html5rocks.com/en/tutorials/webcomponents/customelements/

This guide walks through using the [W3 Spec on Custom Elements](http://w3c.github.io/webcomponents/spec/custom/) to define custom elements. The point is mostly to be aware that this is a spec that's coming at some point in the future and Ember's trying to be compatible in regard to components.

## Notes

### Templates

You can match an `else` with an `each`:

```handlebars
{{#each person in people}}
  Hello, {{person.name}}!
{{else}}
  Sorry, nobody is here.
{{/each}}
```

`bind-attr` binds an HTML element's attribute to the controller. It can be a string, e.g. `<img {{bind-attr src=logoUrl}}>` or a boolean which toggles the attribute's presence, e.g. `<input type="checkbox" {{bind-attr disabled=isAdministrator}}>`. `bind-attr class=` does [magic things](http://emberjs.com/guides/templates/binding-element-class-names/).

`link-to` works both in a `{{#link-to 'index'}}block form{{/link-to}}` and an `{{#link-to 'inline form' 'index'}}`

`{{action}}` ties an element to firing an event that is handled by the controller (or the route). You can pass parameters and tie it to an event other than click, e.g. `<button {{action "select" post on="mouseUp"}}>Select</button>`.

`{{input}}` and `{{textarea}}` that make it easy to bind attributes, e.g. `{{input value=firstName disabled=entryNotAllowed size="50"}}`

`{{log "Name: " name}}` and `{{debugger}}` can help debug during development.

`{{partial "something"}}` renders the template in place without changing the context.

`{{render "author" author}}` renders the author template using the author controller.

For an example custom helper, [see the definition of `format-date`](https://github.com/artfuldodger/ember-blog/blob/master/app/helpers/format-date.js).

## Views

You probably want to define components instead of views. See the "Routeable Components" section of [The Road to Ember 2.0 RFC](https://github.com/emberjs/rfcs/pull/15).

Do read the Guide on Views, but I won't be including many notes here.

The built-in views, [Ember.Checkbox](http://emberjs.com/api/classes/Ember.Checkbox.html), [Ember.TextField](http://emberjs.com/api/classes/Ember.TextField.html), [Ember.TextArea](http://emberjs.com/api/classes/Ember.TextArea.html), and [Ember.Select](http://emberjs.com/api/classes/Ember.Select.html) are primarily interacted with using the Handlebars helper but can be extended to customize form behavior.

## Components

Allow us to define our own application-specific HTML tags.

Put a dash in your component's name.

Just defining the handlebars template in `app/templates/components` makes it available as a component without writing any JavaScript.

Behavior is defined by subclassing [Ember.Component](http://emberjs.com/api/classes/Ember.Component.html). You'll put that in, for example, `app/components/some-stuff-component.js` to correspond with `app/templates/components/some-stuff.hbs`.

Components need to be passed properties; they don't have access to the same properties as the template. Properties passed to the component are bound.

Components can be passed a block and output it with `{{yield}}`.

To specify an element tag name other than `div`, give your `Component` subclass a `tagName` property.

The `classNames` property is an array of class names your component's element will get.

Boolean properties determine if a class name matching the property's name is added to the component's element or not.

Actions triggered from within your component template are handled by your `Component` subclass. If you want to send that action up to the controller/router, call `this.sendAction()` from your Component.

Component actions should largely be used to translate primitive events into events that make sense within the context of your application.
