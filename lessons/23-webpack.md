- module bundlers



## Webpack

![image](https://user-images.githubusercontent.com/22635061/142248899-0892712d-9e84-4cf7-a1f0-25be038436f4.png)


**Webpack** is a static module bundler. In a particular project, webpack treats all files and assets as modules. 
Under the hood, it relies on a dependency graph. 
A dependency graph describes how modules relate to each other using the references (require and import statements) between files. 

Basic terms:
- **Entry**: module that webpack uses to start building its internal dependency graph. From there, it determines which other modules and libraries that entry point depends on (directly and indirectly) and includes them in the graph until no dependency is left. By default, the entry property is set to ./src/index.js, but we can specify a different module (or even multiple modules) in the webpack configuration file.
- **Output**: where to emit the bundle(s) and what name to use for the file(s). The default value for this property is ./dist/main.js for the main bundle and ./dist for other generated files — such as images, for example. Of course, we can specify different values in the configuration depending on our needs.
- **Loaders**: by default, webpack only understands JavaScript and JSON files. To process other types of files and convert them into valid modules, webpack uses loaders. Loaders transform the source code of non-JavaScript modules, allowing us to preprocess those files before they’re added to the dependency graph. For example, a loader can transform files from a CoffeeScript language to JavaScript or inline images to data URLs. With loaders we can even import CSS files directly from our JavaScript modules.
- **Plugins**: plugins are used for any other task that loaders can’t do. They provide us with a wide range of solutions about asset management, bundle minimization and optimization, and so on.
- **Mode**: typically, when we develop our application we work with two types of source code — one for the development build and one for the production build. Webpack allows us to set which one we want to be produced by changing the mode parameter to development, production or none. This allows webpack to use built-in optimizations corresponding to each environment. The default value is production. The none mode means that no default optimization options will be used. To learn more about the options webpack uses in development and production mode, visit the mode configuration page.


## Basic Setup
```
```

## Webpack config
Add `webpack.config` in the project root

```

``
