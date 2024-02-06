
*ES6 fundamental datatypes* of which 6 are Primitive
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
- LEX is referred by function's `[[Environment]]` property, and through LEX, a function can access outer variables around its *definition*
- When function(s) are created / returned, they have their own *fresh copy* of closure and can only modify it
- Like plain objects, LEX isn't garbage collected till its referenced
- Closures can capture variables in *block/module* scope too

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

