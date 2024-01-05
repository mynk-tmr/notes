
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
  return {discordName, showNitro = () => nitro}; //exposing members by return
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
const learner = (state) => ({ 
  learns = () => `learns ${state.what}`
})

const earner = (state) => ({ 
  earns = () => `earns from ${state.what}`
})

const developer = (name) => {
	let state = { name, what: 'coding'};  // internal state object
	return Object.assign({}, {name}, learner(state), earner(state) )
}
```


## Module Pattern

In this, an IIFE is used as factory function. It allows easy encapsulations and namespacing (a technique to avoid name collisions). It cannot be reused to create additional instances.

```js
const calculator = (function () {
  const add = (a, b) => a + b;
  const sub = (a, b) => a - b;
  return { add, sub };  // -> returned object
})();

calculator.add(2,4) //6

//Dependency injections
const myLib = (function(d1, d2, d3) {
  //code
})(obj1, obj2, obj3);

```

## Class pattern

Classes are based on prototype-based *constructor functions*, except
 - `constructor()` is **explictly** defined & requires **new** to be called
 - class methods are **non-enumerable**
 - in **strict-mode** and have **TDZ** like let

**Public properties**
```js
class Myclass {
	name = ''; //instance field ; added to class prototype
	static type = 'myclass'; // not added to proto; created on class itself
	constructor(val) {
		this.width = '100'; //instance field verbose
		this.name = val; //overrides default field
	}
}
```

**super**
- keyword used to *access* *public* props in superclass, or invoke its constructor
- immediately after `super()` is called, fields in base class are added to subclass.
- not a object, can't use `delete super.prop`

**Private fields** 
- not added to class prototype ; NOT inherited, non-deletable
- like metadata of instance, that only class knows
```jsx
class Privateclass {
	#id; //added directly to instance, but hidden from it 
	constructor(val) {
		this.#id = `${val}`
	}
	//to make them visible
	static getId(obj) { 
	    if (#id in obj) return obj.#x; 
	}
}
```

### Inheritance & Composition

Two ways to create super-class
```js
class child extends father // can be constructor function
Object.setPrototypeOf(child.prototype, father.prototype) // extend is just sugar

class God extends null // valid ; 
class Inline extends class {  /*code */ } // class can be expression
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
  data = new Map();
  /* rest of methods */
}
```

```js
//simulate private constructor
class Hello {
  static #created = false;
  static create() {
    Hello.#created = true;
    return new Hello();
  }
  constructor() {
    if (!Hello.#created) throw 'bad instantiation'; 
    //else rest of code...
    Hello.#created = false;
  }
}
```