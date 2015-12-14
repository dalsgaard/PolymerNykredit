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
