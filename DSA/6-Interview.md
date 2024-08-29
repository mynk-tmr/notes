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
	let t1 = this;
	if (args_outer<1) return t1
	return function(...args_inner) { 
		let t2 = this;
		return t1.apply(t2, args_outer.concat(args_inner)) 
}}
```

**toArray**
```js
function toArray(args) { return Array.prototype.slice.call(args); }
```

## DOM

```js
//animate moving element
function moveLeft(elem, dist, ms) { 
	let src = 0, timeId = setInterval(frame, ms); 
	function frame() { 
		elem.style.left += ++src + 'px'; 
		if (src == dist) clearInterval(timeId) 
	} 
}
```
