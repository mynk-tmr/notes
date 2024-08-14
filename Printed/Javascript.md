#### JavaScript
- object-oriented scripting language
- runtime envr provides I/O ability to JS through objects
- ECMAScript : Standardized JS
- interpreted and JIT compiled
- single-threaded : Core JS has 1 call stack
- *dynamic* : runtime type binding ; unlike compile time viz. fixed

ES6 fundamental datatypes of which 6 are Primitive
1. **Number** : stored in 64 bit double precision.
2. **Bigint:** stored in arbitrary precision. For Integers larger than 2^53 -1 (15 digits)
3. **Boolean**: Represent a _logical entity_ and can be true /false.
4. **Null**: Shows something that does not exist. Has only one value: _null._
5. **Undefined**: initial value of unassigned variables, non-existant properties
6. **String**: a set of UTF-16 characters (16-bit unsigned integer (0-65536)). They are *immutable* array-like objects.
7. **Symbol:** a primitive type for unique *identifiers*. Unlike others, it does not have any literal form.
8. **Object**: represent a single entity having **state** (data) and **behaviour** (methods). It is structured as a list of _property-value_ pairs.

---
##### Numeric literals

```jsx
//Each base letter can be capitalised
0o33 // OCTAL , not allowed in strict mode
0xff; //HEX
0b11 //BINARY
0.255e3; or 25500e-2  // EXPONENT FORM

// pre-defined
-Infinity +Infinity //uncomputable numbers like 0 division

NaN //a special number that represents failed conversion, imaginary or indeterminate no.s (Infinity operations)

```

---
##### BigInt Literals `let a = 33n;`
- Uses **64 bits**, where 63rd is signed, 62-52 store coefficient and rest store power (fraction)
- cannot represent decimals or Octals. BigInts can’t use >>> , since they don’t have fixed width, or highest bit.
- can be **compared** to numbers and **parsed to int, float**.

`a = 1n + 2; // TypeError: **Cannot mix BigInt and Number**`
`isNaN(3n) // TypeError, forced 3n to number`
`5n / 2n; // 2n`

---
**Template literals** — special strings that allow embedded expressions and preserve indentations. ${} return expr result as string

**Tagged templates** :- allow you to **parse** template literals with a function. Output is tag function’s return.

```jsx
let size='';
console.log(test`burger is ${size} big`);

function test(substr_array, placeholder1, ph2, ...) {
	return string[0]+ 'very' + string[1] //burger is very big
}
```

---
### Symbols

```jsx
let id = Symbol("id")  // create symbol with optional name/description
let id2 = Symbol("id") // different symbol

//use Symbols to safely add properties
Obj[id] = 'fbi' //no-overwritings
Obj[id2] = 'cia'

// If you want same-named symbols to be equal, use the global registry.
let sym1 = Symbol.for('monkey') //creates global symbol
let sym2 = Symbol.for('monkey') //returns it, No new symbol
Symbol.keyFor(sym1)  //monkey ; for non-global -- undefined

//Get Symbols
Object.getOwnPropertySymbols(obj) //get all symbols
Reflect.ownKeys(obj) //return all keys including symbolic ones.
```

Symbols don’t auto-convert to string. Explicitly use `.toString()` or `symbol.description`

Symbols allow us to create **“hidden” properties** of an object, that no other part of code can accidentally access or overwrite. They are skipped by almost every method except `Object.assign` and `structuredClone`

**“system” symbols** — that JavaScript uses internally,  accessible as `Symbol.*`. Can be used to modify built-in behavior and fine-tune objects. 

[Well-known symbols](https://tc39.github.io/ecma262/#sec-well-known-symbols)
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

## Keyed Collections

Keyed collections 
- data structures *ordered by key* not index. e.g. Map and set objects 
- associative in nature and use *SameValueZero* check (NaN can be key).
- **iterable in order of insertion**
- Map is a collection of **unique** **keyed** items, where key can be *any* type (even objects). Plain Object allows only `String` or `Symbol` keys
- Set is a iterable collection of *unique* *values* of any type 

Methods and properties in both
- `new Map/Set(iterable)`
- `.size` : length
- `map.set(key, value) / set.add(val)` : Returns new map/set ; overrides previous
- `.get(key)` – returns val or `undefined`
- `.has(key)`
- `.delete(key)` - return t/f
- `.clear()`
- `map.keys(), .values(), .entries()` 
	- not same as static `Object.keys` etc. -> return real array ; not arraylike


```jsx
new Map([[1, 'hello'], [2, 'bye']]); //for key-values
new Map(Object.entries(obj)); //from obj's key-values
Object.fromEntries(map) //create object
```

#### Weakmap & Weakset

```jsx
let john = { name: "John" };
let arr = [ john ]; 
john = null; // original isn't garbage-collected [arr[0]]

// to prevent this, use weakmap, weakset
```

[`WeakMap`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakMap) and [`WeakSet`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakSet) only allow *objects* *keys* and have weak reference to them, so they can easily be removed by garbage collector. They only have `get` `set` `delete` `has` methods and no `size` and aren't iterables (you can't get all content)

