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



