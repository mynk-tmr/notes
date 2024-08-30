**Main Features**
- object-oriented scripting language
- prototype based inheritance
- interpreted and JIT compiled
- single-threaded : Core JS has 1 call stack
- dynamic/runtime type binding of variables

**Primitive datatypes** 
1. **Number** : stored in 64 bit double precision.
2. **Bigint:** stored in arbitrary precision. For Integers larger than `2^53 -1 (15 digits)`
3. **Boolean**: Represent a _logical entity_ and can be true /false.
4. **Null**: Shows something that does not exist. Has only one value: _null._ It's Primitive even if `typeof null == 'Object'`
5. **Undefined**: initial value of unassigned variables, params, non-existant properties
6. **String**: a set of UTF-16 characters (16-bit unsigned integer (0-65536)). They are *immutable* array-like objects.
7. **Symbol:** a primitive type for unique *identifiers*. Unlike others, it does not have any literal form. Allow us to create **“hidden” properties** of an object, that no other part of code can accidentally access or overwrite. 

**Non-Primitives**
1. **Object**: structured as a list of _key-value_ pairs.
2. **Array**: ordered list of values
3. **Map**: a collection of *unique keyed* items, where key can be *any* type. Plain Object allows only `String` or `Symbol` keys
4. **Set**: a iterable collection of *unique* *values* of any type 
5. **WeakMap & WeakSet**: use weak references to `Object` keys, so they can be garbage collected. They only have `get` `set` `delete` `has` methods and no `size` and aren't iterables

**Numeric literals**
```js
0o33; 0xff; 0b11; 0.255e3

-Infinity +Infinity //uncomputable numbers like 0 division
```

Floating point math is inaccurate because numbers are stored as approximations

**NaN** : a special number that represents failed conversion, imaginary or indeterminate number (`Infinity  or undefined` involving maths). `NaN =/= NaN`

**BigInts**
- Uses **64 bits**, where 63rd is signed, 62-52 store coefficient and rest store power (fraction)
- cannot represent decimals. BigInts can’t use `>>>` , since they don’t have fixed width, or highest bit.
- can be **compared** to numbers and parsed to int, float.

```js
a = 1n + 2; // TypeError: Cannot mix BigInt and Number
isNaN(3n) // TypeError, forced 3n to number
5n / 2n; // 2n
```

**Template literals**: special strings that allow embedded expressions `${}` and preserve indentations
**Tagged templates**: allow you to **parse** template literals with a function. Evaluates to function's return

```jsx
function f(st_array, ...tags) {}
```

**Symbols**
* skipped by almost every method except `Object.assign` and `structuredClone`
* don’t auto-convert to string. Explicitly use `.toString()` or `symbol.description`
* **“system” symbols** — that JavaScript uses internally,  accessible as `Symbol.**`. Can be used to modify built-in behavior and fine-tune objects. 

```jsx
let id = Symbol("id")  // create symbol with optional description/name
usa[id] = 'fbi' //doesn't override 'id'

// If you want same-named symbols to be equal, use the global registry.
let sym1 = Symbol.for('monkey') //creates global symbol
let sym2 = Symbol.for('monkey') //returns it, No new symbol

Symbol.keyFor(sym1)  //monkey ; for non-global -> undefined

//Get Symbols
Object.getOwnPropertySymbols(obj) //get all symbols
Reflect.ownKeys(obj) //return all keys including symbolic ones.
```

```js
const obj = {
	[Symbol.toPrimitive](hint) { //'string'/'number'/'default'
		//return primitive equivalent ; 
	},
	*[Symbol.iterator]() { 
	    for(let i = 0; i <= 100; i++) yield i; //return iterator object
	}
}
```

**Equality Testing**
- **sameValue**: same type and value . Used by `Object.is`
- **sameValueZero**: `+0 == -0` e.g. Map methods
- **Object equality** : 
	- referential equality ie. refers to same location in memory
	- loosely equal to primitive counterparts
	- Primitives are tested by value.
	- `empty_obj != {} ... not same memory`
	- `Object.keys(obj).length == 0  //correct way`

**Keyed collections**
- data structures *ordered by key* not index. e.g. Map and set objects 
- associative in nature and use *SameValueZero* check (NaN can be key).
- iterable in order of insertion

```js
new Map(iterable) //Set
.size ; map.set(k, v) set.add(v) ; //returns new set/map

.get .has .delete .clear

map.keys() .values() .entries() //unlike Object.keys() ; returns real array not array-like

new Map([[1, 'hello'], [2, 'bye']]); //for key-values

// map to object & viceverse
Object.fromEntries(map) 
new Map(Object.entries(obj))

let john = { name: "John" }, arr = [ john ]; 
john = null; // original isn't garbage-collected [arr[0]] ; // to prevent this, use weakmap, weakset
```

`WeakMap` and `WeakSet` are used as “secondary” data structures for “primary” object storage. Use cases :
- extra data about objects, which auto-deletes when object dies
- memoizing /caching 
- data about sealed/alien objects

```jsx
// Symbol can be used too for this, only our code can see this
let isRead = Symbol("isRead");
messages[0][isRead] = true;
```

**THIS keyword** 
At function execution, a property `this` is assigned to function object which refers to current execution context. It depends on how function is called.
* **object literals** : inhertied from enclosing context (scope)
* **event handlers:** `currentTarget` ; if inline html refers to element

**How to evaluate expressions ?**
- decide order of operations -> by *precedence*
- when same precedence -> *associativity* . Almost all are left-to-right