`WeakMap` and `WeakSet` are used as “secondary” data structures for “primary” object storage. Use cases :
- extra data about objects, which auto-deletes when object dies
- memoizing /caching 
- data about sealed/alien objects

```jsx
// Symbol can be used too for this, only our code can see this
let isRead = Symbol("isRead");
msg_obj[0][isRead] = true;
```

----
### Control Flow 

**Expression evaluation**
- decide order of operations -> by *precedence*
- when same precedence -> *associativity* . Almost all are left-to-right

**Conditional stmts**
- `case: ` tests for *strict* equality ; case can be enum values / string object

**Loops**
- `for` evaluates step at end of current iteration
- `for in` iterates over *enumerable* properties 
- `for of` iterates over *values* of **iterable** objects. Both pass by **value** to key/val
- `for await(.. of ..)`
	- iterate over async or sync objects
	- can trigger custom iteration workflows for each property
- `onix : for(key in obj)`
	- onix is *label*, an identifier for *codeblock* that you can refer to elsewhere
- `break` terminates current (or *labelled*) loop
- `continue` *jumps* control-flow to next iteration. In for, it updates STEP. Given *label* must be LOOP
- you can only jump to *ancestor* label

**Ternary/conditional operator**
- acts on 3 operands and works like an *inline-if-else*.
- returns value of expr chosen to be evaluated.
- can't work with `break` `continue` `return` (can't control flow)

---
### Error Handling

An *exception* is a undesirable event during runtime, that brings the execution to a halt. To handle them, we specify steps to follow for each such event.

**Stack trace** : list of function calls that lead to an error -> latest to earliest

`try {..}`
- creates a codeblock where any error transfers control to *first catch block* in call stack. 
- for user-defined exception `throw val` 
- Error object is visible only in `catch` block. 

`finally {..}` : always execute *immediately* after `try` and `catch` blocks. *Overrides* rethrows/returns in `try-catch` with its own return (if given)

All errors are *serializable* object with props `name` , `message` , `stack`
**Types**
- `ReferenceError` — accessing undeclared or uninitialized variables
- `SyntaxError` — incorrect syntax
- `TypeError` — invalid operands/arguments; invalid use or change to a value
- `StackOverflowError` — when number of execution contexts exceed size of host’s stack 

---
#### Precedence table

```js
fn(), obj.x, new Box()
++x
//rest of unary (same pre)
//arithmetic (Power > DM > AS)
//bitwise shifts
in instanceof <= etc.//compare 
== != etc. //equality checks
/* logical ops (7-3) => & ^ | && || ?? */

=>  x:'s'  +=  ...spread, yeild, return //assignments

(1+2, 3+4)  //comma 7 ; eval all return last
```

---
#### Equality checks
- *samevalue*: strict but `NaN==Nan` and `-0` =/= `+0` . Used by `Object.is`
- *samevalueZero*: `+0 == -0` e.g. Map methods
- *Object equality* : same memory space ; loosely equal to primitive counterparts 
	- `empty_obj != {} ... not same memory`
	- `Object.keys(obj).length == 0  //correct way`

---
#### Type Coercion

```jsx
null + 2 == 2  //0 coerced
undefined / 3 //NaN
'hello'.toUppercase() //auto object coercion
'3' > 2 //true ; on comparing diff types, they're number coerced

(2+undefined) == NaN //false NaN != NaN , use isNaN()

String(any) or '' + any //’undefined’, ‘null’, 'funct...', 'NaN'
String([1,[[[2]]]])  // '1,2'

isNaN(undefined) //true ; coerce to number
Number.isNaN(undefined) //false ; No coercion
NaN ** 0 === 1 //weird

'box' > 'adam' == true //b after a
```

---
#### Short-Circuit Operators

Operators which always evaluate *left* operand first, and then right only if needed. It only affects **evaluation order** of operands, not final result. These are — **&&, ||, ??, ?**  and their assigment counterparts

If multiple short-circuits, they are **grouped** but circuits evaluates left to right only.

```jsx
let A, B, C return false, false, true
C() || B() && A() // only C called
A() && C() || B() // A & B called
```

