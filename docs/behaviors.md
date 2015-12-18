# Behaviors

- Lifecycle callbacks
- Properties
- Defaults attributes
- Observers,
- Listeners

### Using a Behavior

```js
Polymer({
  is: 'foo-bar',
  behaviors: [BazBehavior]
});
```

### Creating a Behavior

```js
var BazBehavior = {
  properties: {
    foo: String,
    bar: Number
  },
  listeners: {
    click: '_toggle'
  },
  created: function () {
    // Do stuff
  },
  _toggle: function () {
    // Do stuff
  },
  baz: function () {
    // Do stuff
  }
};
```

### Extending Behaviors

```js
FooBehavior = [ BarBehavior, BazBehavior ];
```