```js
`Method / Props` : fn(), obj.x, new Box()
`Unary` ++x ; `Rest of them with same precedence`
`Arithmetic Power > DM > AS`
`Bitwise shifts`
`Compare` : in  instanceof  <= 
`Equality`: == != ===
`Logical`: & ^ | && || ??
`Assignment & Returns` : =>  x:'s'  +=  ...spread, yeild, return 
`Comma operator` : evaluate all, compute to last expression's result
```

`case: some_val ` tests for *strict* equality ; case can be enum values / string object

```js
delete obj[prop] //true except if prop is own non-configurable
void op //evaluate op and return undefined
```

**Loops**
- `for` evaluates step at end of current iteration
- `for in` iterates over *enumerable* properties of Object
- `for of` iterates over *values* of **iterable** objects. Both use pass by **value** to iterate 
- `for await(.. of ..)` : iterate over async or sync objects
- `onix : for(key in obj)` : onix is **label**, an identifier for codeblock that you can refer to elsewhere
- `break` terminates current (or labelled) loop
- `continue` *jumps* control-flow to next iteration of current (or labelled) loop. In for, it updates STEP. Given label must be LOOP. You can only jump to **ancestor** label

**Ternary/conditional operator**
- acts on 3 operands and works like an *inline-if-else*.
- returns value of expr chosen to be evaluated.
- can't work with `break` `continue` `return` (can't control flow)

**Exception Handling**
* An *exception* is a undesirable runtime event, that brings the execution to a halt.
* All errors are *serializable* object with props `name` , `message` , `stack`
* **Stack trace** : list of function calls that lead to an error -> latest to earliest
* **Try** : 
	* creates a codeblock where any error transfers control to *first catch block* in call stack. 
	- for user-defined exception `throw val` 
	- Error object is visible only in `catch` block. 
- **Finally** : always execute immediately after `try` and `catch` blocks. Overrides rethrows/returns in `try-catch` with its own return (if given)
- **Types**
	- `ReferenceError` — accessing undeclared or uninitialized variables
	- `SyntaxError` — incorrect syntax
	- `TypeError` — invalid operands/arguments; invalid use or change to a value
	- `StackOverflowError` — when number of execution contexts exceed size of host’s stack 

**Extending Built-in Objects** : adding new properties to `prototype` of Class
```js
Date.prototype.nextDay = function() { 
	return new Date(this.setDate(this.getDate() +1))
}
```

**Short-Circuit Operators** — **&&, ||, ??, ? , &&= etc.**
* always evaluate *left* operand first, and then right only if needed
* Only affects **evaluation order** of operands, not final result  
* If multiple short-circuits, they are **grouped** but circuits evaluates left to right only.
* `&& || ??` returns **first** falsy/ truthy/ non-nullish operand 
* `?.` short circuits expression to `undefined` if left operand is *nullish*
* **falsy**: `0`, `''`, `NaN` , `null` `undefined` ; **truthy**: every other

```jsx
let A, B, C return false, false, true
C() || B() && A() // only C called
A() && C() || B() // A & B called
```

**Bitwise Operators**
* operate on individual bits of operands. They treat them as signed int, but result as signed `number`.
* -ve integer : 2's complement (invert bits; add 1)
* **and (&) or(|)  xor(^)** : return 1 for each bit positions where both 1 / atleast one is 1 /  both same.
* `<<` left-shit bits (pad right with 0) ; `>> or >>>` signed / unsigned right shift (pad left with sign bit / 0)

**postfix/prefix**
- evaluates to a value, not a reference (so no chaining)
- In while & do while loops, *postfix* runs *1 extra* than prefix.

**Spread syntax** 
* "expands" an *iterable* into its elements OR an object into its *own_enum_key : value* pairs
* can be used anywhere.
* falsy & non-iterables spread nothing. 
* cannot mutate object, cannot trigger setters, cannot copy prototype, unlike `Object.assign()`

**Rest syntax** 
* collects multiple elements and "condenses" them into a single array. 
* Allowed on only last parameter

**Destructuring Assignment**
* unpacks values from *objects & iterables* into distinct variables. 
* Default values apply only for `undefined` and are lazily evaluated.

```js
let [a, ...r] = 'hey'; // a= 'h' ; r = ['e', 'y']
//swap
[arr[1], arr[0]] = [arr[0], arr[1]]

let {
	x : prop, 
	[f()] : var3, //f() returns some prop_name
	fullName: { lastName : [a,b,c] } 
}  = myOBJECT;
```

**Property Descriptors**
* describes property of object via a flag object
* data property :- *no getter, no setter*
* accessor property :- 
	* *no value*, no writable ; use when value is dynamic like date-dependent
	* getter & setter are automatically called

```js
flags  = {
  value : 'Mayank', // for property 
  writable : true, // value editable ; if config-false, can be set to false, but not to true
  enumberable : true, //seen by loops
  configurable : true, // deletable, attr modifiable
  get() {},
  set() {}, 
}
```

**Function types**

```jsx
function add(a,b) { a+b } //Function Declaration [separate stmt]
hero = function () {..}   //Function Expression [created as part of expr]
bar = function foo() {..} //NFE : named func expr
mk = new Function('...args' , '<body>');  //the New Syntax
```

Note
* **Why NFE?** Can be used in recursive calls. We can safely assign bar to other variable
* Function expressions aren't hoisted.
* **new syntax** function is always created in **global** **scope** and can only refer to global values. `eval(str)` can refer to local variables and change them. Avoid using both

**Properties available in function scope**
- `arguments`
- `length` : no. of args, _excluding_ …rest params.
- `new.target` : **true** if function was called with new, otherwise false.
- `name` : function name, same as provided in declaration / assignment

