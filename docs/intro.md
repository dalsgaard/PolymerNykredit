# Introduction to Polymer

## Creating an Element

```html
<link rel="import" href="./components/polymer/polymer.html">
<dom-module id="foo-bar">
  <template>
    <style>
      :host {
        display: block;
      }
    </style>
    <h2>Polymer@Nykredit</h2>
  </template>
  <script>
    Polymer({
      is: 'foo-bar'
    });
  </script>
</dom-module>
```

### Importing Polymer

```html
<link rel="import" href="./components/polymer/polymer.html">
```

### The Element

```html
<dom-module id="foo-bar">
  <template>
    <!-- ... -->
  </template>
  <script>
    <!-- ... -->
  </script>
</dom-module>
```

### The Template

```html
<template>
  <style>
    :host {
      display: block;
    }
  </style>
  <h2>Polymer@Nykredit</h2>
</template>
```

### Registering the Element

```js
Polymer({
  is: 'foo-bar'
});
```

#### Creating a constructor

The `Polymer` function will return a constructor for the newly registered element.
Script tags inside the `dom-module` tag will be executed in global scoop
(like all other script tags)
- so we can save a reference to the constructor like this.

```js
var FooBar = Polymer({
  is: 'foo-bar'
});
```

#### Passing arguments to the constructor

Sometimes it is convenient to pass arguments to an elements constructor.
This can be done by implementing the `factoryImpl` method.

```javascript
var FooBar = Polymer({
  is: 'foo-bar',
  factoryImpl: function(bar) {
  	this.bar = bar;
  }
});
```

## Using the Element

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Foo</title>
    <script src="components/webcomponentsjs/webcomponents-lite.min.js"></script>
    <link rel="import" href="foo-bar.html">
  </head>
  <body>
    <foo-bar></foo-bar>
    <script>
      window.addEventListener('WebComponentsReady', function() {
        var fooBar = document.querySelector('foo-bar');
      });
    </script>
  </body>
</html>
```

### Including the `webcomponentsjs` Script

```html
<script src="components/webcomponentsjs/webcomponents-lite.min.js"></script>
```

### Importing the Element

```html
<link rel="import" href="foo-bar.html">
```

### Using the Element

```html
<foo-bar></foo-bar>
```

### Listening for _WebComponentsReady_

```js
window.addEventListener('WebComponentsReady', function() {
  var fooBar = document.querySelector('foo-bar');
});
```

## Extending Native Elements

A native element is extended by setting the `extends` property on the prototype.

```js
Polymer({
  is: 'foo-input',
  extends: 'input'
  }
});
```

To use the extended version of the element set the `is` attribute on the element.

```html
<input is="foo-input">
```
or create the element in JavaScript via the DOM API.

```javascript
var foo = window.document.createElement('input', 'foo-input');
```
