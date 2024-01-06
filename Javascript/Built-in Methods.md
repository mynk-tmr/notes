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
Object.preventExtensions(obj) //no new props
Object.seal(obj) // no (+) (-) props , sets config-false for all
Object.freeze(obj) // seal and write-false all

//test
Object.isExtensible(obj)
Object.isSealed(obj)
Object.isFrozen(obj)

//group object based on cb's return for each obj
Object.groupBy(arr_of_obj, cb) //return sync with original

Object.entries(obj) //2D array, with a entry as [own_enum_prop, value]
Object.fromEntries(array_2d)

// create object with given prototype
child = Object.create(parent, inits); //like {id: 1, name: 'ds'}

//inherit
Object.getPrototypeOf(dog) // Function : animal
dog.hasOwnProperty("type") 
"type" in dog  // true if own or inherited 
```

**property descriptors**
```js
Object.defineProperty(obj, 'name', flags) //will overwrite
Object.defineProperties(obj, {name: flags}) //multi
Object.getOwnPropertyDescriptor(obj, 'name')
Object.getOwnPropertyDescriptors(obj)

//flag aware cloning -- include all symbols, properties, flags
let clone = Object.defineProperties({}, Object.getOwnPropertyDescriptors(obj));

//no-flag aware
Object.assign(target, obj1, obj2) //exclude prototype
structuredClone(obj) //deep copy of nested objects & circular references
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
.splice(start, 4,...newEle) //return deleted
.sort()
.reverse()
```

**Custom Sorts `sort(callback)`**
- `cb(a,b)` -> elements being compared ; must return `Number`
- If return > 0, then **a is bigger** and sorted behind.
- *random sort* : return `0.5 - Math.random()`

**Iterative methods**
signature : `fn(cb,thisArg)` ; for each element & index, executes cb.

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

 [typed array guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Typed_arrays).

## Number

```js
Number.MAX_VALUE;
Number.MIN_VALUE;
Number.POSITIVE_INFINITY;
Number.NEGATIVE_INFINITY;

//Static methods
Number.parseInt(num, radix) //returns integers, based to radix. 
Number.toString(num, radix) //returns in string format
Number.parseFloat(num)
Number.isFinite(num)   //false if infinity or NaN
Number.isInteger(num)  // type integer 

//Round/Floatise a number
num.toFixed(2) // 2 decimals
num.toPrecision(3) //3 digits
num.toExponential(decimals_in_coeff) //coeff * 10^n where 1≤coeff<10
```

## String

```js
str.concat(...args) 
str.trim() , trimStart(), trimEnd() //remove whitespaces
str.padStart(4, “de”), padEnd() // dede<str>
str.repeat(N) //string copied N times

//char related
str[4]    //undefined
str.charAt(4) // ''
str.charCodeAt(4) //UTF-16, NO match - NaN

String.fromCharCode(...unicodes) //returns a concat string from unicodes

utf_str.normalize() // converts to normal string. e.g. "\u5043\u3422\u4722" becomes some readable text

//Search Methods — all are Case-Sensitive by default.

[bool] startsWith(substr, check_upto) or endsWith()
[bool] includes(substr, check_from)
[pos] indexOf(substr, check_from) or lastIndexOf() //reverse search
[pos] search(/regex/) //string arg -> regex
[arr or null] match(/regex/) //array of matches
[arr] split(/regex/) //array of non-matches (default : arr[0] =str)

// String Extraction 
replace(/regex/, inserted) //first match replaced (unless /g)
slice(0,8)  // 0-7 returns '' if  +end < +start
substring(0,8) //0-7 -ve indices treated as 0; if end < start, it swaps args
substr(0, 4) //4 length
```

## RegExp

```js
const regex1 = /ab\\+c/g; // treat + as '+'
const regex2 = new RegExp("a(b\\+)c", "g"); // flags are immutable

//2 methods [return after first match]
regex2.exec("dyab+c83") ///["ab+c" , "b+"] [fullMatch, ...groupMatches] 
regex2.test("dyab+c83") //true

//return range of matches
const regex3 = /foo/dg; //d flag 
regex3.exec("foo bar foo").indices  // [[0,3]]
regex3.exec("foo bar foo").indices  // [[8,11]] ; stateful

//group capture & replace
str.replace(/(-)(\w)/g, "$2$1")
str.replace(regex, (current_match, ...groups) => 
  groups[1].toUpperCase())  //on hit, curr replaced with returned val
```

Regex objects (with /g or /y) are stateful , ie., store `lastIndex` of matched string. **exec** and **test** advance past previous match.

```jsx
myreg = new RegExp("d(b+)d", "g")
myreg.exec("cdbbdbsbz") //lastIndex : 5 ; li is 1-based
myreg.exec("cdbbdbsbz") //lastIndex : 0

/d(b+)d/g.exec("cdbbdbsbz") // lastIndex : 5
/d(b+)d/g.exec("cdbbdbsbz") // lastIndex : 5 ; different literal 
```

[https://cheatography.com/davechild/cheat-sheets/regular-expressions/](https://cheatography.com/davechild/cheat-sheets/regular-expressions/)

## Non-standard

[performance.now()](https://developer.mozilla.org/en-US/docs/Web/API/Performance/now) - no. of milliseconds from page load with microsecond precision