**Function Parameters**
- _default_ : for `undefined` ; lazy
- …_rest_ parameter collects **indefinite** no. of args into 1 **array**
    - `arguments` is array-like, while rest parameter is real
    - `arguments` maintains all parameters, rest only uncollected ones
    - In some cases, `arguments` syncs with parameter changes. Rest never updates on changes.

**User-defined Properties** `myfun.prop = expr/func;`
- To lessen pollution of global space or to avoid name conflicts.
- To track calls and meta-data related to function

**Arrow Functions**
* anonymous functions that inherit `this` from outer scope ie., they are **statically** bound to the **enclosing** context where it is **defined.**
* Pros : 
	- predictable behaviour w.r.t closures / `this`. 
	- `this` : when in object (bound to it) ; when `static` bound to class
	- don't lose `this` when passed as callbacks
	- In non-arrows ,  `this` is runtime bound; either invoker object OR globalThis 
- Limits :
	- don’t have `arguments` , `new.target` or `super`
	- context can’t be changed (`call,bind,apply, thisArg`) and is silently ignored. Hence, **can’t be shared** via prototypal inheritance
	- can’t be used as constructors

**Strict mode `‘use strict’;`** 
- silent errors may *throw* e.g. invalid assignments, invalid deletion
- no `globalThis` substitution (now `undefined` ). Can’t skip `var`
- **block-scoped function declarations** (emulate private methods)
- strict functions don't allow rest, default, or destructuring
- use `delete window.x` for global variables
- Non-leaking eval : `eval("var x;")` ⇒ variables are scoped to eval only
- Immutable `arguments` : stores original parameters
-  run *faster* than identical sloppy code
- Prohibits some syntax for *future-proofing*
- Easier to write *‘secure’* Javascript e.g. can't access `callee` , `caller` and `arguments` in strict fn

**Call, Apply & Bind**
* allows changing the context of functions (`this` object inside them) 
* `fn.call(ctx, ...args)` : invokes function with given `this` value and arguments
* `fn.apply(ctx, array-like)` (faster, more optimised)
* `fn.bind(ctx, ...args)` : copies the function but with given bindings. The binding is immutable and call, apply, bind won’t work later.
* Function properties aren't passed on.
* Supports
	* **Call forwarding** : a technique that allows a function to pass on its context & args to another.
	* **Method borrowing** : calling a method from an object in context of another object.

```jsx
let f = function (x,y,z) { g.apply(this, arguments) } //call forward
Array.prototype.forEach.call('Hello World', cb); //borrow

//binding `this` of non-arrows
setTimeOut(ob.func.bind(ob))
```

**Functional Programming**
* a declarative programming style where one applies pure functions in sequence to solve complex problems
* Concepts
	* **first class** : functions treated like variables
	* **callback** : a function passed into another as an argument and then invoked inside it
	- **higher-order functions** : consume and/or return functions. They abstract actions and used in composition
	- **decorators** : take another function and adds “features” to it without affecting function’s body. 
	- **referential transparency** : ability to replace function with result without breaking code
	- **arity**: more parameters -> harder to break apart functions
- v/s OOP
	- uses pure functions and immutability of data ; instead of shared state in OOP
	- complete separation between data & behaviors of a program. 
	- FP is more declarative and OOP is more imperative
- Problems in Inheritance (Why use Composition)
	- **tight coupling**
	- **fragile base class problem** : Changing one small thing in either of the class or subclasses could break code
	- **duplication** : need to create a subclass to do 1 extra thing, but get everything passed down to it.

**Pure functions** 
* where return values are identical for identical arguments
* Features
	* **idempotent** : can be executed several times without changing the result beyond its first iteration
	* no-side effects or global state mutation
	* no shared state 
	* return something
	* composable
* Benefits (same as FP)
	* easy to extend, maintain, reuse, cache results, memory efficient, minimize side effects

**Currying**
* A technique that transforms a multi-arg function into several **single-arg functions**
* possible because of **closures
* Uses
	- generate single purpose functions that are easy to maintain and debug
	- flexible invocations ⇒ sum(1,2)(3) or sum(1)(2)(3) etc…
	- Composition / Piping

**Partial application** 
*  function which has been applied to some, but not all of its args (some args are fixed in closure)
* transforming functions from higher to lower arity through currying with `bind()` 
* helps to avoid having to repeat some args in calls.

```jsx
double = multiply.bind(null, 2);
```

**Composition / Piping** 
* a technique to use a HOF to chain functions in particular order
* Why
	* improves readability and reusability of code, 
	* can chain same set of functions in various ways to solve different problems
* composition : chain RTL ; pipe : chain LTR

```js
const pipe = (...fns) => (input) => fns.reduce( (res, fn) => fn(res), input);
// pipe(mult3, add2, sub5)(9)

const pipeAsync = (...fns) => (input) =>
    fns.reduce((chain, fn) => chain.then(fn), Promise.resolve(input));
```

**Immutability**
* one of the building blocks of FP. It allows you to write safer and cleaner code
* immutable value : cannot be changed once created.  e.g. PRIMITIVES
* objects and arrays are mutable — their properties/values can be changed 
* Why use immutability
	* better performance
	* reduce memory leaks (make object references instead of cloning)
	* thread-safety (multiple threads can reference the same object without interfering)
* Done by `Object.freeze()`

**Closure**
* What
	* combination of a function and the lexical environment within which it was declared.
	* when functions are created, they **capture** variables in their outer scope
	* functions created in **same run** share the closure, but in different runs do not
- LEX : specification object referred by function's `[[Environment]]` property, and through LEX, a function can access all variables in chain of scope
- Uses :  **FP + Encapsulation**

```js
const usePeople = () =>
	let people = [] //private property
	const add = name => people.push(name)
	const getAt = idx => people[idx]
	return { add, getAt }
```

