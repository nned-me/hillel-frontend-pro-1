**Lesson points**
- html templates
- SVG
- Canvas basics


## Templates


- **Its content is effectively inert until activated**. Essentially, your markup is hidden DOM and does not render.

- **Any content within a template won't have side effects**. Script doesn't run, images don't load, audio doesn't play,...until the template is used.

- **Content is considered not to be in the document.** Using document.getElementById() or querySelector() in the main page won't return child nodes of a template.

- **Templates can be placed anywhere inside of <head>, <body>, or <frameset>** and can contain any type of content which is allowed in those elements. Note that "anywhere" means that <template> can safely be used in places that the HTML parser disallows...all but content model children. It can also be placed as a child of <table> or <select>:

  - It can be accessed as a **DocumentFragment** instance via the special content property of the <template> element.


```
<template id="mytemplate">
  <img src="" alt="great image">
  <div class="comment"></div>
</template>
```

```
  elem.append(tmpl.content.cloneNode(true));
  // or
  var clone = document.importNode(t.content, true);
  document.body.appendChild(clone);
```


## SVG

- Small file sizes that compress well
- Scales to any size without losing clarity (except very tiny)
- Looks great on all displays
- Granular Design control like coloring, interactivity and filters
  

 
  
```
<svg xmlns="http://www.w3.org/2000/svg">
  <circle r="50" cx="50" cy="50" fill="red"/>
  <rect x="120" y="5" width="90" height="90"
        stroke="blue" fill="none"/>
</svg>
```
  
```
  let circle = document.querySelector("circle");
  circle.setAttribute("fill", "cyan");
```

  
```
<svg width="100" height="100">
  <circle cx="50" cy="50" r="40" stroke="green" stroke-width="4" fill="yellow" />
</svg>

 <svg width="400" height="110">
  <rect width="300" height="100" style="fill:rgb(0,0,255);stroke-width:3;stroke:rgb(0,0,0)" />
</svg>
  
<svg height="140" width="500">
  <ellipse cx="200" cy="80" rx="100" ry="50"
  style="fill:yellow;stroke:purple;stroke-width:2" />
</svg>

<svg height="210" width="500">
  <line x1="0" y1="0" x2="200" y2="200" style="stroke:rgb(255,0,0);stroke-width:2" />
</svg>
  
<svg height="210" width="500">
  <polygon points="200,10 250,190 160,210" style="fill:lime;stroke:purple;stroke-width:1" />
</svg>
  
 <svg height="200" width="500">
  <polyline points="20,20 40,25 60,40 80,120 120,140 200,180"
  style="fill:none;stroke:black;stroke-width:3" />
</svg>
  
<svg height="210" width="400">
  <path d="M150 0 L75 200 L225 200 Z" />
</svg>

/**
  M = moveto
  L = lineto
  H = horizontal lineto
  V = vertical lineto
  C = curveto
  S = smooth curveto
  Q = quadratic Bézier curve
  T = smooth quadratic Bézier curveto
  A = elliptical Arc
  Z = closepath  
**/
  
 
<svg height="60" width="200">
  <text x="0" y="15" fill="red" transform="rotate(30 20,40)">I love SVG</text>
</svg>  
```

### SVG Viewport & Viewbox
 - **viewPort** - The viewport is the visible area of the SVG image. An SVG image can logically be as wide and high as you want, but only a certain part of the image can be visible at a time. The area that is visible is called the viewport.
 - **viewBox** - which part of SVG we display in the viewPort

param: `min-x, min-y, width, height` 

  
```
<svg width="400" height="200" viewBox="0 0 40 20" >
  ...
</svg>  
```
  viewBox="0 0 50 20" 
  
  
 **Online editors: **
 - Boxy SVG https://boxy-svg.com/
 - Vector Paint https://vectorpaint.yaks.co.nz/ 
 - Method https://editor.method.ac/
  
 ### Resources
- **CSS Tricks: Everything You Need To Know About SVG** https://css-tricks.com/lodge/svg/
- **SVC on the Web**: https://svgontheweb.com/
- **Web Designer Depo: THE ULTIMATE GUIDE TO SVG**  https://www.webdesignerdepot.com/2015/01/the-ultimate-guide-to-svg/
  
  
## Canvas
  Works with 2D and 3D
 
  ```
    <canvas id="drawer" width="150" height="150">
    </canvas>
  ```

  ```
  var canvas = document.getElementById('tutorial');
  var ctx = canvas.getContext('2d');
  ```

  ```
      function draw() {
      var canvas = document.getElementById('canvas');
      if (canvas.getContext) {
        var ctx = canvas.getContext('2d');

        ctx.fillStyle = 'rgb(200, 0, 0)';
        ctx.fillRect(10, 10, 50, 50);

        ctx.fillStyle = 'rgba(0, 0, 200, 0.5)';
        ctx.fillRect(30, 30, 50, 50);
      }
    }
```
  