Notes
- *falsy*: `[]`, `0`, '', `NaN` , `nullish` 
- *truthy*: other values including `"0"` `" "`
- `&&` returns first *falsy* operand 
- `||` returns first *truthy* operand
- `??` returns first *non-nullish* operand
- `?.` short circuits expression to `undefined` if left operand is *nullish*

---
##### Bitwise Operators

operate on individual bits of operands. Treat them as signed int, but result is treated as signed `number`. 
-ve integer : 2's complement, invert bits; add 1.

**and (&) or(|)  xor(^)** : return 1 for each bit positions where both 1 / atleast one is 1 /  both same.

**x << 2** left shift bits, right is padded by 0
**>>** right shift bits ; pad left by *sign* bit
**>>>** unsigned right shift ; pad left 0

---
##### Unary Operators

```jsx
delete obj[prop] //true except if prop is own non-configurable
typeof op // string
void op //evaluate op and return undefined
```

```jsx
typeof null; //  "object"
typeof Date; // "function" .. class
typeof variable; // "undefined"
typeof document.all // "undefined"
```

**postfix/prefix**
- evaluates to a value, not a reference (so no chaining)
- In while & do while loops, *postfix* runs *1 extra* than prefix.

---
##### Spread & Rest

Spread syntax "expands" an *iterable* into its elements OR an object into its *own_enum_key : value* pairs . It can be used anywhere. Falsy & non-iterables spread nothing. 

Rest syntax collects multiple elements and "condenses" them into a single array. Allowed on only last parameter.

Spread syntax cannot mutate object or trigger setters or copy prototype, unlike `Object.assign()`

---
#### Destructuring Assignment

Unpacks values from *objects & iterables* into distinct variables. Default values apply only for strictly `undefined` and are lazily evaluated.

```js
let [a, ...r] = 'hey'; // a= 'h' ; r = ['e', 'y']
//swap
[arr[1], arr[0]] = [arr[0], arr[1]]

func({id=211} = {}) {..} //to allow call with func(), else error
```

objects : imitate pattern
```jsx
let {
	id,
	foo : x, 
	bar = '0', //default 
	[f()] : var3, //f() returns some prop_name
	fullName: { 
		lastName : [a,b,c]
	} 
}  = myOBJECT;
```

---
##### Strict mode `‘use strict’;` 

Semantic changes
1. silent errors may *throw* e.g. invalid assignments, invalid deletion
2. run *faster* than identical sloppy code
3. Prohibits some syntax for *future-proofing*
4. Easier to write *‘secure’* Javascript e.g. can't access `callee` , `caller` and `arguments` in strict fn

Notable changes
- no `globalThis` substitution (now `undefined` ). Can’t skip `var`
- **block-scoped function declarations** (emulate private methods)
- strict functions don't allow rest, default, or destructuring
- use `delete window.x`
- No duplicate parameters or object properties
- Non-leaking eval : `eval("var x;")` ⇒ variables are scoped to eval only
- Non-updating immutable `arguments` : stores original parameters

---
### Functions

- *first class* : treated like variables. Functions are **executable objects**
- *Callback* : a function passed into another as an argument and then invoked inside it
- _pure_ *function* : always produces same value for same args. No side-effects and not read global values. Helps us avoid action-at-distance anti-pattern.
- _higher-order functions :_ consume and/or return functions. They abstract actions, and are used in data processing and function composing

#### Function Creation

```jsx
function add(a,b) { a+b } //Function Declaration [separate stmt]
hero = function () {..}   //Function Expression [created as part of expr]
bar = function foo() {..} //NFE : named func expr
mk = new Function('...args' , '<body>');  //the New Syntax
```

In NFE, name is visible only within function's body. It can be used in recursive calls. We can safely assign bar to other variables without breaking code.

Function expressions aren't hoisted.

**new syntax** parses string args into a function object. It is always created in **global** **scope** and can only refer to global values. `eval(str)` can refer to local variables and change them. Avoid using both [highly risky].

#### Function Keywords

Pre-defined properties
- `arguments`
- `length` : no. of args, _excluding_ …rest params.
- `new.target` : **true** if function was called with new, otherwise false.
- `name` : function name, same as provided in declaration / assignment

Parameters
- _default_ : for `undefined` ; lazy
- …_rest_ parameter collects **indefinite** no. of args into 1 **array**
    - `arguments` is array-like, while rest parameter is real
    - `arguments` maintains all parameters, rest only uncollected ones
    - In some cases, `arguments` syncs with parameter changes. Rest never updates on changes.

User-defined Properties `myfun.prop = expr/func;`
- To lessen pollution of global space or to avoid name conflicts.
- To track calls and meta-data related to function