**Mistakes**
* using `var` or outer `let/const` as iterator : Functions created in loop will refer to end value of variable

**Stale closures :** capture variables that have outdated values. Created when variables aren't updated by functions
```jsx
function add() {
	let a = 0, msg = `a is ${a}`
	const inc = () => ++a; //inc() inc() 
	const log = () => msg; //knows a is 2, but `a is 0` due to stale msg
	return [inc, log];
}

//to fix, make the stale dynamically evaluated
const log = () => `a is ${a}`;
```

**Prototypal Inheritance**
* model where objects inherit from other objects, instead of classes. Done via using linking `.prototype` property of objects
* Features
	* Objects inherit all properties **up the chain**, but can **shadow** them with **own** properties
	* at runtime, property lookup starts at object and goes up the chain, until a match is found.
- Cons
	- no multiple inhertiance ⇒ allows only 1 parent prototype
	- changing prototype of a constructor breaks chain
- Note
	- `prototype` : object that an object inherits or descends from.
	- `[[Prototype]]` : hidden internal property that references prototype.
	- `__proto__` : accessor property that exposes `[[Prototype]]` and allows you to modify it. Modern method is `getPrototypeOf, setPrototypeOf`
- Constructor Functions
	- have an internal `[[Construct]]` method and `.prototype` property (to serve as instance prototype)
	- When `new func()` evaluates, constructor creates an empty object, set its prototype to `func.prototype` , then function executes with `this` set to object created
	- If no return, returns `this` OR our explicit object

```jsx
Object.prototype.__proto__ === null //this has no prototype
```

Classes are based on prototype-based *constructor functions*, with enhancements
 - `constructor()` is explictly defined & requires **new** to be called
 - class methods are **non-enumerable**
 - have **strict-mode** and **TDZ** 
 - **super** keyword is used to *access* *public* props in superclass, or invoke its constructor
 - have *private* fields that aren't inherited and only accessible in Class definition
 - **extends** as syntactic sugar of `Object.setPrototypeOf()`

NOTES
- derived constructors have no initial `this` binding ; upon `super()` , `this = new Base().`
- `super.func()` uses `this` around function call not super-class

**Object Composition** : copying properties from one or more objects to another, without retaining a reference between them. It relies on `Object.assign()`

**Generator Functions**
* can **pause** and don’t follow run to completion. 
* Only `*f` can pause itself with `yeild` and only its **own generator object** can resume it with `next()`
* It is a control + communication mechanism. `*f` once returns, is marked complete

```jsx
function* f() {
	for(let i=0; i<10; i++)
	let response = yeild i; //send {value:i, done: false} 
	return 3; // send {done: true} , no value
}

const it = f(); //create iterator object
it.next() //start *f ; 1
it.next('ok') //response = 'ok' ; 2
it.return('quit') // {done: true, value: 'quit'}
it.throw('error') //throw error in *f

[...keygen] //0,1,2....
```

**Generator composition** : `yeild*` can be used for it. It “embeds” one generator into another by delegating execution to another generator function
```jsx
function* innerGenerator() {
	yield 1; yield 2; yield 3;
}
function* outerGenerator() {
	yield* innerGenerator(); //as if inner's body embeded here
}
```

**Iterators**
- objects that abide by iteration protocol ie., 
	- have `next()` , optionally `throw()`,  `return()`
	- Each takes atmost 1 arg & return `{value, done}`  (IteratorResult interface)
- async iterators allow us to process a stream of generating data chunk by chunk.

**Iterables**
- objects that iterators can iterate upon. They implement `[Symbol.iterator]` and return iterator
- a iterator can become iterable (return `this` in `Symbol..`) e.g. generator object
- Arraylike have index and length but aren't always iterable

```js
Array.from(obj, ^mapFn, ^thisArg) // convert iterables/arraylikes
```

|                   | Iterable                                         | Async iterable                           |
| ----------------- | ------------------------------------------------ | ---------------------------------------- |
| Object implements | `Symbol.iterator`                                | `Symbol.asyncIterator`                   |
| `next()` returns  | {value, done}                                    | `Promise` that resolves to {value, done} |
| to loop, use      | `for..of` which runs next()<br>until `done:true` | `for await..of`                          |
| features          | can be `...spread`                               | NO                                       |


**Callbacks**
* fundamental async pattern in JS. It is a function that event loop "calls back" into stack at a **later** time.
* Flaws
	- **Inversion of Control** : they handover control flow to other api (unpredictable)
	- **Non-linearity** : flow is hard to grasp
	- **Pyramid of Doom** : deeply nested timers to perform a series of async operations. 
	- **Race conditions** : when some async callback may finish synchronously
- The flaws result in **Callback Hell** where code is unmaintable, unpredictable, buggy. Therefore, use
	- **Promises** 
	- **Generators** let you 'pause' individual functions.
	- `async/await` = generators + promises

**Promises**
* An object that represents eventual completion of 1 async task
* When settled, it executes a _callback handler_ asynchronously using **microtask** queue.
* Solves callback hell
	 - **Inv. of Control** :- we don't pass callback into 3rd party API, instead get a promise
	 - **Race conditons** :- callback always run on mtqueue, even when errors
 - How resolved ?
	- non-promise: immediately fulfilled with value
	- thenable: call `onFulfilled` recursively until a definite value is found
	- promise:  create **new** promise that will *resolve* to original (hardlinked to eventual state)
	- `Promise.resolve(x)` => return **original** promise (flattens promises)
- How rejected ?
	- thenable/promise :- call *onRejected* (no unwrapping)
	- others :- create a **new** rejected promise that wraps reason
