JavaScript is an object-oriented scripting language used to make webpages interactive. 

Inside a host environment, JavaScript can be connected to host’s objects to acquire control over host. Core JS has no input or output ; host provides that mechanism.

_ECMAScript_ : Standardized version of JavaScript 

Features of JS
 - lightweight & has low CPU usage
 - interpreted and JIT compiled
 - single-threaded : Core JS has 1 call stack
 - multi-paradigm : OOP, functional, declarative
 - dynamic/untyped

Limits of core JS
 - Weak error handling and type checking
 - Security risks: cross-site script attacks
 - Performance: comparatively slow

hashbang comment : `#!/usr/bin/env node` ; specifies path to engine that will execute script

Variable and expression type is determined during runtime, unlike in static languages where it is determined at compile time and can’t be changed


__V8's JIT compiler__

It compiles ECMAScript directly to native machine code.
At first execution, interpreter is used. On further executions, V8 finds *patterns* such as frequently used functions, variables, etc. and *compiles* them to improve performance. The compiled code is *re-optimized dynamically* at runtime, based on code’s execution profile.


**Engine Memory** 
- *Call stack* : a LIFO data structure that stores execution contexts, primitives and references to objects
- *Memory Heap* : stores actual objects


__Stages of code execution__

Takes place for each execution context,

 1. **Memory creation phase** allocate memory for variables and functions (No initialisation)
 2. **Code execution phase** values are computed and assigned to variables (wherever possible). 

_Execution context_ is an internal data structure that contains details about a function’s execution like value of `this`, local & closure variable, control flow's location, etc.

_Call Stack_ : It is initialised with global context. When a function is called, it’s execution context is created and pushed to stack, and when it completes, it is popped

_Parsing_ : Script is scanned left to right, line by line and converted into stream of tokens, control characters, line terminators, comments, or whitespace. 


### Garbage Collection

Notes
 - performed automatically. We cannot force or prevent it.
 - garbage collector — a background process that monitors all objects and removes those that are _unreachable_ via chain of references from root
 - a root is a data which is never garbage collected — global bindings and local bindings of current chain of nested function calls.

To delete an object, we must delete all _incoming_ references to it. 

_Unreachable island_: when a pack of interlinked objects is cutoff from root references.

**“mark-and-sweep”** : basic garbage collection algorithm

```go
1- “mark” (remember) roots
2- mark all references from them
3- visit marked objects and mark their references. 
4- repeat 3rd step until no objects can be marked.
5- remove all 'unmarked' objects
```

JS engine apply many optimizations to speed it up

 - **Generational collection** – objects are split into two sets: “new ones” and “old ones”. Objects which remain in memory for long are considered old and examined less often.
 - **Incremental collection** – engine splits the whole set of existing objects into multiple parts. And then clear these parts one after another.
 - **Idle-time collection** – the garbage collector tries to run only while the CPU is idle


### Scope and Closures

_Scope_ : accessibility or visibility of variables / functions ⇒ global < module < function < block.

_Overshadowing_ : when more specific variable/func is used in a scope, given name-conflicts. Using var to overshadow let/const is `illegal` , usually result of `vars` in block

_Global scope_ : declared outside any code block or with a global object. When declared with `var`, they become properties of `globalThis` ; future proof => `window.x`


`var` vs. `let/const` : no Block scope, supports hoisting, redeclarations

_Scope Chain_ 
  - Each scope can access data/functions in all enclosing scopes but not vice-versa. This approach is called *lexical scoping*. 
  - JS engine searches a variable’s value firstly in current scope and then outer scope(s) one by one until global scope is reached.
  - inner function can be `only` called from statements in parent function.

_Hoisting_ : process where, before code execution, interpreter relocates _func/class/variable/import_ declarations to `top` of their scope. Only functions are hoisted with their values. 

`let/const/class` remain in TDZ until their declarations are reached during program flow. `temporal` means zone depends on execution order of statements.


### Using JS 

3 ways to include JS 
  - `inline` (event attributes) 
  - `internal` (within script element)
  - `external` (link file)


__CommonJS modules__
`modules.export= getHello` => export
`const myvar = require('url')` => import

require() function can be called from anywhere within the program, whereas import() cannot be called conditionally. require() needs `.js` files, import() needs `.mjs`


### ES6 modules

A self-contained piece of code that encapsulates related functionality together. Written in separate files and imported & exported to other modules. 

Benfits -
  - organize code into logical units, easier to maintain and reuse.
  - expose and use only relevant data/functionality
  - doesn't pollute global scope  ; no `globalThis`
  - use strict mode, auto-defered 

Tell runtime it's a module
  - `type=module` in html
  - `'type': 'module'` in package.json


Imports are _live_ bindings and importing module _cannot change it's value_


```js
//named exports
export let one, two  // declarations
export {f, g}  // list 

//default exports --> only 1 allowed
export default myfunc; 
export {myfunc as default} 
export default func(){..} 

// re-export/aggregating combine import + export ; syntax like import
export {default as f, g} from './no.js' 
export * from './nat.js'  // only re-export NAMED

//name-conflicts => neither value is exported e.g. if 'nat.js' has g


// 4 types of import
import {f, g as google} from './dist.js' //named 
import mydef from './hi.js' //default  => give name
import * as tree from './test.js' // namespace => test.func() ; 'as' is mandatory
import "./test.js";  // side-effect => only runs code in file

//test is namespace object ; a sealed object with null prototype. 
```

More info : https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules#top_level_await
https://devdocs.io/javascript/operators/import

### Event Loop, Macrotask, Microtasks

JavaScript execution flow is based on an *event loop*. General algorithm of event loop :- 
1. If call stack is empty and **task queue** has a *macrotask*, deque and execute it
2. execute all *microtasks* placed in **microtask queue** by macrotask in similar fashion (FIFO)
3. paint DOM, do network requests etc
4. Go to step 1

Microtasks :- `promise` handling, `await` calls, `queueMicrotask(func)` 
Macrotasks :- `script` execution, `event` handlers, callbacks in  `setTimeout ,setInterval`

```js
setTimeout(() => alert("timeout")); 
Promise.resolve().then(() => alert("promise"));
alert("code");

// "code promise timeout" because macro -> micro -> macro
```


**Web Workers**

Run code in different parallel thread. Can exchange messages with the main process, but they have their own variables, and their own event loop.  JS never shares data across threads

Web Workers do not have access to DOM, so they are useful, mainly, for calculations, to use multiple CPU cores simultaneously.