#### Closure
- combination of a function and its lexical environment specification object
- LEX is referred by function's `[[Environment]]` property, and through LEX, a function can access all variables in chain of  scope
- When function(s) are created, they capture variables in their outer scope during creation
	- functions created in **same run** share the closure, but in different runs do not. e.g. `useToggle` returns
- Like plain objects, LEX isn't garbage collected till its referenced

```js
//common mistake... using non-block variable in loops (var ; let from outer)
//functions created in loop will refer to end value of variable
```

**Stale closures :** capture variables that have outdated values. Created when variables aren't updated by functions
```jsx
function add() {
	let a = 0;
	let msg = `a is ${a}`
	const inc = () => ++a; //inc called thrice
	const log = () => msg; //knows a is 3, but `a is 0` due to stale msg
	return [inc, log];
}

//to fix, make the stale dynamically evaluated
const log = () => `a is ${a}`;
```

#### Arrow Functions

Special anon F.Ex. that inherit `this` from outer scope ie., they are **statically** bound to the **enclosing** context where it is **defined.**

Pros : 
- predictable behaviour w.r.t closures / this. 
- bound to instance as instance fields and to class as static fields.
In non-arrows , 
- `this` is runtime bound; either invoker object OR globalThis ; 
- they lose `this` when passed as callbacks

```jsx
obj.show = function() {  //same as show() {..} in obj
	this.arr.forEach(arrowcb) //this -> obj and arrowcb will not lose it
} 

obj = {
  getThisGetter() {
    return () => this;
  },
};

fn = obj.getThisGetter(); // fn is arrow, fn() == obj
hof = obj.getThisGetter ; // hof is non-arrow, hof()() === window  
```

Limits :
- don’t have `arguments` , `new.target` or `super`
- context can’t be changed (`call,bind,apply, thisArg`) and is silently ignored. Hence, **can’t be shared** via prototypal inheritance
- can’t be used as constructors
- Never return arrow functions if it will be invoked by objects

#### Call, Apply and Bind

`call(thisArg, ...args)`
`apply(thisArg, arrayish)` (faster, more optimised)
`bind(thisArg, ...args)` : returns a **bound** function with same body as invoker, but this bound to obj and args bound to parameters. This binding is immutable and call, apply, bind won’t work later.

Note : Function properties aren't passed on.

```jsx
// Call forwarding : a technique that allows a function to pass on its context & args to another.
let f = function (x,y,z) { g.apply(this, arguments) }

// Method borrowing : calling a method from an object in context of another object.
Array.prototype.forEach.call('Hello World', cb); 

// Primitive to object e.g. Number
show.call(1); 

//fixing `this` of non-arrows
function outer() { inner.call(this); } 
setTimeOut(ob.func.bind(ob)); 
```

**Partial application:** transforming functions from higher to lower arity. Such functions are *partial functions.* Just *fix some args* of function and return it.
They avoid having to repeat some arg(s) again & again in calls.

```jsx
double = multiply.bind(null, 2);

// if context (this) can change, hof
function partializer(original_func, ...bindUs) {
  return function(...args) {  
    return original_func.call(this, ...bindUs, ...args); 
  }
}
```


---
#### this keyword

object literals : `this` inhertied from enclosing context
Event handlers
- `this=currentTarget`
- in inline, `this=element` but in function refers to `window`

---
#### Property Descriptors

Data property :- *no getter, no setter*
Accessor property :- *no value*, no writable ; use when value is dynamic like date-dependent
Getter & setter are automatically called when working with **accessor** property

```js
flags  = {
  value : 'Mayank', // for property 
  writable : true, // value editable ; if config-false, can be set to false, but not to true
  enumberable : true, //seen by loops
  configurable : true, // deletable, attr modifiable
  get() {}, // or get : getter 
  set() {}, 
}
```

## Prototypal Inheritance

##### What ?
- Object-to-Object inheritance model using prototypes (a special internal property of each object).
- Objects link their `.prototype` to share properties and form a *chain*. Each object inherits all properties present **up** the prototype chain
- At runtime, property lookup starts at object and goes up the chain, until a match is found.
- Object's *own* props **shadow** properties available in chain. 
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

Classes are based on prototype-based *constructor functions*, with enhancements
 - `constructor()` is explictly defined & requires **new** to be called
 - class methods are **non-enumerable**
 - have **strict-mode** and **TDZ** like let
 - **super** keyword is used to *access* *public* props in superclass, or invoke its constructor
 - have *private* fields that aren't inherited and only accessible in Class definition
 - **extends** as syntactic sugar of `Object.setPrototypeOf`

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

#### Function Currying

A technique that transforms a function of multiple arguments into several functions of a single argument in sequence `f(a,b,c) === f(a)(b)(c)`

Currying is possible because of **closures —** a returned function has access to previous argument(s) in chain.