- __Floating promise__ :- when a then handler *does not return*, there's no way to track promise anymore. Next `then` handler is called *early* with `undefined` value.

| `then(onF,onR)`                                                                                    | `catch(onR)`                              | `finally(cb)`                                                                                 |
| -------------------------------------------------------------------------------------------------- | ----------------------------------------- | --------------------------------------------------------------------------------------------- |
| handles both settlements; callback called with value/reason as argument                            | handles all errors in previous then chain | process something once promise is settled. `cb` takes No parameter                            |
| returns **promise** fullfilled with onF/onR's return or rejected with error/rejection in callbacks | same as then                              | returns **pending** promise that settles with previous value or error / rejection in callback |

**Error-handling in Promise**
* When a promise is rejected, 2 events can fire on `window` 
- `rejectionhandled` : with a `.then()` or `.catch()` on promise. Used to see **intermediate errors** in a promise chain. !!! really useful.
- `unhandledrejection` :  
	- by current task & its microtasks. It **"bubbles"** to top of call stack and host throws an `error` 
	- To handle bubbling, use `eventListener()`
- properties of events : `promise` & `reason`
- try-catch-finally blocks are sync, so can't work on Promise.

**Promise models / How to run async tasks in parallel ?**
* 4 static methods are available for this. They take `iterable` of promises & return *1 promise* that will settle based on how those promises are settled.
* `all():` all must fulfill. Return -> value array / rejection reason
* `allSettled()`: all must settle. Return object array with `value/reason` and `status` ('fulfilled' / 'rejected')
* `any()`: fulfills as soon as 1 fulfills. Returns -> value / `data.errors[]` containing rejection reasons 
* `race()`: settles as soon as 1 settles. 
* If iterable is empty
	* `all() & allSettled()` => immediately fulfillled
	* `any()`  => immediately rejected
	* `race()` => forever pending

**Drawbacks of Promises**
- single value or reason => needs array/object wrapping to send >1
- only way to pinpoint errors is 1 eventListener on window
- handlers trigger only once, unlike in events

**Async/Await**
* simplify the process of working with chained promises
* `async` function returns a *new Promise* that will resolve to function's *actual return* value or rejected with an exception uncaught.
* they can have `await <promise>` expressions that suspend function execution until promise settles
* Benefits
	* `try/catch` support ; sync like async code

**`await <promise>`**
- await becomes `throw reason` when promise is rejected OR error occurs
- it behaves like `Promise.resolve()` and evaluates to **definite value** 
- await null :  rest of function will run later on mtqueue
- always use `await Promise.all`, instead of `[await p1, await p2, ..]`. Later isn't caught by catch()

**Modules**
* self-contained pieces of code that encapsulates related functionality together. 
* written in separate files and export / import values
- expose and use only relevant data/functionality. Solves namespacing problems

**IIFE Module pattern**
* used earlier to emulate modules. Had problems 
	* naming conflicts if you don't use const to declare module.
	* dependency issues if scripts are placed in the wrong order

```js
const calculator = !function(w) {
	const add = (a,b) => a+b;
	const show = (x) => w.alert(x);
	return {add, show}
}(window)

calculator.add(1,2)
```

__CommonJS modules__
`modules.export = {}` => export
`const myvar = require('url')` => import 

**ES6 Modules**
- doesn't pollute global scope  ; no `globalThis`
- use strict mode, auto-defered 
- if name conflicts, neither value is exported
- Imports are **live** bindings and importing module cannot change it's value
- To use in browser, add `<script type="module">`
- **static** : `import ..` that can't be called conditionally.
- **dynamic**: `import()` or `require()` that can be called from any place
	- return a promise that resolves to a **namespace** object containing exports. It's a **sealed** object with null prototype
	- not a function, just a keyword

**Import / Export types**
* **default** : can be imported with any name
* **named**: " " with same name
* **namespace**: `import * as` 
* **side-effect**: `import 'file.js'`

**Top-level await :** In modules, `await` can be used at top level. It causes parent module to pause, until child finishes.
```js
// parent.js  
import child from './child.js';

//child.js
const colors = fetch("../data/colors.json").then(x => x.json());
export default await colors;
```

**Cyclical Dependencies** : error-prone
```js
// -- a.js (entry) --
import { b } from "./b.js"; //evaluate dependency b first
export const a = 2;

// -- b.js --
import { a } from "./a.js"; //a not declared yet, so ReferenceError if accessed
export const b = 1;
```

**Isomorphic Modules**
- runtime agnostic modules, achieved using dependency inversion
- How? create a module  `binding.js` to communicate with runtime. It must perform checks and insert polyfills / change methods accordingly


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

`var` : no Block scope, no TDZ, redeclarable, creates a property on `window`
`const` : can’t be bound to another reference during runtime
##### Scope Chain
- Each scope can access values in all its enclosing scopes but not vice-versa. This approach is called **lexical scoping** 
- names in inner scope **overshadow** outer ones. (Err! block var trying to overshadow let/const)
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

**Garbage Collection**
- performed automatically. We cannot force or prevent it.
- garbage collector — a background process that monitors all objects and removes those that are _unreachable_ via chain of references from **root**
- a root is a data which is never garbage collected — global bindings and local bindings of functions in call stack
- To delete an object, we must delete all _incoming_ references to it. 
- If object is *weakly referenced*, it can be garbage collection if it is unreachable via hard link

**“mark-and-sweep”** : basic garbage collection algorithm
1. “mark” (remember) roots
2. mark all objects they refer to
3. visit marked objects and mark their references. 
4. repeat 3rd step until no objects can be marked
5. remove all 'unmarked' objects