```
  function draw() {
    var canvas = document.getElementById('canvas');
    if (canvas.getContext) {
      var ctx = canvas.getContext('2d');

      ctx.beginPath();
      ctx.moveTo(75, 50);
      ctx.lineTo(100, 75);
      ctx.lineTo(100, 25);
      ctx.fill();
    }
  }  
```
  
```
  function draw() {
  var canvas = document.getElementById('canvas');
  if (canvas.getContext) {
     var ctx = canvas.getContext('2d');

    ctx.beginPath();
    ctx.arc(75, 75, 50, 0, Math.PI * 2, true); // Outer circle
    ctx.moveTo(110, 75);
    ctx.arc(75, 75, 35, 0, Math.PI, false);  // Mouth (clockwise)
    ctx.moveTo(65, 65);
    ctx.arc(60, 65, 5, 0, Math.PI * 2, true);  // Left eye
    ctx.moveTo(95, 65);
    ctx.arc(90, 65, 5, 0, Math.PI * 2, true);  // Right eye
    ctx.stroke();
  
    // Stroked triangle
    ctx.beginPath();
    ctx.moveTo(125, 125);
    ctx.lineTo(125, 45);
    ctx.lineTo(45, 125);
    ctx.closePath();
    ctx.stroke();
  }
}
```

```
  arc(x, y, radius, startAngle, endAngle, counterclockwise)
// Draws an arc which is centered at (x, y) position with radius r starting at startAngle and ending at 
// endAngle going in the given direction indicated by counterclockwise (defaulting to clockwise).

  
arcTo(x1, y1, x2, y2, radius)
// Draws an arc with the given control points and radius, connected to the previous point by a straight line.
  
```
**Note:** Angles in the arc function are measured in radians, not degrees. To convert degrees to radians you can use the following JavaScript expression: radians = (Math.PI/180)*degrees.  
  
  
```
  ctx.fillStyle = 'orange';
  ctx.fillStyle = '#FFA500';
  ctx.fillStyle = 'rgb(255, 165, 0)';
  ctx.fillStyle = 'rgba(255, 165, 0, 1)';

  ctx.globalAlpha = transparencyValue;
  // Applies the specified transparency value to all future shapes drawn on the canvas. 
  // The value must be between 0.0 (fully transparent) to 1.0 (fully opaque). This value is  1.0 (fully opaque) by default.
```

```
```

### Transformations
  
State:
  
- The transformations that have been applied (i.e. translate, rotate and scale – see below).
- The current values of the following attributes: `strokeStyle, fillStyle, globalAlpha, 
  lineWidth, lineCap, lineJoin, miterLimit, lineDashOffset, 
  shadowOffsetX, shadowOffsetY, shadowBlur, shadowColor, 
  globalCompositeOperation, font, textAlign, textBaseline, direction, 
  imageSmoothingEnabled`.
- The current clipping path
  
  
- save() Saves the entire state of the canvas.
- restore() Restores the most recently saved canvas state.

  
#### Translate
```
  translate(x, y)
```
![translate](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial/Transformations/canvas_grid_translate.png)

  
#### Rotate
```
rotate(angle)
// Rotates the canvas clockwise around the current origin by the angle number of radians.
```
![rotate](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial/Transformations/canvas_grid_rotate.png)
  
  
#### Scaling
```
  scale(x, y)
```
Scales the canvas units by x horizontally and by y vertically. Both parameters are real numbers. Values that are smaller than 1.0 reduce the unit size and values above 1.0 increase the unit size. Values of 1.0 leave the units the same size.


#### Transforming

```
  transform(a, b, c, d, e, f)
  // Multiplies the current transformation matrix with the matrix described by its arguments. 

  setTransform(a, b, c, d, e, f)
  // Resets the current transform to the identity matrix, and then invokes the transform() method with the same arguments. 
  // This basically undoes the current transformation, then sets the specified transform, all in one step.

  resetTransform()
  // Resets the current transform to the identity matrix. This is the same as calling: ctx.setTransform(1, 0, 0, 1, 0, 0);  
```

The parameters:

- a (m11)
Horizontal scaling.

- b (m12)
Horizontal skewing.

- c (m21)
Vertical skewing.

- d (m22)
Vertical scaling.

- e (dx)
Horizontal moving.

- f (dy)
Vertical moving.


### Images
  
  https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial/Using_images
  
