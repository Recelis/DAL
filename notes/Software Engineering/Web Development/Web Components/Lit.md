# Lit : Frontend Frameworks, Web Components

[docs](https://lit.dev/docs/components/overview/)

## Dependencies

This is a library that is built on top of the `native Web Components` and uses standard browser APIs.

They are similar to other front-end frameworks with what they have, except that they have scoped css styles built in.

```typescript
import { LitElement, css, html } from "lit";
import { customElement, property } from "lit/decorators.js";

@customElement("simple-greeting") // defines your custom element using the @customElement decorator.
export class SimpleGreeting extends LitElement {
  // Define scoped styles right with your component, in plain CSS
  static styles = css`
    :host {
      color: blue;
    }
  `;

  // Declare reactive properties
  @property()
  name?: string = "World";

  // Render the UI using `HTML Templates`
  render() {
    return html`<p>Hello, ${this.name}!</p>`;
  }
}
```

Which converts it to:

```HTML
<simple-greeting name="Markup"></simple-greeting>
```

This is scoped to the `custom element`, and everything inside is called the `Shadow DOM`.

The JavaScript that the Lit components converts to is:

```javascript
const greeting = document.createElement("simple-greeting");
```

The LitElement base class is a subclass of HTMLElement. It actually inherits from ReactiveElement which inherits from HTMLElement.

### Typescript typings

TypeScript will infer the class and all of its properties given the HTML element.

```typescript
@customElement("my-element")
export class MyElement extends LitElement {
  @property({ type: Number })
  aNumber: number = 5;
  /* ... */
}

declare global {
  interface HTMLElementTagNameMap {
    "my-element": MyElement;
  }
}
```

i.e. now it'll expect any element of "my-element" type will have "aNumber" as a property.

## Implementations
### Rendering

[docs](https://lit.dev/docs/components/rendering/)

The render function returns your template. This template will be given a Lit's html tag function.

```typescript
import { LitElement, html } from "lit";
import { customElement } from "lit/decorators.js";

@customElement("my-element")
class MyElement extends LitElement {
  render() {
    return html`<p>Hello from my template.</p>`;
  }
}
```

You shouldn't change your component state, produce side effects in your template. Your template should be pure to your component's properties.

### Composure
You can compose templates made from other templates too.

```typescript
render() {
    return html`
      ${this.headerTemplate()}
      ${this.articleTemplate()}
      ${this.footerTemplate()}
    `;
  }
```

The templates are initially rendered when they are added to the DOM. The component is actually encapsulated in the `Shadow DOM` which is part of the browser spec now for `custom elements`.

### Props

Or known as `Public reactive properties`

The public properties should not be changed by the component itself and are treated as inputs to the component. You use the @property decorator with class field declaration for your public reactive properties.

```typescript
class MyElement extends LitElement {
  @property({ type: String })
  mode?: string;

  @property({ attribute: false })
  data = {};
}
```

#### Declaring properties in a static properties class field

If you want a static properties class field. You cannot use class fields when declaring reactive properties.

```typescript
class MyElement extends LitElement {
  static properties = { foo: { type: String } };
  foo = "Default"; // ❌ this will make `foo` not reactive
}
```

#### Config

The Options object provides config metadata about how the property behaves. A big idea is about `reflection` which is whether or not a change in the value in JS should trigger will correspond to Lit updating the corresponding HTML attribute on the DOM element.

```typescript
@property({ type: String, reflect: true })
title = 'Hello';

// then later
myEl.title = 'World';
// this would not update the HTML element but it needs reflect: true.
```

Initially, the HTML element does not have the property as an attribute, but with reflect: true, Lit will set the attribute onto the HTML element.

```typescript
@property({ type: String }) // no reflect
status = 'active';

// later
myEl.status = 'offline';
// The <my-element> tag will NOT update the `status` attribute
```

There are other options within the options object such as `attribute` where the property does not reflect to an attribute if it is false, this overrides the reflect field. You can also set the attribute to a string with reflect: true.

```typescript
@property({ attribute: 'my-attr', reflect: true })
myProp = 'value';
```

This will tell myProp to bind to `my-attr`.



This is because Lit sets up the reactive properties with getters and setters in the element prototype. While using a class field (`food = "Default"`) is defining on the instance. hiding the getter/setter which is equivalent to:

```typescript
class MyClass {
  get foo() {
    console.log("Getter called!");
    return "getter value";
  }

  set foo(value) {
    console.log("Setter called with:", value);
  }

  foo = "class field value"; // This creates an *own property* on the instance
}
```

Instead you want to wait until the constructor which doesn't override the getter/setter has been set up. This means it now goes through Lit's setter and triggers rerendering, and doesn't override the getter/setter.

```typescript
class MyElement extends LitElement {
  static properties = {
    foo: { type: String },
  };
  constructor() {
    super();
    this.foo = "Default";
  }
}
```

### State 
Lit calls these `Internal Reactive State`
```typescript
state()
  _showModal = false;
```


### Lifecycle


This is what the lifecycle callbacks look like:

```js
class MyCustomElement extends HTMLElement {
  static observedAttributes = ["color", "size"];

  constructor() {
    // Always call super first in constructor
    super();
  }

  connectedCallback() {
    console.log("Custom element added to page.");
  }

  disconnectedCallback() {
    console.log("Custom element removed from page.");
  }

  connectedMoveCallback() {
    console.log("Custom element moved with moveBefore()");
  }

  adoptedCallback() {
    console.log("Custom element moved to new page.");
  }

  attributeChangedCallback(name, oldValue, newValue) {
    console.log(`Attribute ${name} has changed.`);
  }
}

customElements.define("my-custom-element", MyCustomElement);
```

### Event Handling

### Automated Testing
The docs suggest using the `Web Test Runner` for unit tests which can test things inside the Shadow DOM of a Lit Component.

[Lit Test Example](https://github.com/modernweb-dev/example-projects/blob/master/lit-element/test/my-element.test.js)

```javascript
import { html, fixture, expect } from '@open-wc/testing';

import { MyElement } from '../src/MyElement';
import '../my-element';

describe('MyElement', () => {
  it('has a default title "Hey there" and counter 5', async () => {
    const el: MyElement = await fixture(html` <my-element></my-element> `);

    expect(el.title).to.equal('Hey there');
    expect(el.counter).to.equal(5);
  });
})
```