Uses
- generate single purpose functions that are easy to maintain and debug
- flexible invocations ⇒ sum(1,2)(3) or sum(1)(2)(3) etc…
- **function composition :** a technique to use a HOF to chain functions in particular order. It improves readability and reusability of code, since you can chain same set of functions in various ways to create distinct HOFs

## Generator Functions

functions that can **pause** and don’t follow run to completion. Only `*f` can pause itself with `yeild` and only its **own generator object** can resume it with `next()`. It is a control + communication mechanism. `*f` once returns, is marked complete, and willn't run ever

```jsx
function* f() {
	for(let i=0; i<10; i++)
	let response = yeild i; //send {value:i, done: false} 
	return 3; // send {done: true} , no value
}

const keygen = f(); //create iterator object
keygen.next() //start *f ; 1
keygen.next('ok') //response = 'ok' ; 2
keygen.return('quit') // {done: true, value: 'quit'}
keygen.throw('error') //throw error in *f

[...keygen] //0,1,2....
```

##### Iterators
- objects that abide by iteration protocol ie., 
	- have `next()` , optionally `throw()`,  `return()`
	- Each takes atmost 1 arg & return `{value, done}`  (IteratorResult interface)
- they can iterate over *iterable objects* 
	- implement `[Symbol.iterator]` and return iterator
	- a iterator can become iterable (return `this` in `Symbol..`) e.g. generator object
- Arraylike have index and length but aren't always iterable

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

```js
Array.from(obj, ^mapFn, ^thisArg) // convert iterables/arraylikes
```

|                   | Iterable                                         | Async iterable                           |
| ----------------- | ------------------------------------------------ | ---------------------------------------- |
| Object implements | `Symbol.iterator`                                | `Symbol.asyncIterator`                   |
| `next()` returns  | {value, done}                                    | `Promise` that resolves to {value, done} |
| to loop, use      | `for..of` which runs next()<br>until `done:true` | `for await..of`                          |
| features          | can be `...spread`                               | NO                                       |
```jsx
async function* f(urls) {
  for (let i = 0; i <= 5; i++) {
    let i = await fetch(urls[i]);
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
- **Non-linearity** : flow is hard to grasp in non-linear way
- **Pyramid of Doom** : deeply nested timers to perform a series of async operations. 
- **Race conditions** : when some async callback may finish synchronously

All these result in **Callback Hell** where code is unmaintable, unpredictable, full of latent bugs and prone to edge-cases. To solve, we have
- **Promises**, that use *split-callback* design
- **Generators** let you 'pause' individual functions.
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
`async` function return a *new Promise* that will resolve to function's *actual return* value 

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

More info : https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules#top_level_await
#### Dependency evaluation
```js
// -- a.js (entry) --
import { b } from "./b.js"; //evaluate dependency b first
export const a = 2;

// -- b.js --
import { a } from "./a.js"; //a not declared yet, so ReferenceError if accessed
export const b = 1;

//CYCLICAL DEPENDECIES are error-prone
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
- modules  `binding.js` responsible for communicating with runtime should perform checks and insert polyfills / change methods accordingly

## JSON

JSON is a **text**-based data format following JavaScript object syntax. JSON exists as a **string**. 

2 methods [json -> array/object]
- `JSON.stringify` (_serialization_)
- `JSON.parse`  (_deserialization_)

Notes
- JSON can be a single primitive
- JSON keys requires double-quotes.
- values that are `function, symbol, undefined` are skipped

```jsx
JSON.parse('{"name":"John"}'); 
arr = JSON.parse('["name":"John"]'); //arr[0] 
JSON.parse(data, (key,val) => {
	//code
	return newValue; // transform values including nested
}) 

//date fields are auto evaluated
```

```jsx
JSON.stringify(obj, ['id', 'name']) //only these, nested keys in them have to be explicity included

//skip values
JSON.stringify(obj, (key, value) => {  //nested key-value also iterated over
	return key == 'under' ? undefined : value
})

//object's toJSON auto-invoked by stringify
let room = {
  number: 23,
  toJSON() { return this.number }
};

//remove circular (self) references
JSON.stringify(meetup, (key, value) => {
  return (key !== "" && value == meetup) ? undefined : value;
})
```

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
- If object is *weakly referenced*, it can be garbage collection if it is unreachable via hard link

**“mark-and-sweep”** : basic garbage collection algorithm
1. “mark” (remember) roots
2. mark all references from them
3. visit marked objects and mark their references. 
4. repeat 3rd step until no objects can be marked.
5. remove all 'unmarked' objects

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

---
### Event Loop, Macrotask, Microtasks

