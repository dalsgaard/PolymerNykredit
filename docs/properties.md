# Properties

Properties can be declared in the `properties` property.

In the simple syntax only the _name_ and the _type_ is declared.

```js
Polymer({
  is: 'foo-element',
  properties: {
  	bar: String,
  	baz: Number
  }
});
```

The _type_ is one of the following
`Boolean`, `Date`, `Number`, `String`, `Array` or `Object`.

Properties can be initialised through attributes.

```js
Polymer({
  is: 'foo-element',
  properties: {
  	bar: String,
  	baz: Number
  },
  ready: function () {
  	console.log(this.bar, this.baz);
  }
});
```

```html
<foo-element bar="1" baz="2"></foo-element>
```

## Configuration Object

In addition to the simple syntax properties can be configured via an object.
This object can have the following keys.

### _type_

This key corresponds to the value in the simple syntax.

```js
properties: {
  bar: {
  	type: String
  }
}
```

This key is mandatory.

### _value_

The default value for the property. The value for this key can either be
a boolean, number, string or function.
If the value is a function the return value vil be used as the default value.

```js
properties: {
  bar: {
  	type: String,
  	value: 'bar'
  },
  baz: {
  	type: Number,
  	value: function () {
  	  return 3;
  	}
  }
}
```

### _reflectToAttribute_

Boolean value determining whether or not the corresponding attribute value
should reflect this property.

```js
properties: {
  bar: {
  	type: String,
  	reflectToAttribute: true
  }
}
```

### _readOnly_

Boolean value determining whether or not this property is read-only.

```js
properties: {
  bar: {
  	type: String,
  	readOnly: true
  }
}
```

The read-only can be changed by calling the `_set<PropertyName>` method
on the element. In this case the `_setBar` method.

```js
ready: function () {
  this._setBar(5);
}
```

### _notify_

Boolean value determining whether or not the property
 can be used by the two-way data binding system.

```js
properties: {
  barBaz: {
  	type: String,
  	notify: true
  }
}
```

If notify is true an `<property-name>-changed` event will be fired
when the attribute is changed. In this case a `bar-baz-changed` event.

```js
foo.addEventListener('bar-baz-changed', function (e) {
  console.log(e);
});
```

### _computed_

String value interpreted as a method name and an argument list.
The method is called to calculate the value of the property.
If one or more of the arguments from the argument list changes,
the property value is recalculated.

```js
Polymer({
  is: 'foo-element',
  properties: {
  	firstName: String,
  	latName: String,
  	fullName: {
  	  type: String,
  	  computed: 'computeFullName(firstName, lastName)'
	}
  },
  computeFullName: function (firstName, lastName) {
  	return this.firstName + ' ' + this.lastName;
  }
});
```

The computed key can be used together with the notify key.

```js
properties: {
  firstName: String,
  latName: String,
  fullName: {
    type: String,
  	computed: 'computeFullName(firstName, lastName)',
  	notify: true
  }
}
```

### _observer_

String value interpreted as a method name.
The method is called whenever the value of the property changes.

```js
Polymer({
  is: 'foo-element',
  properties: {
  	bar: {
  	  type: Number,
  	  observer: 'barChanged'
	}
  },
  barChanged: function (newValue, oldValue) {
  	console.log('From ' + oldValue + ' to ' + newValue);
  }
});
```


## Observers

The `observers` array can be used to listen to multiple properties or sub-paths of a property.

### Observing Multiple Properties

This syntax is very much like computed properties
- a string value interpreted as a method name and an argument list.
The listener is called when one or more of the arguments from the argument list is changed,
and none of the arguments are undefined.

```js
Polymer({
  is: 'foo-element',
  properties: {
    bar: String,
    baz: String
  },
  observers: [
    'barOrBazChanged(bar, baz)'
  ],
  barOrBazChanged: function (bar, baz) {
    console.log(bar, baz);
  }
});
```

### Observing Property Sub-Paths

A 'dot' notation is used to observe a sub-path.

```js
Polymer({
  is: 'foo-element',
  properties: {
    user: {
      type: Object,
      value: function () {
        return {};
      }
    }
  },
  observers: [
    'emailChanged(user.email)'
  ],
  emailChanged: function (user) {
    console.log(user);
  }
});
```

The listener is called when the sub-path changes,
but only if it's `set` method is used (or notifyPath).

```js
foo.set('user.email', 'kim@kimdalsgaard.com');
```
