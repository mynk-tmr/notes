## Prototypal Inheritance

##### What ?
- Object-to-Object inheritance model using prototypes (a special internal property of each object).
- Objects link their `.prototype` to share properties and form a *chain*. Each object inherits all properties present **up** the prototype chain
- At runtime, property lookup starts at object and goes up the chain, until a match is found.
- Object's own props **shadow** properties available in chain. 
- Cons
	- no multiple inhertiance ⇒ allows only 1 parent prototype
	- changing prototype of a constructor breaks chain

```jsx
Object.prototype === null //no prototype ;default of objects
```
##### How ?
- Each non-arrow function has a
	- internal `[[Construct]]` method
	- `.prototype` : an object used by constructor to create other objects.
- when `new func()` evaluates, constructor creates an empty object, set its prototype to `func.prototype` , then function executes with `this` set to object created
- If no return, returns `this` OR our explicit object

review : [https://javascript.info/prototypes](https://javascript.info/prototypes)

Classes are based on prototype-based *constructor functions*, with enhancements
 - `constructor()` is explictly defined & requires **new** to be called
 - class methods are **non-enumerable**
 - have **strict-mode** and **TDZ** like let
 - **super** keyword is used to *access* *public* props in superclass, or invoke its constructor
 - have *private* fields that aren't inherited and only accessible in Class definition
 - **extends** syntactic sugar of `Object.setPrototypeOf`

NOTES
- Bound functions and Proxy can be constructed, but non-inhertiable ( No `prototype` )
- derived constructors have no initial `this` binding ; upon `super()` , this = new Base().
- `super.func()` uses `this` around function call not super-class

##### 3 types of inheritance in Javascript
- Differential Inheritance
    - a **delegate prototype** is used to serve as base for other objects. 
    - Object acquire some or all of its properties from *prototype* rather than from class definitions.
    - Cons : not good for storing state, any mutation to proto’s member by any instance, changes it for all other instances.
- Concatinative Inheritance / Mixins / Object composition
    - **copying properties** from one or more objects to another, without retaining a reference between them. It relies on JavaScript’s runtime object extension feature ie. `Object.assign()`
- Functional Inheritance
    - using factory function, and then adding new properties using concatenative inheritance.
    - functional mixins : functions created for extending existing objects
##### Comparison
| Class inheritance | Object Composition |
| :--- | ---- |
| creates **is-a** relationships viz restrictive | allows **has-a, uses-a, or can-do** relationships. simpler, more expressive, and more flexible |
| used when you decide types based on what they are | … based on what they do |
| animal sees, dog is animal that barks, cat is animal that meow | animal is seer, dog is seer+barker, cat is seer+meower |

## Functional Programming
#### Decorator Functions

take another function and adds “features” to it without affecting function’s body. We can combine multiple decorators

```jsx
function addCache(orgFunc) {
  const cache = new Map();
  return function(key) { 
    if (!cache.has(key)) cache.set(key, orgFunc.call(this,key)); 
    return cache.get(key);
  };
}
```

To create decorators that keep _access to wrapped function properties_, use a **Proxy** object to wrap a function

[Proxy and Reflect](https://javascript.info/proxy#proxy-apply)
#### Function Currying

A technique that transforms a function of multiple arguments into several functions of a single argument in sequence `f(a,b,c) === f(a)(b)(c)`

Currying is possible because of **closures —** a returned function has access to previous argument(s) in chain.

Uses
- generate single purpose functions that are easy to maintain and debug
- flexible invocations ⇒ sum(1,2)(3) or sum(1)(2)(3) etc…
- **function composition :** a technique to use a HOF to chain functions in particular order. It improves readability and reusability of code, since you can chain same set of functions in various ways to create distinct HOFs

---
Lambda calculus
1. This [Medium article](https://medium.com/@axdemelas/lambda-calculus-with-javascript-897f7e81f259)
2. This [Medium article](https://medium.com/functional-javascript/lambda-calculus-in-javascript-part-1-28ff63824d4d) a tutorial on lambda calculus in JavaScript.
3. This [blog post](https://www.willtaylor.blog/an-introduction-to-lambda-calculus-explained-through-javascript/) explains how to apply its concepts to JavaScript.

## Generator Functions

functions that can **pause** and don’t follow run to completion. Only `*f` can pause itself with `yeild` and only its **own iterator object** can resume it with `next()`. It is a control + communication mechanism. `*f` once returns, is marked complete, and willn't run

```jsx
function* f() {
	for(let i=0; i<10; i++)
	let response = yeild i; //send {value:i, done: false} 
	return 3; // send {done: true} , no value
}

const keygen = f(); //create iterator object
keygen.next() //start *f
keygen.next('ok') //response = 'ok'
keygen.return('quit') // {done: true, value: 'quit'}
keygen.throw('error') //throw error in *f

[...keygen] //0,1,2....
```

##### Generator composition
`yeild*` can be used for it. It “embeds” one generator into another by delegating execution to another generator function
```jsx
function* innerGenerator() {
	yield 1; yield 2; yield 3;
}
function* outerGenerator() {
	yield* innerGenerator(); //as if inner's body embeded here
}
```

##### Iterators
- objects that abide by iteration protocol ie., a `next()` that return `{value, done}` schema
- they can iterate over *iterable objects* that implement `[Symbol.iterator]` Such objects support `for of` and `...spread`
- Arraylike have index and length but aren't always iterable

```js
Array.from(obj, ^mapFn, ^thisArg) // convert iterables/arraylikes
```

|                   | Iterators                   | Async iterators        |
| ----------------- | --------------------------- | ---------------------- |
| Object implements | `Symbol.iterator`           | `Symbol.asyncIterator` |
| `next()` returns  | {value, done}                   | `Promise` that resolves to {value, done}              |
| to loop, use      | `for..of` which runs next()<br>until `done:true` | `for await..of`        |
| features          | can be `...spread`          | NO                     |
```jsx
async function* f(url) {
  for (let i = 0; i <= 5; i++) {
    let i = await fetch(url[i]);
    yeild i;
  }
}
```

Async iterators allow us to process a stream of generating data chunk by chunk.

## Callbacks

Core JS is **synchronous**, code is executed line by line and there's no 'wait' feature

**Async != Parellism** :- happens later vs. happening simultaneously. EL doesn't support parallelism ; webworkers do.

**Concurrency** :- when 2 or more "processes" happen in same window of time (rapid context switches) which gives illusion of parallelism. Event loop supports concurrency.

**Run to completion** :- JS code is broken up into two or more chunks (e.g `functions`) , where the first chunk runs completely _now_ and the next chunk runs _later_, in response to an *event*. <u>Macro-tasks peform modification on top of previous state, micro-tasks run within same state</u>

Callbacks are *fundamental* async pattern in JS. It is a function that event loop "calls back" into stack at a **later** time. This pattern has many flaws.

- **Inversion of Control** : they handover control & flow of code to other api with minimal ways to tackle unpredicted behaviour
- **Non-linearity** : express async flow in a hard to grasp non-linear way
- **Pyramid of Doom** : deeply nested timers to perform a series of async operations. 
- **Race conditions** : when some async callback may finish synchronously

All these result in **Callback Hell** where code is unmaintable, unpredictable, full of latent bugs and prone to edge-cases. To solve, we have
- **Promises**, that use *split-callback* design
- **Generators** let you 'pause' individual functions. They don’t follow run to completion.
- `async/await` wrap generators and promises in a higher level syntax.

## Promises

An object that represents eventual completion of 1 async task ; when settled, it can execute _callback handler_ asynchronously using **microtask** queue.

Solutions to callback hell
 - **Inv. of Control** :- we don't pass callback into 3rd party API, instead get a promise. To ensure we only get promise, use `Promise.resolve(api_call)`
 - **Race conditons** :- callback always run on mtqueue, even when errors

> A promise is resolved/rejected once and fixed forerver
##### Resolution
- non-promise: immediately fulfilled with value
- thenable: call onFulfilled recursively until a definite value is found
- promise:  create **new** promise that will *resolve* to original (hardlinked to eventual state)
- `Promise.resolve()` => return **original** promise (flattens promises)
##### Rejection
- thenable/promise :- call *onRejected* (no unwrapping)
- others :- create a **new** rejected promise that wraps reason

### Consuming Promises
| `then(onF,onR)` | `catch(onR)` | `finally(cb)` |
| ---- | ---- | ---- |
| handles both settlements; callback consume value/reason | handles all errors in previous then chain  | process something once promise is settled |
| returns `promise` fullfilled with onF/onR's *return* or rejected with onF/onR's *throw* | same as then | fulfilled value is *invoker* object ; cb takes `no param` |

__Floating promise__ :- when a then handler *does not return*, there's no way to track promise anymore. Next `then` handler is called *early* with `undefined` value.

### Error-handling

When a promise is rejected, 2 events can fire on `window` 
- each has `event.promise` and `event.reason` (the promise & its reason)
- these are
	- `rejectionhandled` : rejection was handled with a `.then()` or `.catch()` on promise. Used to see **intermediate errors** in a promise chain. !!! really useful.
	- `unhandledrejection` : rejection was not handled by current macrotask & its microtasks. It **"bubbles"** to top of call stack and host throws an `error` 
- try-catch-finally blocks are sync, so can't work on Promise.

TO PREVENT BUBBLING
```jsx
//in browser
window.addEventListner('unhandledrejection' , dealwithit);
```

### Promise Concurrency

4 static methods are available to run promises or operation in parallel. They take iterable of promises & return *1 promise* that will resolve based on how those promises are settled.
Catch if iterable is empty
```js
`all() & allSettled()` => immediately fulfillled
`any()`  => immediately rejected
`race()` => forever pending
```


**Drawbacks of Promises**
- single value or reason => needs array/object wrapping to send >1
- only way to pinpoint errors is 1 eventListener on window
- handlers trigger only once, unlike in events
- promisifying api takes effort => use a library

## Async / Await

Async/Await simplify the process of working with chained promises. 
`async` function return a Promise that will resolve to function's *actual return* value (when its available)

`await <promise>`
- suspends function execution until promise settles
- await becomes 'throw reason' when promise is rejected OR error occurs
- evaluates to *fulfillment* value of promise/thenable, ELSE to expr itself
- resolution similar to `Promise.resolve()`

```jsx 
await null; 
/* code to run later on mtqueue (after this tick) */
```

#### Error handling

```jsx
//always use 'await' to call functions for better tracing
await dosometask()

//use composers to chain Promises
wrong = [await p1, /*pause */ await p2]; // not caught by .catch()
right = Promise.all(p1,p2)
```

## ES6 Modules

A self-contained piece of code that encapsulates related functionality together. Written in separate files and imported & exported to other modules. 

__CommonJS modules__
`modules.export= getHello` => export
`const myvar = require('url')` => import 

##### Different imports
- static : `import ..` that can't be called conditionally.
- dynamic: `import()` or `require()` 
	- return a promise that resolves into a *namespace* object with exported values
	- called from any place
	- no `type=module` requirement
	- **not a function**, just a keyword

### ES6 modules
  - organize code into logical units, easier to maintain and reuse.
  - expose and use only relevant data/functionality
  - doesn't pollute global scope  ; no `globalThis`
  - use strict mode, auto-defered 
  - if name conflicts, neither value is exported
  - namespace object is a **sealed** object with null prototype

Imports are _live_ bindings and importing module _cannot change it's value_

```js
export {myfunc as default} //named to default

// combine import + export
export {default as f, g} from './no.js' 
export * from './nat.js'  // only re-export NAMED

// 4 types of import
import {f, g as google} from './dist.js' //named 
import mydef from './hi.js' //default  => give name
import * as tree from './test.js' // namespace
import "./test.js";  // side-effect
```

More info : https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules#top_level_await
#### Dependency evaluation
```js
// -- a.js (entry) --
import { b } from "./b.js"; //evaluate dependency b first
export const a = 2;

// -- b.js --
import { a } from "./a.js"; //a not declared yet, so ReferenceError if accessed
export const b = 1;

//CYCLICAL DEPENDECIES are errpr-prone
```

#### Top level await
In modules, `await` can be used at top level
Causes parent module to pause, until child finishes. In old browser, use IIFE async to simulate
```js
// parent.js  
import child from './child.js';

//child.js
const colors = fetch("../data/colors.json").then(x => x.json());
export default await colors;
```

##### Isomorphic modules
- runtime agnostic modules, achieved using dependency inversion
- modules  `binding.js` responsible for communicating with runtime should perform checks and insert polyfills / change methods according

---
#### JavaScript
- object-oriented scripting language
- runtime envr provides I/O ability to JS through objects
- ECMAScript : Standardized JS
- interpreted and JIT compiled
- single-threaded : Core JS has 1 call stack
- *dynamic* : runtime type binding ; unlike compile time viz. fixed

## V8's JIT compiler

At first execution, interpreter is used. On further executions, V8 compiles & optimise frequently executed code (Hot code) into **machine code** to improve performance. The compiled code is *re-optimized dynamically* at runtime, based on code’s execution profile.

**Engine Memory** 
- *Call stack* : 
	- a LIFO data structure that stores execution contexts, primitives and references to objects
	- _Execution context_ is an internal data structure that contains details about a function’s execution like value of `this`, local & closure variable, control flow's location, etc.
	- call stack is initialised with global context. When a function is called, it’s execution context is created and pushed to stack, and when it completes, it is popped
- *Memory Heap* : stores actual objects

_Parsing_ : Script is scanned left to right, line by line and converted into stream of tokens, control characters, line terminators, comments, or whitespace. 

Execution : takes place for each execution context,
 1. **Memory creation phase** allocate memory for variables and functions
 2. **Code execution phase** : values are computed and assigned to variables (wherever possible). 

### Garbage Collection

- performed automatically. We cannot force or prevent it.
- garbage collector — a background process that monitors all objects and removes those that are _unreachable_ via chain of references from **root**
- a root is a data which is never garbage collected — global bindings and local bindings of current chain of nested function calls.
- To delete an object, we must delete all _incoming_ references to it. 
- _Unreachable island_: when a pack of interlinked objects is cutoff from root references.

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
`const` : can’t be bound to another reference during runtime
##### Scope Chain
- Each scope can access values in all its enclosing scopes but not vice-versa. This approach is called **lexical scoping** 
- names in inner scope **overshadow** outer ones. Using var to overshadow let/const is `illegal`
##### Hoisting
- process where, before code execution, interpreter relocates _func/class/variable/import_ declarations to `top` of their scope. 
- Only functions are hoisted with their values. 
- `let/const/class` remain in TDZ until their declarations are reached during program flow. Any attempt to access during TDZ throws error.
##### Lexical Environment
- A specification object for a *executing* function/block/script. It has 2 keys
	1. **Environment Record** : local variables are its properties, also store `this : value`
	2. A reference to **outer LEX** object (or null)
- ERec is populated with variables during allocation phase, but are uninitialised until declared. (why TDZ exist)
- JS engine traverses LEX chain to find variables 

---
### Event Loop, Macrotask, Microtasks

JavaScript execution flow is based on an *event loop*. General algorithm of event loop :- 
1. If call stack is empty and **task queue** has a *macrotask*, deque and execute it
2. execute all *microtasks* placed in **microtask queue** by macrotask in (FIFO)
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

**Scheduling Execution**
- Browser starts enforcing **atleast 4ms** delay if same callback has been scheduled **5 or more** times.
- Timer may slow down
    - on tabs that are inactive or streaming or loading page
    - OS power saving settings
- timer continues “ticking” while showing alert/confirm/prompt.
- **Zero delay scheduling** is used to execute code immediately after current task

**Web Workers**

Run code in different parallel thread. Can exchange messages with the main process, but they have their own variables, and their own event loop.  JS never shares data across threads

Web Workers do not have access to DOM, so they are useful, mainly, for calculations, to use multiple CPU cores simultaneously.

---