JavaScript execution flow is based on an *event loop*. General algorithm of event loop :- 
1. If call stack is empty and **task queue** has a *macrotask*, deque and execute it
2. execute all *microtasks* placed in **microtask queue** by macrotask (same fashion as above)
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

##### Scheduling Execution
- Browser starts enforcing **atleast 4ms** delay if same callback has been scheduled **5 or more** times.
- Timer may slow down
    - on tabs that are inactive or streaming or loading page
    - OS power saving settings
- timer continues “ticking” while showing alert/confirm/prompt.
- **Zero delay scheduling** is used to execute code immediately after current task

##### Web Workers
Run code in different parallel thread. Can exchange messages with the main process, but they have their own variables, and their own event loop.  JS never shares data across threads

Web Workers do not have access to DOM, so they are useful, mainly, for calculations, to use multiple CPU cores simultaneously.

## DOM

DOM represents content on HTML document as a **hierarchical tree** of node objects.
- **DOM-API** exposes the functionality to change webpage’s content
- It is cross-platform and language-independent.
- It is *live data-structure* and updates when changed by JS

DOM basic objects
- `window` : global object
- `document` : property of window. Serves as **root** node.
    - its properties are `documentElement` (html) , `head` `body`

#### DOM Datatypes
```js
`NODELIST (iterable of nodes)`
children.length, children[4], children.forEach //live

`HTMLCollection - a live iterable of HTMLElements`
//returned by -Element- methods

`NamedNodeMap - a live iterable of `Attr` objects mapping element's attributes`
div.attributes.id.value //or specified 

`DOMTokenList - space separated live classes on element`
div.classList //get
```

##### Node Codes / Constants
`1 (Node.ELEMENT_NODE)`
`3 (Node.TEXT_NODE)`
`8 (Node.COMMENT_NODE)`
`9 (Node.DOCUMENT_NODE)`
`11 (Node.DOCUMENT_FRAGMENT_NODE)`

`DocumentFragment` interface is a lightweight version of `Document`. It is used to compose nodes and inserted as a whole to improve performance
```jsx
let fragment = new DocumentFragment();
fragment.append(eleList) //append to fragment
div.appendChild(fragment) //append fragment to DOM
```

##### Node v/s Element
Node is a generic name of any **object** in DOM tree.
An element is a node with a specific node type `Node.ELEMENT_NODE`. It has several subtypes like `HTMLInputElement`

##### Node Relationships
Each node has **6 relationships** (default : null)
`parentNode` `firstChild` `lastChild` `previousSibling` `nextSibling` `childNodes`
(with `*Element*` versions )
- Selecting nodes — use the most specific method, match is **case-sensitive.**
- Altering attributes — always use **get** and **set** methods
- Inserting *multiple nodes* return **undefined** or throw error if insertion fails.  But for 1 node a *reference* to node is returned

##### Altering attributes
- standard html attributes -> element properties. Some may type convert
- Boolean attributes can’t be set to false, they have to be `removeAttribute()`

```jsx
getAttribute('id'); //better than div.id
setAttribute('id', 333); //name is lowercased, string coerced
removeAttribute('id'); //No error on absent 
hasAttribute('id')

//styling 
$ div.style //inline styles object
$ div.currentStyle // computed styles object
$ getComputedStyle(div,':hover') 


div.className //returns a string of space-separated classes
div.classList //returns a domtokenlist
div.classList[2] //get 3rd class

//classlist
values() //return iterable list of classes
add('hero', 'mob');                                      
remove('hero', 'mob'); 
toggle('active');
replace('dark', 'light'); // replace dark with light
contains('hero') // false
```

## Event-Handling

Events are fired to notify **"interesting changes"** that may affect code execution. They may be user-generated (like clicks) or system generated (like low battery). 

Each event is represented by an **object** that implements **Event interface.** They can be accessed only via listeners

An **event target** is any object that can subscribe to an event. It can trigger callbacks (called eventlisteners) upon event.

Events happen in **cascade** : a compound event happens after elemental events
Event flow has 3 phases 
- Capturing phase ⬇️ — event flows down from **root** element to event **target** via branch
- Target phase 🎯 — event reaches its target element
- Bubbling phase ⬆️ — event flows up from target to root through same DOM branch

##### Event Delegation
- a technique which uses **bubbling** to handle event at a Node higher than actual node target
- Greatly reduces no. of event handlers needed, thus improving memory utilisation and performance
- wherever possible, attach listeners on `document`

##### Handler Optimisations
used for rate-limiting the execution of callback to improve performance
- Given a rapid series of events,
    - **Debouncing** ensures that callback is executed **only once** by calling it only when time difference between 2 events > max_delay
    - **Throttling** ensures that callback is executed at **regular intervals** by calling it only when time elapsed since previous callback > max_delay
- debounce — *search*
- throttling — *infinite scrolling*

3 ways to register **event-listeners :**
- HTML event attributes — `onclick='handler()'`
- property — `btn.onclick = handler`
- `.add`
    - can add any number of handlers for a **single** event.
    - works on any event target, even XMLHttpRequest

```jsx
addEventListener(type, listener, flags)
//named listeners aren't re-registered. Anonymous do

