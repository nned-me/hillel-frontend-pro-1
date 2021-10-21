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


**Let's try it!**
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
2. Use Shadow Dom and add internal `<style>` element to it.
3. Use Shadow Dom and add external style via `<href type="style">` element to it.


## Shadow DOM
What is Shadow DOM? Basicallly it is an indendent DOM tree that can be attached to any element



## HTML templates






