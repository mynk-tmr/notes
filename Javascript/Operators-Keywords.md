
##### Precedence table

- ()
- fn calls, prop_access & (empty constructor)
- postfix
- other unary => prefix, `! ~ + -` `typeof, void, delete`
- arithmetic => `** ,  */ , -+` 
- bitwise shifts => `>> >>> <<`
- compare => `in, instanceof, < ≤ > ≥`
- equality checks
- logical -> 7 to 3 => `& ^ | && || ??` (or=nullc)
- assignments : eg. `=>  x:'s'  +=  ...spread, yeild, return` *right associatives*
- comma : evaluates all expression on a single line, but only return result of last e.g. `(1 + 2, 3 + 4) -> 7`

---
##### Type Coercion

```jsx
null + 2 == 2  //0 coerced
undefined / 3 == NaN //Number(undefined) = NaN
'hello'.toUppercase() //auto object coercion
'3' > 2 //true ; on comparing diff types, they're number coerced

(2+undefined) == NaN //false NaN != NaN , use isNaN()

String(any) or '' + any //’undefined’, ‘null’, 'funct...', 'NaN'
String([1,[[[2]]]])  // '1,2'

isNaN(undefined) //true ; coerce to number
Number.isNaN(undefined) //false ; No coercion
```

---
##### Short-Circuit Operators

Operators which always evaluate the *left* operand first, and then right only if left’s evaluation can’t determine the result of entire expression. It only affects **evaluation order** of operands, not final result. These are — **&&, ||, ??, ?**  and their assigment counterparts

If multiple short-circuits are used, they are **grouped** but circuits evaluates left to right no matter the precedence.

```jsx
let A, B, C return false, false, true
C() || B() && A() // only C called
A() && C() || B() // A & B called
```

Notes
- **nullish** values : `null` `undefined`.  They are *loosely* equal. 
- **falsy** values : `false == 0 ==""`, `NaN` , `nullish`   ( `Boolean(val)` returns false)
- **truthy** values : other values including `"0"` and space-only strings like `" "`
- !!x -> Boolean(x)
- `&&` returns first *falsy* operand 
- `||` returns first *truthy* operand
- `??` returns first *defined* (non-nullish) operand

********************Optional Chaining :******************** **?.** checks left operand and if it’s **nullish**, the expression short circuits and evaluates to `undefined` instead of throwing an error.

`obj?.prop  arr?.[]   func?.()`

---
##### Bitwise Operators

operate on individual bits of operands. They are treated as signed int, but result is treated as signed `number`.

**and (&) or(|)  xor(^)** : return 1 for each bit positions where both 1 / atleast one is 1 /  both same.

**x << 2** left shift bits, right is padded by 0

**>>** right shift bits ; left is padded by sign bit

**>>>** unsigned right shift ; left is padded by 0

Note : Negative integers are stored in 2's complement, obtained by inverting bits of +ve value, then adding 1.

---
##### Unary Operators

```jsx
[bool] delete obj[prop]
typeof op // string
void op //evaluate op and return undefined
func = () => void doSomething(); //ensure func() never returns anything
```

```jsx
typeof null; //  "object"
typeof Date; // "function" .. class
typeof variable; // "undefined"
typeof document.all // "undefined"
```

---
##### Spread & Rest

Spread syntax "expands" an *iterable* into its elements OR an object into its *own_enum_key : value* pairs . It can be used anywhere. Empty, falsy, non-iterables spread nothing. 

Rest syntax collects multiple elements and "condenses" them into a single array. It is only allowed on last parameter.

Spread syntax cannot mutate an object and cannot trigger setters, unlike `Object.assign()`

```jsx
arr2 = [...arr]; // copy
arr1 = [...arr1, ...arr2]; // merge
mergeobj = {...A, ...B} //exclude non-enum & proto
```

---
##### Destructuring Assignment

Unpacks values from *objects & iterables* into distinct variables. It combines `for of , =` . Default values apply only for strictly `undefined` and are lazily evaluated.

iterables
```jsx
let [a=3, b,  , ...rest] = arr;
let [a, ...r] = 'hey'; // a= 'h' ; r = ['e', 'y']
//swap
[arr[1], arr[0]] = [arr[0], arr[1]]

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

```jsx
func({id=211} = {}) {..} //to allow call with func(), else error
```

---
**`new` keyword** : calls a method in *constructor* form and returns an object.

- If no `return`, it returns `this` object of constructor
- otherwise
    - If `return` is object, then object is returned
    - If `return` is primitive, it’s ignored.

---
##### Strict mode `‘use strict’;` 

Semantic changes
1. silent errors may *throw* e.g. invalid assignments, invalid deletion
2. run *faster* than identical sloppy code
3. Prohibits some syntax for *future-proofing*
4. Easier to write *‘secure’* Javascript e.g. can't access `callee` , `caller` and `arguments` in strict fn

Notable changes

- no `globalThis` substitution (now `undefined` ). Can’t skip `var`
- **block-scoped function declarations**
- In functions -> rest, default, or destructured parameters is a syntax error.
- use `delete window.x`
- No duplicate parameters or object properties
- Non-leaking eval : `eval("var x;")` ⇒ variables are scoped to eval only
- Non-updating immutable `arguments` : stores original parameters

---

delete op: `true` for all cases *except* when the property is an [own](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/hasOwn) [non-configurable](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty#configurable_attribute) property