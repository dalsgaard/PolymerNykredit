# Data Binding

## One-Way Binding

Polymer supports one-way binding in the template via the _[[\<name\>]]_ syntax.
The context for the resolving of the binding is the element.

```html
<dom-module id="foo-element">
  <template>
    <div class="bar">[[bar]]</div>
    <button on-tap="step">Step</button>
  </template>
  <script>
    Polymer({
      is: 'foo-element',
      properties: {
        bar: {
          type: Number,
          value: 0
        }
      },
      step: function () {
        this.bar++;
      }
    });
  </script>
</dom-module>
```

## Auto-binding

```html
<body>
  <template is="dom-bind">
    <foo-element bar="[[bar]]"></foo-element>
  </template>
  <script>
    var t = document.querySelector('template[is="dom-bind"]');
    t.addEventListener('dom-change', function() {
      var foo = document.querySelector('foo-element');
    });
  </script>
</body>
```

## Two-Way Binding

Polymer also supports two-way binding in the template via the _{{\<name\>}}_ syntax.

To enable two-way data binding for a property it will have to _notify_ when it changes.

```javascript
properties: {
  bar: {
    type: Number,
    value: 0,
    notify: true
  }
}
```

And then it is possible to _bind_ two or more properties together.

```html
<template is="dom-bind">
  <foo-element bar="{{bar}}"></foo-element>
  <foo-element bar="{{bar}}"></foo-element>
</template>
```

### Two-way binding to native elements

To bind to native properties it is necessary to provide the event
that is fired when the property changes.

```html
<template>
  <div class="bar">[[bar]]</div>
  <button on-tap="step">Step</button>
  <input type="range" value="{{bar::change}}"/>
</template>
```

## Binding to Structured Data

It is possible to bind to sub-properties
by specifying a path inside the binding annotation.

```html
<dom-module id="foo-element">
  <template>
    <div>
      <span>[[user.firstName]]</span>
      <span>[[user.lastName]]</span>
    </div>
    <input type="text" value="{{user.firstName::change}}"/>
    <input type="text" value="{{user.lastName::change}}"/>
  </template>
  <script>
    Polymer({
      is: 'foo-element',
      properties: {
        user: {
          type: Object,
          value: function () { return {}; },
          notify: true
        }
      }
    });
  </script>
</dom-module>
```

## Computed bindings

Computed bindings is much like _computed properties_.

```html
<dom-module id="foo-element">
  <template>
    <div>{{fullName(firstName, lastName)}}</div>
    <input type="text" value="{{firstName::change}}"/>
    <input type="text" value="{{lastName::change}}"/>
  </template>
  <script>
    Polymer({
      is: 'foo-element',
      properties: {
        firstName: String,
        lastName: String
      },
      fullName: function (firstName, lastName) {
        return firstName + ' ' + lastName;
      }
    });
  </script>
</dom-module>
```

### _'One shot'_ bindings

If the computed binding has no dependencies the binding is computed only once.

```html
<dom-module id="foo-element">
  <template>
    <div>{{time()}}</div>
  </template>
  <script>
    Polymer({
      is: 'foo-element',
      time: function () {
        return new Date().toString();
      }
    });
  </script>
</dom-module>
```

## Repeat Templates

Polymer supports a repeating template.
For every item in the _items_ a new instance of the template is created.

```html
<dom-module id="foo-element">
  <template>
    <ul>
      <template is="dom-repeat" items="{{basket}}">
        <li>
          <span class="name">[[item.name]]</span> :
          <span class="quantity">[[item.quantity]]</span>
        </li>
      </template>
    </ul>
  </template>
  <script>
    Polymer({
      is: 'foo-element',
      ready: function () {
        this.basket = [
          { name: 'Foo', quantity: 0 },
          { name: 'Bar', quantity: 0 },
          { name: 'Baz', quantity: 0 }
        ]
      }
    });
  </script>
</dom-module>
```

Events _targeting_ elements inside the repeat template contains a context
like the one present when the template instance was created.
The context is the property _model_.
If the _item_ is changed via the _model_'s _set_ method data binding will work.

```html
<dom-module id="foo-element">
  <template>
    <ul>
      <template is="dom-repeat" items="{{basket}}">
        <li>
          <span class="name">[[item.name]]</span> :
          <span class="quantity">[[item.quantity]]</span>
          <button on-tap="decrease">-</button>
          <button on-tap="increase">+</button>
        </li>
      </template>
    </ul>
  </template>
  <script>
    Polymer({
      is: 'foo-element',
      ready: function () {
        this.basket = [
          { name: 'Foo', quantity: 0 },
          { name: 'Bar', quantity: 0 },
          { name: 'Baz', quantity: 0 }
        ]
      },
      increase: function (e) {
        e.model.set('item.quantity', e.model.item.quantity + 1);
      },
      decrease: function (e) {
        e.model.set('item.quantity', e.model.item.quantity - 1);
      }
    });
  </script>
</dom-module>
```

## Conditional Templates

Polymer has a conditional template that renders
depending on whether or not a value is falsy.

```html
<dom-module id="foo-element">
  <template>
    <input type="checkbox" checked="{{show::change}}"/>
    <label>Show</label>
    <template is="dom-if" if="{{show}}">
      <div>Ta-da!</div>
    </template>
  </template>
  <script>
    Polymer({
      is: 'foo-element'
    });
  </script>
</dom-module>
```
