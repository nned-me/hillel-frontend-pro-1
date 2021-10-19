**Lesson Themes:**
- `async` / `await`
- modules
- imports

## Async / await


## Modules in JS

**Why use modules?**
- code structure
- code isolation
- flexibility


**Module types:**
- IIFE - Immediately invoked function expression
- CommonJS Modules -- used by **Node.js**
- ES6 Modules -- native in ES6 supported in modern browsers
- AMD -- Asynchronous Module Definition. Used by **RequireJS**, slowly becoming obsolette
- UMD -- Universal Module Definition, to make your code work in differnet environments
- SystemJS Module -- used by SystemJS to transpile modules for old browsers
- Webpack module -- Webpack works with all modules, packing them in a super-structure
- Babel Module -- Babel also transpiles ES6 modules for old browsers 
- Typescript Module -- Typecript uses and transpiles ES6 modules 



## CommonJS Modules

```
// add.js
function add (a, b) {
  return a + b
}

module.exports = add
```

```
// index.js
const add = require('./add')

console.log(add(4, 5))
```

Internaly this look like:
```
(function (exports, require, module, __filename, __dirname) {
  function add (a, b) {
    return a + b
  }

  module.exports = add
})
```



## AMD Modules
```
  define('amdCounterModule', ['dependencyModule1', 'dependencyModule2'], (dependencyModule1, dependencyModule2) => {
      let count = 0
      const increase = () => ++count
      const reset = () => {
          count = 0
          console.log('Счетчик сброшен.')
      }

      return {
          increase,
          reset
      }
  })
```

```
```



## ES6 Modules
```
let count = 0;
// Named export:
export const increase = () => ++count;
export const reset = () => {
    count = 0;
    console.log("Count is reset.");
};
// Or default export:
export default {
    increase,
    reset
};
```

```
// Different ways of importing modules
import defaultExport from "module-name";
import * as name from "module-name";
import { export1 } from "module-name";
import { export1 as alias1 } from "module-name";
import { export1 , export2 } from "module-name";
import { export1 , export2 as alias2 , [...] } from "module-name";
import defaultExport, { export1 [ , [...] ] } from "module-name";
import defaultExport, * as name from "module-name";
import "module-name";
const promise = import("module-name");
```

