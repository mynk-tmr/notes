**String to Object (Razorpay)**
```js
// a.b.c, 4 => { a : {b : { c : 4}}}

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
		    do {
				temp = keys[--i] + '.' + temp
		    } while(!key.startsWith('"'))
			key = temp.slice(1);
	    }
	    ans = { [key] : ans ?? finalValue }
	return ans;
}
```

## Utils / Lib

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

**sum(1)(3)(6)**
```js
function sum(...args) {
    let inner = (...more) => sum(...args,...more)
    inner.toString = () => args.reduce((a, b) => a+b) //total 
    return inner;
}
```

