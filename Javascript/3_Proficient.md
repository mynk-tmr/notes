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