JS engine apply many **optimizations** to speed it up
 - **Generational collection** – objects are split into two sets: “new ones” and “old ones”. Objects which remain in memory for long are considered old and examined less often.
 - **Incremental collection** – engine splits objects into multiple parts and then clear these parts one after another.
 - **Idle-time collection** – garbage collector tries to run only while the CPU is idle

**Memory Leaks**
* when unused data on heap memory isn't garbage collected, leading to more memory usage over time. 
* Effects -> poor performance, slow response times, crash
* Causes
	* undeclared / global variables
	* forgotten timers  & callbacks : clear timers & don't capture objects in closure
	* Multiple event listeners : they form closures and capture DOM elements even if removed from UI. Hence, use `removeListener()` or `{once : true}` flag

**Concurrency Model of JS**
* Based on an “event loop” to carry out non-blocking I/O operations. It offloads certain operations to the system kernel whenever possible.
* Core JS is **synchronous**, code is executed line by line and there's no 'wait' feature
* Concepts
	* **Parellism:** happens simultaneously. e.g. Web Workers
	* **Concurrency :** when processes happen in same window of time (via rapid context switches) which gives illusion of parallelism
	* **Asynchronous:** code that runs later. Not same as Parellism. 
* **Run to completion** :- JS code is broken up into chunks (e.g `functions`) , where 1st chunk runs completely _now_ and the next chunk runs _later_, in response to an *event*.
* **Queues**
	* **Job / Microtask queue** :- `promise` handling, `await` calls, `queueMicrotask(func)` 
	* **Callback / Task queue** :- `script` execution, `event` handlers, callbacks in  `setTimeout ,setInterval`
	* **Animation queue** :- callbacks put by `requestAnimationFrame()`
* Tasks peform modification on top of previous state, micro-tasks run within same state

**Event Loop Working**
* If call stack is empty, pop first task in **task queue** into stack for execution
* execute all **microtasks** created by task (empty the job queue)
* execute all callbacks in **animation** task queue
* repaint the UI
* do network requests / play audio etc
* Repeat step 1

**setTimeout Web API**
* browser starts enforcing **atleast 4ms** delay if same callback has been scheduled **5 or more** times.
- Timer may slow down
    - on tabs that are inactive or streaming or loading page
    - OS power saving settings
- timer continues “ticking” while showing alert/confirm/prompt.
- **Zero delay scheduling** is used to execute code immediately after current task


**Web Workers API**
* feature that allows for multi-threading in JavaScript 
* **main thread** : where browser runs all JS, processes user events, painting, garbage collection. Long running function -> blocks page
* **worker threads**: where a web worker runs code in parallel to thread that spawned it in background.
	* data is only copied among threads (not shared for thread safety)
	* have own event loops
	* limits : no DOM access, no access to `window`, not all web APIs are available
	* uses `self` to access worker scope, not `window`
* Use cases : 
	* complex computations like maths ; network requests
	* indirect DOM using **message pipeline**

```js
let worker = new Worker("worker.js") //file run on worker thread
worker.postMessage('hello worker')
worker.addEventListener("message", ({data}) => { // data : "Hello"
	worker.terminate()
})

//worker.js
addEventListener("message", () => { postMessage("Hello")})
```

**Shared Workers**
* a `Worker` can only be accessed from script that created it, a `SharedWorker` can be accessed by any script that comes from the same domain (different windows, iframes, workers.)
* uses an explicit `port` object to communicate 

```js
const worker = new SharedWorker("worker.js");
worker.port.start()
worker.port.postMessage({ x : 3, y : 2});

//worker.js
addEventListener("connect", (e) => {
  const port = e.ports[0];
  port.start()
  port.onmessage = (e) => console.log(e.data)
})
```


**Service Workers**
* act as proxy servers between webapp, browser and network (when available)
* used to create **offline** webapps, executing code based on if network is available. 
* allow access to **push** notifications and background **sync** APIs

## MATH 

Note : only `static` methods and can't be used on BigInt

```js
Math.PI
Math.abs(-39)
Math.sign(9) /* -1, 1 or 0 */
Math.sin(radian) ; Math.asin(value) 
Math.log(num); Math.log10(num); Math.log2(num) //num >0
Math.floor(float) ; Math.ceil(float)
Math.min(...args) ; Math.max(...args)
Math.round(3.444) ; Math.trunc(3.444)
Math.sqrt(9) ; Math.cbrt(8) ;
  
//random integers in range [a,b]
mod = b - a + 1; //remove 1 to exclude b
Math.floor(Math.random() * mod) + a;
```

## DATE 

In a date object, time is static and stored as ms from Jan 1, 1970. It does not have any properties, only methods.

```js
new Date() //current time obj
Date.now() //ms timestamp
new Date(0); //1 jan 1970 ; ms
new Date(-86400_000); //31 dec 1969
new Date("1995-03-25T13:33:55.383") 
new Date(1995, 3, 25) //omitted set to 0
ms = Date.parse("Aug 9, 1995") //parsed date for set* methods

//month & sunday start from 0
```

Smart objects
```jsx
ti.setDate(feb_28.getDate() + 2) //mar 1 or mar 2 (auto-leap-check)
why.setMinutes(why.getMinutes() + 25) //+25 minutes
dt.setDate(0) //18 apr -> 31 march

dt - why //subtract ms

function getLastDayOfMonth(year, month) {
  let date = new Date(year, month + 1, 0);
  return date.getDate();
}
```

__Date object Methods__
- set* => accepts *ms/parsed* date => setHours(), setFullYear()
- get* => return in *local* time => getHours()
- to* => human readable form => toLocaleDateString(), toLocaleTimeString()


