# Style

### Shady DOM versus Shadow DOM

Shadow DOM is still a very new technology
and not all evergreen browsers implements the specification yet.
Because it is very difficult and computation heavy to implement it as a polyfill
the Polymer project has developed an alternative called Shady DOM.

Shady DOM is the default, but Shadow DOM can be enabled by setting a flag i JavaScript

```html
<script src="components/webcomponentsjs/webcomponents-lite.js"></script>
<script>
  window.Polymer = window.Polymer || {};
  window.Polymer.dom = 'shadow';
</script>
```

or via the URL

```
http://foo.bar/index.html?dom=shadow
```

Shady DOM behaves very much like Shadow DOM, but there is still some differences
- some of these differences will be described below.

## Local Style

Local styling is done in a _style_ tag inside the _dom-module_ tag.
The host element can be styled via the `:host` selector.

```html
<dom-module id="foo-element">
  <style>
    :host {
      border: solid 1px black;
      display: block;
    }
  </style>
  <template>
    <h1>
      <content></content>
    </h1>    
  </template>
  <script>
    Polymer({
      is: 'foo-element'
    });
  </script>
</dom-module>
```

Normal styling rules inside the _style_ tag only applies to the Shadow DOM elements.

```html
<style>
  :host {
    border: solid 1px black;
    display: block;
  }
  span {
    color: blue;
  }
</style>
```

When it comes to styling the content inserted via the _content_ tag
there is a difference between the Shadow DOM and the (default) Shady DOM.
Given the following setup.

```html
<style>
  :host {
    display: block;
  }
  ::content span {
    color: blue;
  }
</style>
```

```html
<template>
  <content></content>
  <span>Baz</span>
</template>
```

```html
<foo-element>
  <span>Foo</span>
  <span>Bar</span>
</foo-element>
```

Shady DOM will render all _span_ tags in a blue color
while Shadow DOM will render only the _span_ tags from the Light DOM in a blue color.

The recommended solution is to wrap the _content_ tag like this.

```html
<style>
  :host {
    display: block;
  }
  .content > ::content span {
    color: blue;
  }
</style>
```

```html
<template>
  <span class="content"><content></content></span>
  <span>Baz</span>
</template>
```

## Style from _'the outside'_

Under the Shady DOM styling from the main document will _leak_ into the Polymer elements.
To prevent this, styling can be placed in a _style_ tag that is extended with _custom-style_.

```html
<head>
  <meta charset="UTF-8">
  <title>Intro</title>
  <script src="components/webcomponentsjs/webcomponents-lite.min.js"></script>
  <link rel="import" href="components/polymer/polymer.html">
  <link rel="import" href="foo-element.html">
  <style is="custom-style">
    span {
      background-color: yellow;
    }
  </style>
</head>
```

## CSS Variables

To make it easier to customise elements Polymer supports CSS Variables.

First the CSS variable will be _consumed_ inside an element.

```html
<style>
  span {
    color: var(--foo-element-color);
  }
</style>
```

And then the CSS variable can be given a value outside of the element.

```html
<style is="custom-style">
  foo-element {
    --foo-element-color: blue;
  }
</style>
```

Variables can be given a default value.

```css
color: var(--foo-element-color, green);
```

## Mix-ins

In addition to CSS variables there is a mix-in feature
that makes it possible to set more values in one go.

First the mix-in will be applied inside an element.

```html
<style>
  span {
    @apply(--foo-element-theme);
  }
</style>
```

And then the mix-in can be given some values outside of the element.

```html
<style is="custom-style">
  foo-element {
    --foo-element-theme: {
      color: yellow;
      background-color: blue;
    }
  }
</style>
```

## Shared styles

It is possible to share style across multiple elements.

First the shared style must be defined inside a _dom-module_ _template_.

```html
<link rel="import" href="components/polymer/polymer.html">
<dom-module id="foo-shared-styles">
  <template>
    <style>
      span {
        color: red;
      }
    </style>
  </template>
</dom-module>
```

And then the styles must be included into another element.

```html
<link rel="import" href="components/polymer/polymer.html">
<link rel="import" href="foo-shared-styles.html">
<dom-module id="foo-element">
  <style include="foo-shared-styles"></style>
  <template>
    <span>Baz</span>
  </template>
  <script>
    Polymer({
      is: 'foo-element'
    });
  </script>
</dom-module>
```

The same _style_ tag can both _include_ and _define_ styles.

```html
<style include="foo-shared-styles">
  span {
    background-color: yellow;
  }
</style>
```

Shared styles can also be included outside of elements in _style_ tags
extended by _custom-style_.

```html
<style is="custom-style" include="foo-shared-styles">
  span {
    background-color: yellow;
  }
</style>
```
