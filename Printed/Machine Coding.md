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

