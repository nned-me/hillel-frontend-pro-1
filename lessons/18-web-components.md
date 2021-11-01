# Web components

- components based design paradigm
- web components basics
- custom elements
- shadow DOM


## Components based design paradigm


### Web components basics
We can create custom elements that incorporate in itself
- HTML
- CSS
- JS


### Custom elements

**There are 2 types of custom elements:**
- **Autonomous custom elements**  — standalone they don't inherit from standard HTML elements. They look liks this in code: `<user-name-link username="Alex" url="/user/123"/ img="alex.png">`

- **Customized built-in elements** inherit from basic HTML elements. They need to use `is` attribute: `<href is="user-name-link" url="/user/123"/ img="alex.png">`, or `document.createElement("href", { is: "user-name-link" })`.

NOTE: At the moment Safari can not use custoimzed built in elements. You can check it here: https://caniuse.com/mdn-api_customelementregistry_builtin

![image](https://user-images.githubusercontent.com/22635061/139707703-6dfe9cfd-def9-4fa6-b210-33315cdfb6b3.png)


### Let's try it!
Let's create rather dumb component `<yolo-btn name="Alex">` which renders button and shows alert YOLO on click .

First, we need to declare ES6 class for this component:
```
class YoloButttonComponent extends HTMLElement {
  constructor() {
    super();
  }
  
  onClick() {
  }
}
```

Then we use `CustomElementRegistry` like this:
```
window.customElements.define('yolo-btn', YoloButttonComponent);
```

Volia! We have our first web component up and running!

### Lifecycle hooks

- **constructor**:	An instance of the element is created or upgraded. Useful for initializing state, setting up event listeners, or creating a shadow dom. 
- **connectedCallback**:	Called every time the element is inserted into the DOM. Useful for running setup code, such as fetching resources or rendering. Generally, you should try to delay work until this time.
- **disconnectedCallback**:	Called every time the element is removed from the DOM. Useful for running clean up code.
- **attributeChangedCallback(attrName, oldVal, newVal)**:	Called when an observed attribute has been added, removed, updated, or replaced. Also called for initial values when an element is created by the parser, or upgraded. **Note**: only attributes listed in the **observedAttributes** property will receive this callback.
- **adoptedCallback**:	The custom element has been moved into a new document (e.g. someone called document.adoptNode(el)).



```
class YoloWeather extends HTMLElement {
  constructor() {
    super();
    
    this.cityName = this.getAttibute('city');
  }
  
  static get observedAttributes() {
    return ['disabled', 'city'];
  }
  
  connectedCallback() {
    // fetch weather by cityName
  }
  
  disconnectedCallback() {
    // remove some stuff
  }
  
  attributeChangedCallback(attrName, oldVal, newVal) {
    // check if city was changed
  }
  
  adoptedCallback() {
    // some iframe stuff?..
  }
}
```



### What about styling?

1. Just add `style=""` to the elements inside the component.
2. Use **Shadow Dom** and add internal `<style>` element to it.
3. Use **Shadow Dom** and add external style via `<href type="style">` element to it.

---
## Shadow DOM
What is Shadow DOM? 
- Basicallly it is an indendent DOM tree that can be attached to any element. 
- It is not acccessible and visible via default methods,  for example with `querySelector`
- It can have own css rules and page css rules do not affect Shadow DOM

![image](https://developer.mozilla.org/en-US/docs/Web/Web_Components/Using_shadow_DOM/shadowdom.svg)

In browser it looks like:
```
#shadow-root
  <style>...</style>
  <slot name="icon"></slot>
  <span id="wrapper">
    <slot>Button</slot>
  </span>
```

**Why use it?**
- **Isolated DOM**: A component's DOM is self-contained (e.g. document.querySelector() won't return nodes in the component's shadow DOM).
- **Scoped CSS**: CSS defined inside shadow DOM is scoped to it. Style rules don't leak out and page styles don't bleed in.
- **Composition**: Design a declarative, markup-based API for your component.
- **Simplifies CSS** - Scoped DOM means you can use simple CSS selectors, more generic id/class names, and not worry about naming conflicts.
- **Productivity** - Think of apps in chunks of DOM rather than one large (global) page.

**NOTE:**  
Shadow DOM is supported by default in Firefox (63 and onwards), Chrome, Opera, and Safari. The new Chromium-based Edge (79 and onwards) supports it too; the old Edge didn't.

**NOTE**
Shadow DOM can be attached not to all elements. 
- The browser already hosts its own internal shadow DOM for the element (<textarea>, <input>).
- It doesn't make sense for the element to host a shadow DOM (<img>).
So host can be **custom element** or one of this element types: 
`"article", "aside", "blockquote", "body", "div", "footer", "h1", "h2", "h3", "h4", "h5", "h6", "header", "main", "nav", "p", "section", "span"`

Every element can hav 2 DOM subtrees:
- Light DOM Tree
- Shadow DOM Tree

**If shadow dom is present, only it is rendering**


There are some bits of shadow DOM terminology to be aware of:

- **Shadow host:** The regular DOM node that the shadow DOM is attached to.
- **Shadow tree**: The DOM tree inside the shadow DOM.
- **Shadow boundary**: the place where the shadow DOM ends, and the regular DOM begins.
- **Shadow root**: The root node of the shadow tree.


**USAGE**
```
const shadow = elementRef.attachShadow({mode: 'open'});
// or
const shadow = elementRef.attachShadow({mode: 'closed'});
```
In open mode shadow of element is accesible by `element.shadowRoot` property. In closed it will be `null`

```
const elemShadow = myCustomElem.shadowRoot;
```

**Usage with Custom Elemens**
```
  class YoloBox extends HTMLElement {
  constructor() {
    super(); // always call super() first in the constructor.

    // Attach a shadow root to 
    const shadowRoot = this.attachShadow({mode: 'open'});
    shadowRoot.innerHTML = `
      <style>.yolo-box { color: red }</style> <!-- styles are scoped! -->
      <div id="yolo-box">...</div>
    `;
  }

  customElements.define('my-element', YoloBox);
```


### Shadow DOM Slots 
Light DOM elements can appear in Shadow DOM via 'portals' called slots.

**Named slots**

```
<yolo-card>
  <img slot="avatar" src="/my-ava.png" />
  <p slot="desc">
    <b>Yolo</b>
    My name is ...
    My page is 
  </p>
</yolo-card>
```
  
```
  const shadowRoot = elem.attachShadow({mode: 'open'});
  shadowRoot.innerHTML = `
    <div id="yolo-box">
        <div class="avatar">
           <slot name="avatar"></slot>
        </div>
        <!-- some more markup--->
        <div class="desc">
           <slot name="desc"></slot>
        </div>
    </div>
  `;
```

**Unnamed slots**
In unnamed slot goes all light DOM in element that is not

```
<yolo-card>
  <img slot="avatar" src="/my-ava.png" />
  <p slot="desc">
    <b>Yolo</b>
    My name is ...
    My page is 
  </p>
  <p>
    some other info
  </p>
  <p>
    some other info 2
  </p>
</yolo-card>
```

```
  const shadowRoot = elem.attachShadow({mode: 'open'});
  shadowRoot.innerHTML = `
    <div id="yolo-box">
        <div class="avatar">
           <slot name="avatar"></slot>
        </div>
        <!-- some more markup--->
        <div class="desc">
            <h3>Desc</h3>
           <slot name="desc"></slot>
        </div>
        <slot>
          <div class="other-info">
            <h3>Other info</h3>
          </div>
        </slot>
    </div>
  `;
```







