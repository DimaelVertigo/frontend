---
description: Polyfill for Web Components
---

# Polymer



Key information:

* The `<dom-module>` tag wraps an element's shadow DOM definition. In this case, the `id` attribute shows that this module includes the shadow DOM for an element called `icon-toggle`.
* The `<template>` actually defines the element's shadow DOM structure and styling. This is where you'll add markup for your custom element.
* The `<style>` element inside the `<template>` lets you define styles that are _scoped_ to the shadow DOM, so they don't affect the rest of the document.
* The `:host` pseudo-class matches the custom element you're defining \(in this case, the `<icon-toggle>`\). This is the element that contains or _hosts_ the shadow DOM tree.
* Polymer uses ES6 class syntax. With this code, you extend the base Polymer.Element class to create your own:

  ```javascript
  class IconToggle extends Polymer.Element {...}
  ```

* You then give your new element a name, so that the browser can recognize it when you use it in tags. This name must match the `id` given in your element's template definition \(`<dom-module id="icon-toggle">`\).

  ```javascript
  static get is() {
    return "icon-toggle";
  }
  ```

* The element has a constructor:

  ```javascript
  constructor() {
    super();
  }
  ```

  At the moment, this constructor does nothing. It is included here as a placeholder since we'll use it later.

* At the end of the script, this line calls the "define" method from the Custom Elements API to register your element:

  ```javascript
  customElements.define(IconToggle.is, IconToggle);
  ```

