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
Function.prototype.bind = Function.prototype.bind || function(ctx, ...args){ 
	let fn = this;
	return (...more) => fn.apply(ctx, [...args, ...more] )
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

**Reduce**
```js
function(callback, initialValue) {
    if (typeof callback !== 'function') throw 'error'
    const array = this, hasInit = initialValue !== undefined;
    let acc = hasInit? initialValue : array[0];

    for (let i in array)
        acc = callback.call(undefined, acc, array[i], i, array);
    return acc;
}
```

**Map**
```js
function(callback, thisArg) {
    const arr = this, result = []
    for(let i in arr) {
	    result[i] = callback.call(thisArg, arr[i], i, arr) 
    }
}
```

**Filter**
```js
function(callback, thisArg) {
    const arr = this, result = []
    for(let i in arr) {
	    let val = arr[i]
	    let bool = callback.call(thisArg, val, i, arr);
	    if(bool) res.push(val)
    }
}
```
​
**Find**
```js
function find(cb) {
	const arr=this;
	for(let i in arr) if( cb(arr[i], i, arr) ) return i;
	return -1
} 

findLast <-- ulta loop
```

​**Promise**
```js
function Promise(executor) {
	let successCB = [], errorCB = [];
	function resolve(val) {
		try {
			val = successCB.reduce((acc, func) => func(acc), val)
		} catch(err) {
			reject(error);
		}
	}
	function reject(error) {
		let handler = errorCB[0];
		if(!handler) throw 'unhandled rejection';
		try {
			handler(error);
			return this;
		}
		catch(error) {
			errorCB.shift();
			reject(error);
		}
	}
	this.then(success, failure) {
		successCB.push(success);
		if(failure) errorCB.push(failure);
		return this;
	}
	this.catch(handler) {
		errorCB.push(handler)
		return this;
	}
	executor(resolve, reject)
}
```


​

setTimeout

[

Let's write a polyfill for setTimeout.

Well my implementation of setTimeout is inspired from the sleep and requestIdleCallback. For those who don't know what sleep is, sleep is a function which blocks the main thread execution for a specified delay. Below is a sleep implementation. The above function loops until the current time exceeds the delayed time.

![](https://alphaayush.notion.site/image/https%3A%2F%2Fmiro.medium.com%2F1*m-R_BkNf1Qjr1YbyOIJY2w.png?table=block&id=b2292fbb-8c1b-45d8-992e-5cd5e41286fa&spaceId=1605f3ad-9940-4d62-be74-28bf33f4a36b&userId=&cache=v2)

https://medium.com/@choprabhishek630/lets-write-a-polyfill-for-settimeout-24564b1a7142

![](https://miro.medium.com/max/1200/0*qmDEtgvI5GAx-CV2)





](https://medium.com/@choprabhishek630/lets-write-a-polyfill-for-settimeout-24564b1a7142)

JavaScript

Copy

(() => { const timers = new Map(); function checkOnIdle() { for (let [id, timeout] of timers.entries()) { const { endTime, callback } = timeout; if (Date.now() > endTime) { callback(); myClearTimeout(id); } } requestIdleCallback(checkOnIdle); } window.mySetTimeout = function (callback, delay) { var startTime = Date.now(); const id = startTime; timers.set(id, { callback, endTime: startTime + delay }); return id; } window.myClearTimeout = function (id) { if (timers.has(id)) { timers.delete(id); } } requestIdleCallback(checkOnIdle); } )() const id = mySetTimeout(() => { console.log("hi2") } , 1000) myClearTimeout(id)

​

setInterval

JavaScript

Copy

(() => { const intervals = new Set(); window.mySetInterval = function (callback, delay) { const id = Date.now(); intervals.add(id); const _interval = () => { setTimeout(() => { if (intervals.has(id)) { callback(); _interval(); } } , delay) } _interval() return id; } window.myClearInterval = function (id) { intervals.delete(id); } } )() const i = mySetInterval(() => console.log('yo'), 1000) setTimeout(() => myClearInterval(i), 10000)

## Currying

**sum(1)(3)(6)**
```js
function sum(a) {
    let inner = (b) => sum(a+b)
    inner.toString = () => a
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

