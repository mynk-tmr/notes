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
new Date() //current time obj
Date.now() //ms timestamp
new Date(0); //1 jan 1970 ; ms
new Date(-86400_000); //31 dec 1969
new Date("1995-03-25T13:33:55.383") 
new Date(1995, 3, 25) //omitted set to 0
ms = Date.parse("Aug 9, 1995") //parsed date for set* methods

//month & sunday start from 0
```

smart objects
```jsx
ti.setDate(feb_28.getDate() + 2) //mar 1 or mar 2 (auto-leap-check)
why.setMinutes(why.getMinutes() + 25) //+25 minutes
dt.setDate(0) //18 apr -> 31 march

dt - why //subtract ms

function getLastDayOfMonth(year, month) {
  let date = new Date(year, month + 1, 0);
  return date.getDate();
}
```

__Date object Methods__
- set* => accepts *ms/parsed* date => setHours(), setFullYear()
- get* => return in *local* time => getHours()
- to* => human readable form => toLocaleDateString(), toLocaleTimeString()


**Utilites**
```js
// Returns days left in the year
const endYear = new Date(2024, 0, 1); 
let daysLeft = (endYear.getTime() - Date.now()) / 86400_000;
daysLeft = Math.round(daysLeft);

getWeekDay = (date) => {
	let days = ['SU', 'MO', 'TU', 'WE', 'TH', 'FR', 'SA'];
	return days[date.getDay()]
}

function secondsLost() {
  let d = new Date();
  return d.getHours() * 3600 + d.getMinutes() * 60 + d.getSeconds();
}
```
  

## OBJECT CLASS  

```js
// Object.assign() create shallow copies (nested objects are copied by reference)
Object.assign(obj, ...CopyUs) //exclude prototype

//Create deep copy => clones nested objects & circular references
newobj = structuredClone(myObj)

// Object.create() create empty object with given prototype
child = Object.create(parent, initprops); 
child = Object.create(null, { p: 42 }); //initialised with p ; unwritable by default

//Object.entries() return 2D array of [own_enum_prop, value] as elements
Object.entries("fo") // [ ['0', 'f'], ['1', 'o']]
Object.keys(obj) Object.values(obj)

//applying array methods on obj
prices = Object.fromEntries(
  Object.entries(prices).map(entry => [entry[0], entry[1] * 2])
);

// Object.groupBy(arr_of_objs, callback) : execute cb on each ele and group elements based on callback’s return.
result = Object.groupBy(students, ({subject}) => subject);

// result = { maths : [...] , science : [...] }
// refers original elements, changes sync

Map.groupBy() //groups elements on any return type
```


## Array

```jsx
box = new Array(40); //length -> 40
box2 = Array.of(9.3); // 1 element 9.3
box3 = Array.from(obj); 
box.length = 5 ; // truncate array , 0 empties it

//2D arrays 
const arr = [[1,2]]

//redefine arr to Object ; length is unaffected
arr['hello'] = 'world';
arr [3.55] = 'hi';  // '3.55'

//Create <empty slots> in array
fish = [1, ,3];  //length 3
fish = [ ,2,3, ]; // length 3 [last comma ignored]
```

Note : empty slots can behave like `undefined` & skipped in iterative methods (*map keeps them*). If empty slot is manually assigned undefined, it won’t be skipped.

```jsx
arr.at(-1) 

//search, optional fromIndex which supports negatives 
`pos` indexOf(val) , lastIndexOf(val) 
`bool` includes(val) //-ve fromIndex treated as a[0] to a[-i]

`DONT MODIFY ORIGINAL`
.join('*') //stringify
.slice(start, ^end_excl)
.concat(...args) //args appended
.flat(2) // lowers dimensionality by 2 

`MODIFIERS`
.pop()  .shift() 
.push(...val) .unshift(...val) //new_size
.slice(start, 4,...newEle) //return deleted
.sort()
.reverse()
```

**Custom Sorts `sort(callback)`**
- `cb(a,b)` -> elements being compared ; must return `Number`
- If return > 0, then **a is bigger** and sorted behind.
- *random sort* : return `0.5 - Math.random()`

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
	return x; //if no initval, 1st ele -> acc, 2nd ele -> curr
}
```

`.reduce()` reduces array to a single value by executing a callback for each element and passing the return of previous callback into next call

## Non-standard

[performance.now()](https://developer.mozilla.org/en-US/docs/Web/API/Performance/now) - no. of milliseconds from the start of page loading with microsecond precision