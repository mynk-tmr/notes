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

**FLAT**
```js
function flat(depth) {
    let res = [];
    flatten(this, 0);
    return res;

    function flatten(arr, currentDepth) {
      for (let val of arr) {
        if (Array.isArray(val) && currentDepth < depth)
	        flatten(val, currentDepth + 1);
        else
	        res.push(val);
      }
    }
}
```

​**Promise**
```js
function myPromise(executor) {
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
	this.then = function(success, failure) {
		successCB.push(success);
		if(failure) errorCB.push(failure);
		return this;
	}
	this.catch = function(handler) {
		errorCB.push(handler)
		return this;
	}
	queueMicrotask(() => executor(resolve, reject))
}
```

**setTimeout**
```js
(() => {
  const timers = new Map();
  window.mySetTimeout = function (callback, delay) {
    let startTime = Date.now(), id = startTime;
    timers.set(id, {
      callback,
      endTime: startTime + delay
    });
    return id;
  }
  window.myClearTimeout = function (id) {
      timers.delete(id)
  }
  function checkOnIdle() {
    for (let [id, timeout] of timers.entries()) {
      const { endTime, callback } = timeout;
      if (Date.now() > endTime) {
        callback();
        myClearTimeout(id);
      }
    }
    requestIdleCallback(checkOnIdle);
  }
  requestIdleCallback(checkOnIdle);
}
)()
```

**setInterval**
```js
(() => {
  const intervals = new Set();
  window.mySetInterval = function (callback, delay) {
    const id = Date.now();
    intervals.add(id);
    
    const inner = () => {
        if (intervals.has(id)) {
          callback();
          repeat();
        }
      } 

    const repeat = () => {
      setTimeout( inner, delay)
    }
    repeat()
    return id;
  }
  window.myClearInterval = function (id) {
    intervals.delete(id);
  }
}
)()
```

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