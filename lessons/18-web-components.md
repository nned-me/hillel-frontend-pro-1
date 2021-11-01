# Web components

- components based design paradigm
- web components basics
- custom elements
- shadow DOM
- HTML templates


## Components based design paradigm


## Web components basics
We can create custom elements that incorporate in itself
- HTML
- CSS
- JS


## Custom elements

**There are 2 types of custom elements:**
- **Autonomous custom elements**  — standalone they don't inherit from standard HTML elements. They look liks this in code: `<user-name-link username="Alex" url="/user/123"/ img="alex.png">`

- **Customized built-in elements** inherit from basic HTML elements. They need to use `is` attribute: `<href is="user-name-link" url="/user/123"/ img="alex.png">`, or `document.createElement("href", { is: "user-name-link" })`.

NOTE: At the moment Safari can not use custoimzed built in elements. You can check it here: https://caniuse.com/mdn-api_customelementregistry_builtin

![image](https://user-images.githubusercontent.com/22635061/139707703-6dfe9cfd-def9-4fa6-b210-33315cdfb6b3.png)


## Let's try it!
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

## What about styling?

1. Just add `style=""` to the elements inside the component.
2. Use **Shadow Dom** and add internal `<style>` element to it.
3. Use **Shadow Dom** and add external style via `<href type="style">` element to it.


## Shadow DOM
What is Shadow DOM? 
- Basicallly it is an indendent DOM tree that can be attached to any element. 
- It is not acccessible and visible via default methods
- It can have own css rules


**Why use it?**
- **Isolated DOM**: A component's DOM is self-contained (e.g. document.querySelector() won't return nodes in the component's shadow DOM).
- **Scoped CSS**: CSS defined inside shadow DOM is scoped to it. Style rules don't leak out and page styles don't bleed in.
- **Composition**: Design a declarative, markup-based API for your component.
- **Simplifies CSS** - Scoped DOM means you can use simple CSS selectors, more generic id/class names, and not worry about naming conflicts.
- **Productivity** - Think of apps in chunks of DOM rather than one large (global) page.

**NOTE:**  
Shadow DOM is supported by default in Firefox (63 and onwards), Chrome, Opera, and Safari. The new Chromium-based Edge (79 and onwards) supports it too; the old Edge didn't.

**NOTE**
Shadow DOM can be attached only to custom elements or one of this element tyeps: 
`"article", "aside", "blockquote", "body", "div", "footer", "h1", "h2", "h3", "h4", "h5", "h6", "header", "main", "nav", "p", "section", "span"`

![image](https://developer.mozilla.org/en-US/docs/Web/Web_Components/Using_shadow_DOM/shadowdom.svg)

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





## HTML templates






