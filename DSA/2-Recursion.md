> Check if a number can be reached from 1 by adding 5 or multiplying with 3 

```ts
fn solve(path=1) {
	//evaluate possiblities
	if(path < ans) return solve(path + 5) || solve(path * 3)
	else return path === ans
}
```

> Flatten an object

```ts
js = {}
fn solve(obj, name='') {
	for(key in obj) {
		val = obj[key], nukey = name + key;
		if(typeof val === 'object') solve(val, nukey + "_")
		else js[nukey] = val;
	}
	return js;
}
```