// default flags
{ capture : false, 
	passive : false, 
	once : false,    
	signal : null,  
}
```

Adding more listeners **inside a listener** doesn’t trigger them during **current phase** of flow

**Listener** can add **effects** to event 
- `preventDefault()` — prevent the browser’s default handling of event ; works only if property `cancelable=true` ; sets `defaultPrevented` to true.
- `stopPropagation()` — stops the event-flow once it has processed all its listeners on **currentTarget** ; works only if `bubbles=true` (default)
- `stopImmediatePropagation()` stops the event-flow. No other listeners will be called

**Passive Listeners**
cannot cancel the default handling, so the browser can start it immediately, without waiting for the listener to finish. Will work only when **all listeners** on an event path are passive.

**synthetic or custom events**
```jsx
const event = new CustomEvent("build", { detail: 'hi', bubbles: true});

//send event to element invoking listeners in appropriate order.
btn.onclick = input.dispatchEvent(event);
```

Unlike **"native" async** events, it invokes event handlers _synchronously_ and returns when all of them have executed. `false` if atleast 1 handler used `preventDefault()`, else true.

## Reference

```js
window.top //topmost window
window.parent

document.activeElement //focused
document.referrer //uri of referrer page
document.readyState //loading, loaded, completed
document.styleSheets //iterable stylesheet collection
document.title 
document.forms //links, images, etc
document.importNode(node, deep=true) //clone node+children from another doc
document.adoptNode(node) //steal node+subtree
document.createElement('p') //TextNode, Comment
document.getElementsByName('div') //LIVE nodelist
document.getSelection().toString() //selected text
```

### HTMLElement interface (live)

```js
tagName //uppercase
innerHTML textContent //markup inside vs. all textnodes in ele+children
innerText //respect css hidden

//FIND elements
$q //first matched child
$qall //static nodelist
div.closest('.book') //itself or ancestor closest
div.matches('.book') //true if this ele matched

//ASSERT
div.contains(ele) //descendant
div.hasChildNodes()
div.hasfocus() 
div.isEqualNode(ele) //equal,
div.isSameNode(ele)

//will relocate if node is in DOM
div.before(...nodes) // insert these
div.prepend(...nodes) //before first child
div.append(...nodes) //after last child
div.after(...nodes)
div.insertBefore(new, nextSibling) //append for none
div.replaceChildren(...nodes) 
div.replaceChild(new, old) 
div.removeChild(child)
div.remove() //remove itself

//helpers
div.blur() div.focus() div.click()
div.cloneNode(deep=true) //clone with listeners (+children)
div.normalize() //fix jumbled text nodes
```

```js
div.insertAdjacentElement(pos, ele) 
div.insertAdjacentHTML(pos, markup) 
'beforebegin' 'afterbegin' // before element / first child
'beforeend' 'afterend' // after last child / element
```

---
##### Event Object properties
$ `target` — element that triggered event
$ `type` of event that was fired e.g. `‘click'`
$`currentTarget` — element on which event is currently flowing
$`detail` more info about the event
$`eventPhase` — 1 for capturing phase, 2 for target, 3 for bubbling
$`isTrusted`  — user-generated `true` and for synthetic `false`
$`timeStamp` — time(ms) when event was created

---
#### Drag events

`event.dataTransfer` stores data being dragged.
```jsx
dT.setData('text/plain', str) //text/uri-list 
dT.getData('text/plain'); //str
dT.setDragImage('hi.png', 50, 50) //cursor at (50,50) inside drag image
dT.dropEffect = 'link'; //cursor [move, copy, none] ; if none event got cancelled
[...dT.types].includes("text/html") //false
```

---
#### Pointer Events