**Utilites**
```js
// Returns days left in the year
const endYear = new Date(2024, 0, 1); 
let daysLeft = (endYear.getTime() - Date.now()) / 86400_000;
daysLeft = Math.round(daysLeft);

getWeekDay = (date) => {
	let days = ['SU', 'MO', 'TU', 'WE', 'TH', 'FR', 'SA'];
	return days[date.getDay()]
}

function secondsLost() {
  let d = new Date();
  return d.getHours() * 3600 + d.getMinutes() * 60 + d.getSeconds();
}
```
  
## OBJECT   

```js
Object.preventExtensions(obj) //no new props
Object.seal(obj) // no (+) (-) props , sets config-false for all
Object.freeze(obj) // seal and write-false all

//test
Object.isExtensible(obj)
Object.isSealed(obj)
Object.isFrozen(obj)

//group object based on cb's return for each obj
Object.groupBy(arr_of_obj, cb) //return sync with original

Object.entries(obj) //2D array, with a entry as [own_enum_prop, value]
Object.fromEntries(iterable)


//inherit
Object.getPrototypeOf(dog) // Function : animal
dog.hasOwnProperty("type") 
"type" in dog  // true if own or inherited 
```

**property descriptors**
```js
Object.defineProperty(obj, 'name', flags) //will overwrite
Object.defineProperties(obj, {name: flags}) //multi
Object.getOwnPropertyDescriptor(obj, 'name')
Object.getOwnPropertyDescriptors(obj)

//flag aware cloning -- include all symbols, properties, flags
let clone = Object.defineProperties({}, Object.getOwnPropertyDescriptors(obj))
```

```js
Object.assign(target, obj1, obj2) 
//copies enum own properties only, calls [get] on sources, [set] on target
//can't copy accessor props
//in errors, target upto error is modified

// create object with given prototype
child = Object.create(parent, inits); //like {id: 1, name: 'ds'}

structuredClone(obj) //deep copy of nested objects & circular references
```

## Array

```jsx
box = new Array(40); //length -> 40
box2 = Array.of(9.3); // 1 element 9.3
box.length = 5 ; // truncate array , 0 empties it

//2D arrays 
const arr = [[1,2]]

//redefine arr to Object ; length is unaffected
arr['hello'] = 'world';
arr [3.55] = 'hi';  // '3.55'

//Create <empty slots> in array
fish = [1, ,3];  //length 3
fish = [ ,2,3, ]; // length 3 [last comma ignored]
```

Note : empty slots can behave like `undefined` & skipped in iterative methods (*map keeps them*). If empty slot is manually assigned undefined, it won’t be skipped.

```jsx
arr.at(-1) 

//search, optional fromIndex which supports negatives 
`pos` indexOf(val) , lastIndexOf(val) 
`bool` includes(val) //-ve fromIndex treated as a[0] to a[-i]

`DONT MODIFY ORIGINAL`
.join('*') //stringify
.slice(start, ^end_excl)
.concat(...args) //args appended
.flat(2) // lowers dimensionality by 2 

`MODIFIERS`
.pop()  .shift() 
.push(...val) .unshift(...val) //new_size
.splice(start, 4,...newEle) //return deleted
.sort()
.reverse()
```

**Custom Sorts `sort(callback)`**
- `cb(a,b)` -> elements being compared ; must return `Number`
- If return > 0, then **a is bigger** and sorted behind.
- *random sort* : return `0.5 - Math.random()`

**Iterative methods**
signature : `fn(cb,thisArg)` ; for each element & index, executes cb.

```jsx
arr.forEach(cb)
.map(cb) //array of callback’s return values
.flatMap(cb) // map then flat 1
.filter(cb) // array of items for which callback returned true
.reduce(cb, initval) // return of final callback
.reduceRight(cb, initval) //starts from last element

itm .find(cb) //first item for which callback returned true.
itm .findLast(cb)
pos .findIndex(cb) 
pos .findLastIndex(cb)
bool .every(cb) // callback returned true for every item
bool .some(cb) //  ...... for 1 item

//setup reducer
cb(acc,curr,currindex, org_arr) { 
	return x; //if no initval, 1st ele -> acc, 2nd ele -> curr
}
```

`.reduce()` reduces array to a single value by executing a callback for each element and passing the return of previous callback into next call

 [typed array guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Typed_arrays).

## Number

```js
Number.MAX_VALUE;
Number.MIN_VALUE;
Number.POSITIVE_INFINITY;
Number.NEGATIVE_INFINITY;

//Static methods
Number.parseInt(num, radix) //returns integers, based to radix. 
Number.toString(num, radix) //returns in string format
Number.parseFloat(num)
Number.isFinite(num)   //false if infinity or NaN
Number.isInteger(num)  // type integer 

//Round/Floatise a number
num.toFixed(2) // 2 decimals
num.toPrecision(3) //3 digits
num.toExponential(decimals_in_coeff) //coeff * 10^n where 1≤coeff<10
```

## String

```js
str.concat(...args) 
str.trim() , trimStart(), trimEnd() //remove whitespaces
str.padStart(4, “de”), padEnd() // dede<str>
str.repeat(N) //string copied N times

//char related
str[4]    //undefined
str.charAt(4) // ''
str.charCodeAt(4) //UTF-16, NO match - NaN

String.fromCharCode(...unicodes) //returns a concat string from unicodes

utf_str.normalize() // converts to normal string. e.g. "\u5043\u3422\u4722" becomes some readable text

//Search Methods — all are Case-Sensitive by default.

[bool] startsWith(substr, check_upto) or endsWith()
[bool] includes(substr, check_from)
[pos] indexOf(substr, check_from) or lastIndexOf() //reverse search
[pos] search(/regex/) //string arg -> regex
[arr or null] match(/regex/) //array of matches
[arr] split(/regex/) //array of non-matches (default : arr[0] =str)

// String Extraction 
replace(/regex/, inserted) //first match replaced (unless /g)
slice(0,8)  // 0-7 returns '' if  +end < +start
substring(0,8) //0-7 -ve indices treated as 0; if end < start, it swaps args
substr(0, 4) //4 length
```

