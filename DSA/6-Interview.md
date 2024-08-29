##### String to Object (Razorpay)
```js
// a.b.c, 4 => { a : {b : { c : 4}}}
keys = str.split('.'), n = keys.length;
ans = null;
for(i : n-1 to 0)
    key = keys[i]
    if(key.match(/\d+/))
        ans = [ans]
        continue;
    if(key.at(-1) === '"')
	    key = keys[i].slice(0, -1)
	    while(keys[--i][0] !== '"')
		    key = keys[i][0] + "." + key
		key = keys[i].slice(1) + "." + key
    ans = { [key] : ans ?? finalValue }
    
return ans;
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
	var this_fn = this;
	return function(){ 
		return this_fn.apply(ctx, arguments)
	}
```

**Currying**
```js
Function.prototype.curry = function(...args_outer) {
	let outer_fn = this;
	if (args_outer<1) return outer_fn
	return function(...args_inner) { 
		let inner_fn = this;
		return outer_fn.apply(this, args_outer.concat(args_inner)) 
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