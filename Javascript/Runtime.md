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

---
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

**Scheduling Execution**
- Browser starts enforcing **atleast 4ms** delay if same callback has been scheduled **5 or more** times.
- Timer may slow down
    - on tabs that are inactive or streaming or loading page
    - OS power saving settings
- timer continues “ticking” while showing alert/confirm/prompt.
- **Zero delay scheduling** is used to execute code immediately after current thread has executed

**Web Workers**

Run code in different parallel thread. Can exchange messages with the main process, but they have their own variables, and their own event loop.  JS never shares data across threads

Web Workers do not have access to DOM, so they are useful, mainly, for calculations, to use multiple CPU cores simultaneously.

---