## RegExp

```js
const regex1 = /ab\\+c/g; // treat + as '+'
const regex2 = new RegExp("a(b\\+)c", "g"); // flags are immutable

//2 methods [return after first match]
regex2.exec("dyab+c83") ///["ab+c" , "b+"] [fullMatch, ...groupMatches] 
regex2.test("dyab+c83") //true

//return range of matches
const regex3 = /foo/dg; //d flag 
regex3.exec("foo bar foo").indices  // [[0,3]]
regex3.exec("foo bar foo").indices  // [[8,11]] ; stateful

//group capture & replace
str.replace(/(-)(\w)/g, "$2$1")
str.replace(regex, (full_match, ...groups) => 
  groups[1].toUpperCase())  //on hit, curr replaced with returned val
```

Regex objects (with /g or /y) are stateful , ie., store `lastIndex` of matched string. **exec** and **test** advance past previous match.

```jsx
myreg = new RegExp("d(b+)d", "g")
myreg.exec("cdbbdbsbz") //lastIndex : 5 ; li is 1-based
myreg.exec("cdbbdbsbz") //lastIndex : 0

/d(b+)d/g.exec("cdbbdbsbz") // lastIndex : 5
/d(b+)d/g.exec("cdbbdbsbz") // lastIndex : 5 ; different literal 
```


## Output Based

When math/logical operations are done on different types, they are coerced to PRIMITIVE (first default, then number, then string)

```js
null + 2 == 2  ; '3' > 2 //true
[] == true // false ; [] --> '' --> 0 ; true --> 1 
NaN ** 0 === 1
-'34'+10 // -24
+'dude' //NaN

(1+2, 3+4) //7
```

### typeOf

```js
typeof Date; // "function"
typeof variable; // "undefined"
typeof document.all // "undefined"
typeof NaN // 'number'

var y = 1, x = y = typeof x // 'undefined' ; = is evaluated RtL
typeof [] // 'object' ; to check array use Array.isArray([])

//truthy objects
new Boolean(false) ; new String('')
```

```js
(2+undefined) == NaN //false NaN != NaN
isNaN(undefined) //true ; coerce to numbe
Number.isNaN(undefined) //false ; No coercion
```

```js
String(val) or '' + val //’undefined’, ‘null’, 'funct...'
String([1,[[[2]]]])  // '1,2'

'box' > 'adam' == true //b after a

Math.max([2,3,4,5]); // NaN
Math.max.apply(null, arr); //max of array
```

```js
3 instanceof Number //false
2 in [1,2] //false ; checks property (in array; props are '0' , '1')
```

### Hoisting

How to solve?
* check all declarations (func, var) and hoist names
* overshadow same names and assert

```js
console.log(a); //error
function foo() { console.log(a,c) } // 'a' undefined
let a = "a"; foo(); 
var c = "c"; 
```

```js
var foo = 'outside'; 
function logIt() { console.log(foo); var foo = 'inside'; } 
logIt(); //undefined <-- value is not hoisted ; shadowing
```

### Closures

```jsx
obj = {
  getThisGetter() {
    return () => this;
  },
};
fn = obj.getThisGetter(); // fn is arrow, fn() == obj
hof = obj.getThisGetter ; // hof is non-arrow, hof()() === window  
```

### OOPs

```js
var obj1 = { price: 20.99, get_price : function() { return this.price; } }; 

var obj2 = Object.create(obj1); 
delete obj2.price; 
console.log(obj2.get_price()); // 20.99 ; from the Prototype chain, gets obj1 price
```

```js
var obj1 = { value: "1"}, obj2 = { value: "2"}

!function(x, y) {
    x = y;
    y.value = "3"
}(obj1, obj2)

console.log(obj1);//{ value: "1" } ; x is variable ; not object 
console.log(obj2);// { value: "3"} ; pass by reference
```

### Timers

```js
for(var i = 0; i < 10; i++)
    setTimeout(function() { console.log(i) }, i); //immediately prints 10 times 10

//to fix
for(var i = 0; i < 10; i++)
    setTimeout(console.log.bind(console, i), i * 1000);
```

Reason : `setTimeout` is executed when current call stack is over (loop finishes). However, anonymous functions keep a reference to i by creating a closure. Value i has been set to 10 after loop's end.

```js
function printNumbers(from, to, delay = 1000) {
  for (i = from; i <= to; i++)
    setTimeout(console.log, delay, i);
}

printNumbers(1, 5); // 1sec delay --> immediately output 6 6 6 6 6 
//var i is func scoped so callbacks refer 6 at end of script
```

### Promises

```jsx
var a = Promise.resolve(1), b = Promise.resolve(a), c  = Promise.reject(a);

b.then(console.log)  //1
c.catch(console.log)  //Promise {1} ; no unwrapping
```

```jsx
// 1 step in resolution require 1 tick

var a = Promise.resolve('a') // resolved
var a2 = new Promise( rs => rs(a)); //not yet (on next tick)
var a3 = Promise.resolve(a) // resolved ; returns a

a2.then(() => alert('a2'))
a3.then(() => alert('a3')) 
// a3 a2
```

```jsx
let p = Promise.reject('why');
setTimeout(
	() => p.catch(() => alert('caught')), //new task
	1000);
window.addEventListener('unhandledrejection', event => alert(event.reason));

//why -> 1s -> caught
```