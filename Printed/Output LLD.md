## Output

**Conversions Output**

```js
null + 2 == 2  ; '3' > 2 //true
[] == true // false ; [] --> '' --> 0 ; true --> 1 
NaN ** 0 === 1
-'34'+10 // -24
+'dude' //NaN

(1+2, 3+4) //7

String(val) or '' + val //’undefined’, ‘null’, 'funct...'
String([1,[[[2]]]])  // '1,2'

//truthy objects
new Boolean(false) ; new String('')
```


**TYPEOF Shenanigans**

```js
typeof Date; // "function"
typeof variable; // "undefined"
typeof document.all // "undefined"
typeof NaN // 'number'

var y = 1, x = y = typeof x // 'undefined' ; = is evaluated RtL
typeof [] // 'object' ; to check array use Array.isArray([])
```

**NaN**

```js
(2+undefined) == NaN //false NaN != NaN
isNaN(undefined) //true ; coerce to numbe
Number.isNaN(undefined) //false ; No coercion
Math.max([2,3,4,5]); // NaN
```

**Misc**
```js
'box' > 'adam' == true //b after a
3 instanceof Number //false
2 in [1,2] //false ; checks property (in array; props are '0' , '1')
```


**Solving Hoisting Problems**
* check all declarations (func, var) and hoist names
* overshadow same names and assert


**Closures Questions**

```jsx
obj = {
  getThisGetter() { return () => this },
};
fn = obj.getThisGetter(); // fn is arrow, fn() == obj
hof = obj.getThisGetter ; //hof()() === window  
```


**Object-Oriented Questions**

```js
var obj1 = { price: 20.99, get_price : function() { return this.price; } }; 

var obj2 = Object.create(obj1); 
delete obj2.price; 
console.log(obj2.get_price()); // 20.99 ; from the Prototype chain, gets obj1 price
```


**Counters & Timer Questions**

```js
for(var i = 0; i < 10; i++)
    setTimeout(function() { console.log(i) }, i); //immediately prints 10 times 10

//to fix
for(var i = 0; i < 10; i++)
    setTimeout(console.log.bind(console, i), i * 1000);
```

Reason : `setTimeout` is executed when current call stack is over (loop finishes). However, functions keep a reference to i by creating a closure. Value i has been set to 10 after loop's end.

```js
function printNumbers(from, to, delay = 1000) {
  for (i = from; i <= to; i++)
    setTimeout(console.log, delay, i);
}

printNumbers(1, 5); // 1sec delay --> immediately output 6 6 6 6 6 
//var i is func scoped so callbacks refer 6 at end of script
```

**Promise-Based Questions**

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

## Utils / Lib

**Small Hacks**
```js
Math.max.apply(null, arr); //max of array

//snake case to camel case
str.replace(/(-)(\w)/g, (full_match, ...groups) => groups[1].toUpperCase())
```

**Partialiser**
```js
function partializer(original_func, ...bindUs) {
  return function(...args) {  
    return original_func.call(this, ...bindUs, ...args); 
  }
}
```

**Cache Decorator**
```js
function addCache(orgFunc) {
  const cache = new Map();
  return function(key) { 
    if (!cache.has(key)) cache.set(key, orgFunc.call(this,key)); 
    return cache.get(key);
  };
}
```

**Curry**
```js
function curry(func) {
    return curried;
    function curried(...args) {
        if(args.length >= func.length)
            return func(...args);
        else
            return function(...more) {
                return curried(...args,...more);
```

**Date-Related**
```js
function getDaysLeftinYear() {
	let end = new Date(2024, 0, 1)
	let left = (end.getTime() - Date.now()) / 86400_000;
	return Math.round(left)
}

function getWeekDay(date) {
	let days = ['SU', 'MO', 'TU', 'WE', 'TH', 'FR', 'SA'];
	return days[date.getDay()]
}

function getSecondsFromDayStart() {
  let d = new Date();
  return d.getHours() * 3600 + d.getMinutes() * 60 + d.getSeconds();
}
```

## Polyfills

**Bind**
```js
Function.prototype.bind = Function.prototype.bind || function(ctx){ 
	return () => this.apply(ctx, arguments) //inherit this from outer
}
```

**Currying**
```js
Function.prototype.curry = function(...args_outer) {
	let fn_outer = this;
	if (args_outer<1) return fn_outer
	return function(...args_inner) { 
		return fn_outer.apply(this, args_outer.concat(args_inner)) 
}}
```

**toArray**
```js
function toArray(args) { return Array.prototype.slice.call(args); }
```

**Promise.withresolvers**
```js
function() {
	let resolve, reject;
	const promise = new Promise((res, rej) => {
	  resolve = res; reject = rej;
	});
	return {promise, resolve, reject}
}
```

## Currying

**sum(1)(3)(6)()**
```js
function sum(a) {
    let inner = (b) => b === undefined? a : sum(a+b)
    return inner;
}
```

## MNCs

**String to Object (Razorpay) : `a.b.c, 4 => { a : {b : { c : 4}}}`**

```js
function ans(str, finalValue) {
	keys = str.split('.'), n = keys.length, ans = null;
	for(i : n-1 to 0)
	    key = keys[i]
	    
	    if(key.match(/\d+/)) {
			ans = [ans]
	        continue;
		}
	        
	    if(key.endsWith('"')) {
		    temp = key.slice(0, -1)
		    while(!temp.startsWith('"'))
				temp = keys[--i] + '.' + temp
			key = temp.slice(1);
	    }
	    ans = { [key] : ans ?? finalValue }
	return ans;
}
```

