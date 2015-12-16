# Events

## _listeners_ Property

Event listeners can be applied to an element via the `listeners` property.
This property is an object that maps event names to event handler function names.

In the case where the event listener is targeted at the _host_,
the event name is just the _plain_ name of the event to listen for (e.g. _'click'_).

If the event listener is targeted at an element inside the _host_,
the event name is made up of the id of the targeted element, a dot,
and the _plain_ name of the event to listen for (e.g. _'bar.click'_).

Given this template.

```html
<template>
  <div id="bar"></div>
</template>
```

Event listeners can be applied like this.

```javascript
Polymer({
  is: 'foo-element',
  listeners: {
    'click': 'click',
    'bar.click': 'barClick'
  },
  click: function (e) {
    console.log('Click!', e);
  },
  barClick: function (e) {
    console.log('Bar Click!', e);
  }
});
```

## on-_event_ Annotations

Event listeners can be added directly (without assigning a _id_) to an element via on-_event_ annotations.

```html
<template>
  <div on-click="barClick"></div>
</template>
```

## Gesture Events

Polymer fires a custom _gesture_ events for certain user interactions
automatically when a declarative listener is added for the event type.
These events fire consistently on both touch and mouse environments,
so it is recommend to use these events instead of their
mouse- or touch-specific event counterparts (e.g. _tap_ instead of _click_).

The following are the gesture event types supported.

- _down_
- _up_
- _tap_
- _track_

## Event Retargeting

Shadow DOM has a feature called _'event retargeting'_
which changes an event’s target as it bubbles up,
such that target is always in the receiving element’s light DOM.
Shady DOM does not do event retargeting,
so events may behave differently depending on which local DOM system is in use.

To normalise the event (equivalent target data on both Shady DOM and Shadow DOM)
`Polymer.dom(event)` should be called.
This method takes a event as parameter and returns normalise version.

The normalised event has the following properties.

- _rootTarget_: The original or root target before shadow retargeting.
- _localTarget_: Retargeted event target.
- _path_: Array of nodes through which event will pass.
