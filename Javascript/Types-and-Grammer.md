
## Data structures

### Iterables

Objects that can be used in `for..of` are called _iterable_. eg. `Array` `String`. To make iterables
- implement `[Symbol.iterator]()` which returns _iterator_ object
- implement `next()` in *iterator* which returns value, each time `for of` calls it.

```jsx
let range = {
  from: 1,
  to: 10,
  [Symbol.iterator]() {
    return {
      pos: this.from,
      quit: this.to, //can be Infinity
      next() { //done, value are keywords
        return this.pos < = this.quit ?
          { done: false, value: this.pos++ } :
          { done: true };
	}}}}
```
!! don't return `range` itself, 2 simultaneous `for-of` would share state

calling ourselves
```jsx
let iterator = obj[Symbol.iterator]();
while (true) {
  let result = iterator.next();
  if (result.done) break;
}
```

Objects that have indexed properties and `length` are called _array-like_. Don't support *array* methods. Not necessarily same as iterables eg. `range`. `String` is both though.

`Array.from(obj, ^mapFn, ^thisArg)` makes a real `Array` from an *iterable or array-like* using a callback that maps all next() values one by one.
```jsx
let x = Array.from(range, n => n * 2);
```


### Maps and Set

[Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map) is a iterable collection of *unique keyed* data items, just like an `Object`, but allows keys of *any* type (even objects). Object allows only `String` or `Symbol`. Any other type is coerced to string.

Set is a iterable collection of *unique* *values* of any type. They don't maintain insertion order and ignores duplicate values in `set()`

Methods and properties in both
- `new Map/Set(iterable)`
- `.size` : length
- `map.set(key, value) / set.add(val)` : Returns new map/set ; overrides previous
- `.get(key)` – returns val or `undefined`
- `.has(key)`
- `.delete(key)` - return t/f
- `.clear()`
- `.keys(), .values(), .entries()` - return iterable array-like. Use Array.from or [...]

Map uses *SameValueZero* check (NaN can be key).
Map preserves insertion order unlike objects (seen in `for of`)

```jsx
new Map([[1, 'hello'], [2, 'bye']]); //for key-values
new Map(Object.entries(obj)); //from obj's key-values
Object.fromEntries(map) //create object

unique_arr = (arr) => Array.from(new Set(arr))

```

#### Weakmap & Weakset

```jsx
let john = { name: "John" };

let array = [ john ];

john = null; // overwrite the reference

// the object previously referenced by john is stored inside the array
// therefore it won't be garbage-collected
// we can get it as array[0]
```




### Control Flow 

**Conditional stmts**
- `case: ` tests for *strict* equality ; case can be enum values / string object
- `switch` is faster -> due to jump table creation during compilation
- `if-else` is more flexible than switch

**Loops**
- entry controlled (check first, then exec) -> `for` `while`
- exit controlled (execute first, then check) -> `do while`
- `for` evaluates step at end of current iteration
- In ONLY while & do while loops, *postfix* runs *1 extra* than prefix.
- `for in` iterates over *enumerable* properties (enum flag `true`) 
- `for of` iterates over *values* of **iterable** objects (built-in or user-specified)
	- both pass by **value** to key/val
- `for await(.. of ..)`
	- iterate over async oR sync objects
	- can trigger custom iteration workflows for each property
- `onix : for(key in obj)`
	- onix is *label*, an identifier for *codeblock* that you can refer to elsewhere
- `break` terminates current (or *labelled*) loop
- `continue` *jumps* control-flow to next iteration. In for, it updates STEP. Given *label* must be LOOP
- you can only jump to *parent* label

**Ternary/conditional operator**
- acts on 3 operands and works like an *inline-if-else*.
- returns value of expr chosen to be evaluated.
- can't work with `break` `continue` `return` (can't control flow)

### Error Handling

**Stack trace** : list of function calls that lead to an error -> latest to earliest

`try {..}`
- creates a codeblock where any error transfers control to *first catch block* in call stack. 
- To specify user-defined exception, we have `throw val` (val can be any value/function)
- Error object is captured and visible only inside `catch` block. 

`finally {..}` : always execute *immediately* after `try` and `catch` blocks. *Overrides* rethrows/returns in `try-catch` with its own return (if given)

#### Error objects

All errors are *serializable* object, so can be cloned with structuredClone() or copied between [Workers](https://developer.mozilla.org/en-US/docs/Web/API/Worker) using postMessage()

**Types**
- `ReferenceError` — accessing undeclared or uninitialized variables
- `SyntaxError` — incorrect syntax
- `TypeError` — invalid operands/arguments; invalid use or change to a value
- `StackOverflowError` — when number of execution contexts exceed size of host’s stack

**Properties**
- `name` , `message` , `stack` 


**Equality checks**
- *loosely equal* : equal after type conv.
- *strictly equal* : same type & val
- *samevalue* : treates `NaN` as equal, but `-0` =/= `+0` . USed by `Object.is(val,val)`
- *samevalueZero* : treates `NaN==NaN`  `+0 == -0` . Many built-in use this e.g. Map