Better since, they are _hardware-agnostic_ and supports mouse, stylus, touch, etc.
Event flow : `down` `enter` `over` `move` `out` `leave` `up`  ; can `cancel` anywhere
Others : `got/lost pointercapture` 
Mouse-only : `click` `dblclick` ; `contextmenu` right-click.
Notes
- preventDefault  `down`
- for `move` to work, prevent `ondragstart` and add `touch-action: none` on element
- `enter/leave` do not bubble
- `move` fires continously 
##### Event properties
- `pointerId` given to **each** finger, stylus, etc during a flow. Starts from 1.
- `pointerType` (mouse, pen, touch, etc.).
- `isPrimary` true if primary for given **type**
- `button` — 0 to 5 {L,M,R, X1, X2, eraser}
- `relatedTarget` — element where pointer was previously
- Modifier keys (**true** if pressed) : `altKey`, `ctrlKey`, `shiftKey` , `metaKey` (ctrl of Mac)
- `clientX` `clientY` — (x,y) coords of pointer at event [viewport relative]
- `screenX` `screenY` — for combined screens (many monitors)
- `pageX` `pageY` — document relative, takes scrolled amount into account
- `offsetX` `offsetY` — target relative

---
#### Keyboard Events

Shouldn’t be used for inputs. **`Fn key`**— no keyboard event
Types
- `keydown` – auto-repeats if the key is pressed for long
- `keyup` – release [once only]
Properties
- `code` (location) : `"KeyA"`, `"Digit0"` , `"Enter"`
- `key` (interpretation) : `"A"`, `"a"` or same as `code` for non-character keys
- Modifier keys `altKey`, `ctrlKey`, `shiftKey` , `metaKey`
- `location` : which **version** of key pressed. [0,1,2,3] → [single key, left one, right one, numpad]
- `repeat` : **true** if long key press
- `isComposing` : **true** if event is fired within a composition session

---
#### Input-Related Events

Event targets :- `<input> <textarea> <select>` and any element with `contenteditable` or `designmode` ON.

`focusin` `focusout` element or any of its descandants gain or lose focus.
`focus` `blur` Do not bubble. All focus events are **non-cancelable**
`beforeinput` fired before value is changed and is **cancelable.** Only for text-related.
`input` fired after every change & **isn’t cancelable.** Applies to all controls.
`change` fired after blur/`Enter` /select & if value has changed.
`cut, copy, paste` should always be **cancelled**.

Event Properties
- _input_ and _beforeinput_
    - `data` : single inserted char ; null for anything else.
    - `dataTransfer` : object representing inserted data. Has **get, set & clear** methods.
    - `inputType` : insertText, deleteContentBackward, insertFromPaste, formatBold
    - `isComposing`
- _clipboard-related_
    - `event.clipboardData` gives access to clipboard.
        - `getData('text/plain')` `setData('text/plain', str)`
        - `dataTransfer`

----
#### Load events

**document.body**
- `DOMContentLoaded` – DOM built but resources are yet to be loaded. `load` happens when resources loaded
- `resize` —  window is resized.
- `beforeunload` — before page close ; shows confirm dialog. In handler, prevent default and set event.returnValue = '"';
Any **element** with resources e.g. img `load` — success `error` 

---
**Full screen and PIP mode**

```jsx
document.fullscreenEnabled //true
document.fullscreenElement; // get element currently in fs mode
div.requestFullscreen(); //put div is fs-mode
div.exitFullscreen();

// Similarly for PIP
pictureInPicture

//events are sent to element and document that owns element
'fullscreenchange'  /* ele enter/left FS */
'fullscreenerror'  /*browser cannot switch to FS */
```

**Pointer Lock**
```jsx
div.requestPointerLock(); // lock pointer, set div as target for pointer events
document.exitPointerLock(); //on doc
document.pointerlockElement //read only getter
'pointerlockchange' // event associated
```

---
#### Dimensions

```js
window.
outerHeight. outerWidth //window
.innerHeight .innerWidth //viewport 

div. //read only
offsetHeight .offsetWidth // border box + scrollbar
.clientHeight  .clientWidth // viewable padding box
.scrollHeight  .scrollWidth // full padding box
.offsetTop .offsetLeft  // (x,y) w.r.t parent

div.getBoundingClientRect()  // position w.r.t viewport
// contains top, bottom, left, right
```

---
#### Scroll manipulation

```jsx
window
.scrollX .scrollY  //amount scrolled
.scrollTo(0, 10) .scrollBy(0, 10)  // left,top [No -ve allowed]

//1 page scrolling
window.scrollTo({top: 100, left: 100, behavior: "smooth"})

//get and set scroll value
div.scrollTop, div.scrollLeft // min 0 , max = scrollHeight - clientHeight

//scroll div into container’s visible area
div.scrollIntoView ({
	behavior: 'smooth',
    block: 'nearest', //align to y-edge (start, center, end)
    inline: 'center' //align to x-edge ("")
})
```

```js
//zoom using 'wheel' event
scale += event.deltaY * -0.01; //wheel down zooms
scale = Math.min(Math.max(0.125, scale), 4); //limit zoom
div.style.transform = `scale(${scale})`;

//scroll events 
'scroll', 'scrollend'
```

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