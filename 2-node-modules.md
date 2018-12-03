# Node.js Core API Basics

## Modules
* in JS, global namespace is a common addressing space for all loaded scripts. Once any JS code is loaded into a web page, it is injected into the global namespace.
* This can lead to bugs, conflicts and security issues.
* to solve this, Node.js uses the CommonJS module standard.
* this allows every module to have its own context and does not pollute the global scope.
* How Node loads modules:
  * referenced by path or name. Name resolves to path unless it is a core module.
  * Core modules are preloaded when a Node process starts.
  * Each module exposes public API. `var module = require('module_name');`
  * require returns oject representing the API exposed. Can be anything: function, object with properties etc.
* Exporting a module:
  * In Node - files and modules are one-to-one.
  ```javascript
  function Circle(x,y,r) {
    function r_squared() {
      return Math.pow(r,2);
    }
    function area() {
      return Math.PI * r_squared();
    }
    return {
      area: area
    }
  }
  module.exports = Circle;
  ```
  * `module.exports` defines what is to be exported. `module` - object that represents current module. `exports` - object the module will expose to spripts that require this module.
  * can be used like:
  ```javascript
  var circle = require('path to module');
  var small = new circle(5,6,7);
  console.log(small.area());
  ```
  * `module.exports` is initialized with empty object. Any number of properties can be added.
  ```javascript
  function printA(){
    console.log('A');
  }
  function printB() {
    console.log('B');
  }
  module.exports.printA = printA;
  module.exports.pi = Math.PI;
  ```
  * to use:
  ```javascript
  var myModule = require('./myModule');
  myModule.printA(); // -> A
  console.log(myModule.pi); // -> 3.14....
  ```
* Loading Modules:
  * `require` does not change global namespace as Node has no global namespace. It returns a module object that can be assigned to local variable.
  * Core Module:
    * Compiled into Node binary distro.
    * can be referred by name. Preferentially loaded if there's 3rd party module with same name.
    * `var http = require('http');`
    * Custom modules can be loaded with absolute or relative path.
    * Can omit `.js` extesion.
    * for folder module, provide path to folder. Node will look for package.json and start executing at main attribute. If no package.json, it will start at `index.js`.
    * If not relative and not core, Node will look for module in node_modules folder in current directory.
    * Example: for `var module1 = require('module1.js')` Node will look for `./node_modules/module1.js`, if not found, it will go into the parent folder `../node_modules/module1.js`.
    * Node will keep going into parent folders until it is found or it reaches root.
    
* Caching Modules: 
  * Modules are cached after the first time they are loaded. As long as a require statement resolves to the same file, it will not be loaded again, even if it is required multiple times.
