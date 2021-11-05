Lesson points
- html templates
- mvc
- flow



- **Its content is effectively inert until activated**. Essentially, your markup is hidden DOM and does not render.

- **Any content within a template won't have side effects**. Script doesn't run, images don't load, audio doesn't play,...until the template is used.

- **Content is considered not to be in the document.** Using document.getElementById() or querySelector() in the main page won't return child nodes of a template.

- **Templates can be placed anywhere inside of <head>, <body>, or <frameset>** and can contain any type of content which is allowed in those elements. Note that "anywhere" means that <template> can safely be used in places that the HTML parser disallows...all but content model children. It can also be placed as a child of <table> or <select>:

<table>


```
<template id="mytemplate">
  <img src="" alt="great image">
  <div class="comment"></div>
</template>
```

```
  elem.append(tmpl.content.cloneNode(true));
```
