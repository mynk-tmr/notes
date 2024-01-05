#### JavaScript
- Features
	- object-oriented scripting language
	- runtime envr provides I/O ability to JS through objects
	- ECMAScript : Standardized JS
	- interpreted and JIT compiled
	- single-threaded : Core JS has 1 call stack
	- *dynamic* : runtime type binding ; unlike compile time viz. fixed
- Limits
	- type checks, security, critical performance

##### V8's JIT compiler

At first execution, interpreter is used. On further executions, V8 compiles & optimise *frequently executed code* (Hot code) into **machine code** to improve performance. The compiled code is *re-optimized dynamically* at runtime, based on code’s execution profile.


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

---
### Scope

`var` : no Block scope, no TDZ, redeclarable, creates a property on `window`
##### Scope Chain
- Each scope can access values in all its enclosing scopes but not vice-versa. This approach is called **lexical scoping** 
- names in inner scope **overshadow** outer ones. Using var to overshadow let/const is `illegal`
- inner function can be `only` called from statements in parent function.
##### Hoisting
- process where, before code execution, interpreter relocates _func/class/variable/import_ declarations to `top` of their scope. 
- Only functions are hoisted with their values. 
- `let/const/class` remain in TDZ until their declarations are reached during program flow. Any attempt to access during TDZ throws error.
##### Lexical Environment
- A specification object for a *executing* function/code block/script. It has 2 keys
	1. **Environment Record** : local variables are its properties, also store `this : value`
	2. A reference to **outer LEX** object (or null)
- ERec is populated with variables during allocation phase, but are uninitialised until declared. (why TDZ exist)
- JS engine traverses LEX chain to find variables 

----
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
export let one=1, two=2  // declarations
export {f, g}  // list 

//default exports --> only 1 allowed
export default myfunc; 
export {myfunc as default} 
export default func(){..} 

// re-export/aggregation combine import + export ; syntax like import
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

---
Dynamic imports
Export and import statements that we covered in previous chapters are called “static”. The syntax is very simple and strict.

First, we can’t dynamically generate any parameters of import.

The module path must be a primitive string, can’t be a function call. This won’t work:

import ... from getModuleName(); // Error, only from "string" is allowed
Second, we can’t import conditionally or at run-time:

if(...) {
  import ...; // Error, not allowed!
}

{
  import ...; // Error, we can't put import in any block
}
That’s because import/export aim to provide a backbone for the code structure. That’s a good thing, as code structure can be analyzed, modules can be gathered and bundled into one file by special tools, unused exports can be removed (“tree-shaken”). That’s possible only because the structure of imports/exports is simple and fixed.

But how can we import a module dynamically, on-demand?

The import() expression
The import(module) expression loads the module and returns a promise that resolves into a module object that contains all its exports. It can be called from any place in the code.

We can use it dynamically in any place of the code, for instance:

let modulePath = prompt("Which module to load?");

import(modulePath)
  .then(obj => <module object>)
  .catch(err => <loading error, e.g. if no such module>)
Or, we could use let module = await import(modulePath) if inside an async function.

For instance, if we have the following module say.js:

// 📁 say.js
export function hi() {
  alert(`Hello`);
}

export function bye() {
  alert(`Bye`);
}
…Then dynamic import can be like this:

let {hi, bye} = await import('./say.js');

hi();
bye();
Or, if say.js has the default export:

// 📁 say.js
export default function() {
  alert("Module loaded (export default)!");
}
…Then, in order to access it, we can use default property of the module object:

let obj = await import('./say.js');
let say = obj.default;
// or, in one line: let {default: say} = await import('./say.js');

say();
Here’s the full example:

Resultsay.jsindex.html
<!doctype html>
<script>
  async function load() {
    let say = await import('./say.js');
    say.hi(); // Hello!
    say.bye(); // Bye!
    say.default(); // Module loaded (export default)!
  }
</script>
<button onclick="load()">Click me</button>
Please note:
Dynamic imports work in regular scripts, they don’t require script type="module".

Please note:
Although import() looks like a function call, it’s a special syntax that just happens to use parentheses (similar to super()).

So we can’t copy import to a variable or use call/apply with it. It’s not a function.