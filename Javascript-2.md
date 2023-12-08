# OOP in Javascript

## Objects

`[expr] : val` => computed or dynamic props
`'1' : val` => integer prop, sorted automatically (+1) isn't
`[Symbol.toPrimitive](hint) {}` => custom convert ; hint is decided by V8 => string, number, default (str+num)

iterable protocol allows objects to define or customize their iteration behavior.

## Property and Object flags

Data property :- no getter, setter
Accessor property :- no value, no writable ; use when value is dynamic like date-dependent
Getter & setter are automatically called when working with accessor property

```js
flags  = {
  value : 'Mayank', // for property 
  writable : true, // value editable ; if config-false, can be set to false, but not to true
  enumberable : true, //seen by loops
  configurable : true, // deletable, attr modifiable
  get() {}, // or get : getter 
  set() {}, 
}

Object.defineProperty(user, 'name', flags); //create/modifies prop ; return obj
Object.defineProperties(obj, {prop1 : flags1 , prop2 : flags2}); //add / modifiy multiple ; return obj
Object.getOwnPropertyDescriptor(user, 'name') // return  object having flags
Object.getOwnPropertyDescriptors(user) // plural, return {p : flag, q: flag ..} 

//flag aware cloning -- include all symbols, properties, flags
let clone = Object.defineProperties({}, Object.getOwnPropertyDescriptors(obj));
```

```js
Object.preventExtensions(obj) //no new props
Object.seal(obj) // no (+) (-) props , sets config-false for all
Object.freeze(obj) // seal and write-false all

//test
Object.isExtensible(obj)
Object.isSealed(obj)
Object.isFrozen(obj)
```

## Prototypal inheritance

A model where an object or class is linked as prototype of another object (or class) enabling sharing of properties and methods to child. 


```js
animal.isPrototypeOf(dog) //true ; check if chain contains animal
dog instanceOf animal //true
dog.source === cat.source  //true ; same constructor

//access object's prototype
Object.getPrototypeOf(dog) // Function : animal
dog.__proto__  // Function : animal ; 'non-standard' way
dog.__proto__ === animal.prototype // true

obj.hasOwnProperty(prop) //true if a property is object’s own.
prop in obj  // true if own or inherited 

// only own properties are visible in object (like in log)

```

_Constructor functions_ : used to construct new objects ; they're reusable object creation code
```js
function makeUser() { this.name = name ; }
// skipping new pollutes global scope
```

_Chaining prototypes_
```js

function animal(id) {this.id = id;} 
function human(id, num) { animal.call(this, id); this.money = num; } //call animal constructor

Object.setPrototypeOf(human.prototype, animal.prototype);  //chain prototypes ; setting human.prototype.prototype
human.prototype = Object.create(animal.prototype)  //PREFERRED method (more perfomative)

// extending objects like 'girl' down prototype chain
animal.prototype.info = /* code */ ;

```

## Factory Functions

These functions create objects and return them. 
Solve problems with constructors, emulate public and private members.

```js
//factory function

function createUser (name) {
	let nitro = false;  //private member, possible due to closure
	let discordName = '@' + name; //public member
	let buyNitro = () => nitro = true; //privileged method that access private data.
  return {name, discordName, showNitro() { return nitro }  }; //exposing members by return
}

olive = createUser('olive') //no new syntax
```

### Implementing Inheritance

```js
const proto = {  //custom prototype object, to extend, simply add properties here
  hello () {
    return `Hello, ${ this.name }`;
  }
};

// concatinative ; no reference among objects
const mkUser = (name) => Object.assign({}, proto, {name}); 

// differential inhertiance ; register custom prototype
const mkUser2 = (name) => Object.assign(Object.create(proto), {name}); 

//functional inheritance ; factory + concat
function createPlayer(id) {
	let {discordName} = createUser(id); //only extract what is needed 
	/* rest of code */
}

```

### Implementing Composition

```js
const learner = (state) => ({   // () -> return { learns() }
  learns() { console.log(`learns ${state.what}`);}
})

const earner = (state) => ({ 
  earns() { console.log(`earns from ${state.what}`);}
})

const developer = (name) => {
	let state = { name, what: 'coding'};  // internal state object
	return Object.assign({}, learner(state), earner(state) )  // {learns(), earns()} with state bound
}
```


## Module Pattern

In this, an IIFE is used as factory function. It allows easy encapsulations and namespacing (a technique to avoid name collisions). It cannot be reused to create additional instances.

```js
const calculator = (function () {
  const add = (a, b) => a + b;
  const sub = (a, b) => a - b;
  return { add, sub };  // calc -> returned object
})();

calculator.add(2,4) //6

//Dependency injections
const myLib = (function(d1, d2, d3) {
  //code
})(obj1, obj2, obj3);

```

## Class pattern

Classes are technically functions and built over prototype-based constructors, with few differences 
 - in class, constructor() has to be defined explictly
 - class methods are non-enumerable
 - class uses strict-mode
 - class can’t be invoked without new keyword
 - class declarations have TDZ, unlike func_decl

Added to class prototype
 - no => static fields, private methods => can't be accessed with `super` or `this` inside instance
 - yes => everything else => accessed on object ; shadowed by object/subclass own properties

Note :
 - class follow `object` syntax 
 - in class, use (`this.prop`) ; for static (`myclass.prop`)
 - `#times` => private member ; visible only inside defining class ; non-inheritable, non-deletable ; if instance tries to access --> 'undefined'.
 - `static` properties are created on the class itself instead of each instance. Used for providing utilites / shared state. Inherited to sub-classes.
 - `super` keyword is used to access public properties in a super-class, or invoke its constructor
 - super isn't object ; can't use `delete super.prop`

```js
// to access private members, static is used
static getX(obj) {
    if (#x in obj) return obj.#x; //#x present in obj, but only class can see it
}
```

### Inheritance & Composition

Two ways to create super-class
```js
class child extends father // can be constructor function
Object.setPrototypeOf(child.prototype, father.prototype) // extend is just sugar

class sukuna extends null // valid ; 
class gomie extends class {  /*code */ } // class can be expression
const Hello = class optionalName{...}
```

Bound functions and Proxy can be constructed, but they don't have a `prototype` property, so they cannot be subclassed.

When extending built-in classes, return of built-in methods are instances of subclass ; to override this 
```js
class MyArray extends Array {
  static get [Symbol.species]() { return Array; }
}
```

A function with a superclass as input and its subclass as output can be used to implement mix-ins.
```js
const getSub = (Base) => class extends Base { /* code */ };
class Bar extends getSub(Foo)
// can be chained  
class child extends mixin1(mixin2(grandpa))
```

To implement Composition, class refer to an object of another class, and only uses that object instead of extending whole class.

```js
class newMap {
  #data;
  constructor() { this.#data  = new Map(); }
  /* rest of methods */
}
```

```js
//simulate private constructor
class Hello {
  static #flg = false;
  static create() {
    Hello.#flg = true;
    return new Hello();
  }
  constructor() {
    if (!Hello.#flg) throw 'error'; // outside can't see flg
    // logic if true
    Hello.#flg = false;
  }
}
```