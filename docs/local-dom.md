#Local DOM

The _template_ tag inside the _dom-module_ tag is automatically _'stamped out'_
when a new element is created.

```html
<dom-module id="foo-element">
  <template>
  	<h1> Foo Element</h1>
  </template>
  <script>
    Polymer({
      is: 'foo-element'
    });
  </script>
</dom-module>
```

### Shadow DOM and Light DOM

#### Shadow DOM

The Shadow DOM is the DOM tree inside the elements template.

#### Light DOM

The Light DOM is the DOM tree inside the element tag.

## Selectors

### id selector

Elements with an _id_ inside the local DOM can be found via the `$` hash.

```html
<dom-module id="foo-element">
  <template>
  	<h1 id="header"></h1>
  </template>
  <script>
    Polymer({
      is: 'foo-element',
      ready: function () {
        this.$.header.textContent = 'Foo Element';
      }
    });
  </script>
</dom-module>
```

### CSS selector

Elements inside the local DOM can be selected via the `$$` method.
The method takes a CSS selector string as the only argument,
and returns the first element that matches the selector.

```html
<dom-module id="foo-element">
  <template>
  	<h1></h1>
  </template>
  <script>
    Polymer({
      is: 'foo-element',
      ready: function () {
        this.$$('h1').textContent = 'Foo Element';
      }
    });
  </script>
</dom-module>
```

## Content

Content from inside the element can be inserted inside the local DOM
via the _content_ tag.

```html
<template>
  <h1>
    <content></content>
  </h1>
</template>
```

```html
<foo-element>
  <span>Foo</span>
  <span>Bar</span>
</foo-element>
```

Special content can be selected via the _select_ attribute.

```html
<template>
  <h1>
    <content select=".header"></content>
  </h1>
  <section>
    <content></content>
  </section>
</template>
```

```html
<foo-element>
  <span class="header">Foo</span>
  <span>Bar</span>
</foo-element>
```

## DOM API

To maintain the proper separation between the Shadow DOM and the Light DOM
it is important to use the Polymer _dom_ methods when manipulating
and querying both DOM trees inside the element.

These methods is mimicking the normal DOM API as seen in the following examples.

```javascript
parent.appendChild(child);
// ->
Polymer.dom(parent).appendChild(child);
```

```javascript
parent.insertBefore(child, before);
// ->
Polymer.dom(parent).insertBefore(child, before);
```

To reference the Light DOM use `this`
and to reference the Shadow DOM use `this.root`.

```javascript
<dom-module id="foo-element">
  <template>
    <h1>
      <content></content>
    </h1>
  </template>
  <script>
    Polymer({
      is: 'foo-element',
      ready: function () {
        var light = document.createElement('span');
        light.textContent = 'Into the Light';
        var shadow = document.createElement('span');
        shadow.textContent = 'Into the Shadow';
        Polymer.dom(this).appendChild(light);
        Polymer.dom(this.root).appendChild(shadow);
      }
    });
  </script>
</dom-module>

```

This is a list of all the _dom_ methods

- `Polymer.dom(parent).appendChild(child)`
- `Polymer.dom(parent).insertBefore(child, before)`
- `Polymer.dom(parent).removeChild(child)`
- `Polymer.dom.flush()`
- `Polymer.dom(parent).childNodes`
- `Polymer.dom(node).parentNode`
- `Polymer.dom(node).firstChild`
- `Polymer.dom(node).lastChild`
- `Polymer.dom(node).firstElementChild`
- `Polymer.dom(node).lastElementChild`
- `Polymer.dom(node).previousSibling`
- `Polymer.dom(node).nextSibling`
- `Polymer.dom(node).textContent`
- `Polymer.dom(node).innerHTML`
- `Polymer.dom(parent).querySelector(selector)`
- `Polymer.dom(parent).querySelectorAll(selector)`
- `Polymer.dom(contentElement).getDistributedNodes()`
- `Polymer.dom(node).getDestinationInsertionPoints()`
- `Polymer.dom(node).setAttribute(attribute, value)`
- `Polymer.dom(node).removeAttribute(attribute)`
- `Polymer.dom(node).classList`
