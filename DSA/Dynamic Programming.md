##### Modelling DP problems
- recursion must branch (many choices are there)
- look at problem in terms of indices
- for each index, try all possible paths
	- requires `prev(s)` subsolution
- pick the best subsolution
	- all possible ways --> sum them
	- min/max/or/and --> apply these to them

##### Pick & Ignore method

```js
//memoize approach
if index is N 
	if subsoln passes, return this or false
pick item[i] & call fn //with updated subsoln
ignore item[i] & call fn //same subsoln
return pick <op> ignore
```
