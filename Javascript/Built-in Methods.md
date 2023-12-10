## MATH CLASS

Note : only `static` methods and can't be used on BigInt

```js
Math.PI
Math.abs(-39)
Math.sign(9) /* -1, 1 or 0 */
Math.sin(radian) ; Math.asin(value) 
Math.log(num); Math.log10(num); Math.log2(num) //num >0
Math.floor(float) ; Math.ceil(float)
Math.min(...args) ; Math.max(...args)
Math.round(3.444) ; Math.trunc(3.444)
Math.sqrt(9) ; Math.cbrt(8) ;
  
//random integers in range [a,b]
mod = b - a + 1; //remove 1 to exclude b
Math.floor(Math.random() * mod) + a;
```

## DATE CLASS

In a date object, time is static and stored as ms from Jan 1, 1970. It does not have any properties, only methods.

```js
today = new Date(); //current timestamp
today = Date.now();
thatDay = new Date("1995-03-25T13:33:55.383")
thatDay = new Date(1995, 3, 25) //omitting hr, min, sec set to 0
ms = Date.parse("Aug 9, 1995") //parsed date for set* methods
```


__Date object Methods__
- set* => accepts ms/parsed date => setHours(), setFullYear()
- get* => return in local time => getHours()
- to* => return in human readable form => toLocaleDateString(), toLocaleTimeString()

```js
// Returns days left in the year
const endYear = new Date(2024, 0, 1); // 2024 Jan 1
let daysLeft = (endYear.getTime() - Date.now()) / 86400_000;
daysLeft = Math.round(daysLeft);
```
  

## OBJECT CLASS  

```js
// Object.assign() create shallow copies (nested objects are copied by reference)
Object.assign(obj, ...CopyUs) //exclude prototype

//Create deep copy => clones nested objects & circular references
newobj = structuredClone(myObj)

// Destructuring exclude non-enumerables & prototype
mergeobj = {...A, ...B}

// Object.create() create empty object with given prototype
child = Object.create(parent, initprops); 
child = Object.create(null, { p: 42 }); //initialised with p ; unwritable by default

//Object.entries() return 2D array of [own_enum_prop, value] as elements
Object.entries("fo") // [ ['0', 'f'], ['1', 'o']]
Object.keys(obj) Object.values(obj)

// Object.groupBy(arr_of_objs, callback) : execute cb on each ele and group elements based on callback’s return.
result = Object.groupBy(students, ({subject}) => subject);

// result = { maths : [...] , science : [...] }
// refers original elements, changes sync
// Map.groupBy() groups elements based on any return type, not just string
```


## Array

```jsx
box = new Array(40); //40 undefined elements, slower than a=[]
box2 = Array.of(9.3); // 1 element 9.3
box3 = Array.from(obj); // like ...spread but universal
box.length = 5 ; // truncate array , 0 empties it

//2D arrays 
const arr = [[1,2]]  //NO a = [][] syntax in JS
arr[1] = [3,4]  // Can't use a[1][0] = 3.. since arr[1] isn't 2D yet
a[1][1] = 5 // now valid.

//redefine arr to Object ; length is unaffected
arr['hello'] = 'world';
arr [3.55] = 'hi';  // '3.55'

//Create <empty slots> in array
fish = [1, ,3];  //length 3
fish = [ ,2,3, ]; // length 3 [last comma ignored]
```

Note : empty slots can behave like `undefined` & skipped in iterative methods (*map keeps them*). If empty slot is manually assigned undefined, it won’t be skipped.

```jsx
arr.at(-1) //last ele

//search, optional fromIndex which supports negatives 
[pos] indexOf(val) , lastIndexOf(val) 
[bool] includes(val) //-ve fromIndex treated as a[0] to a[-i]

[str] join('*') 
[itm] pop() ; shift() 
[new_length] push(...val) ; unshift(...val)

new_array .slice(start, ^end_excl)
new_array .concat(...args) //args appended
new_array .flat(2) // lowers dimensionality by 2 ; remove 2 inner brackets

!!! //modify original
del_items .splice(start, 4,...args) //replace 4 items with arglist
delete ar[1] // creates empty slot, return T/f
.sort() // dictionary sort
.reverse()
```

**Custom Sorts `sort(callback)`**
- `cb(a,b)` -> elements being compared ; must return `Number`
- If return > 0, then **a is bigger** and sorted behind.
- **Random** **sort** : return `0.5 - Math.random()`

**Iterative methods**
```jsx
undefined !!! forEach()
.map(cb) //array of callback’s return values
.flatMap(cb) // map then flat 1
.filter(cb) // array of items for which callback returned true
.reduce(cb, initval) // return of final callback
.reduceRight(cb, initval) //starts from last element

itm .find(cb) //first item for which callback returned true.
itm .findLast(cb)
pos .findIndex(cb) 
pos .findLastIndex(cb)
bool .every(cb) // callback returned true for every item
bool .some(cb) //  ...... for 1 item

//setup reducer
cb(acc,curr,currindex, org_arr) {
	return //to-acc
}
```

`.reduce()` reduces array to a single value by executing a callback for each element and passing the return of previous callback into next call