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
- they can iterate over *iterable objects* that implement `[Symbol.iterator]
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
```js

```
