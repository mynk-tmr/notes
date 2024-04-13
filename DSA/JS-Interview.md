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
        key = keys[i-1].slice(1) + "." + keys[i].slice(0, -1)
        --i
    ans = { [key] : ans ?? finalValue }
    
return ans;   
```

