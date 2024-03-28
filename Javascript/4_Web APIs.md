A Web API acts as `interface` to use browser's or system's data/functionality. A web app uses many `APIs` to perform tasks.
##### Types of APIs
- `Browser APIs` — built into browser and are able to expose data from browser and system. e.g. Web Audio API
- `Third-party APIs `— built into third-party platforms (e.g. Twitter, Facebook) that allow you to use some of platform's functionality in your web pages
- `JavaScript libraries` — JS files containing **custom** functions that you can use. e.g React.
- `JavaScript frameworks` — packages of HTML, CSS, JavaScript, and other technologies that you use to write an entire web application from scratch

Library vs. Framework — "Inversion of Control". _A developer calls a method from a library. A framework calls the developer's code._

Features of Client-side web APIs
- **Objects-based —** code interacts with APIs using objects, which serve as containers for the data and functionality that API uses and exposes
- **Fixed recognizable entry points.** e.g in Web Audio API — it is the `AudioContext` object. In DOM API, it is Node object
- Use of **events** to handle changes in state
- **Security mechanisms** where appropriate

## JSON

JSON is a **text**-based data format following JavaScript object syntax. JSON exists as a **string**. 

2 methods [json -> array/object]
- `JSON.stringify` (_serialization_)
- `JSON.parse`  (_deserialization_)

Notes
- JSON can be a single primitive
- JSON keys requires double-quotes.
- values that are `function, symbol, undefined` are skipped

```jsx
JSON.parse('{"name":"John"}'); 
arr = JSON.parse('["name":"John"]'); //arr[0] 
JSON.parse(data, (key,val) => {
	//code
	return newValue; // transform values including nested
}) 

//date fields are auto evaluated
```

```jsx
JSON.stringify(obj, ['id', 'name']) //only these, nested keys in them have to be explicity included

//skip values
JSON.stringify(obj, (key, value) => {  //nested key-value also iterated over
	return key == 'under' ? undefined : value
})

//object's toJSON auto-invoked by stringify
let room = {
  number: 23,
  toJSON() { return this.number }
};

//remove circular (self) references
JSON.stringify(meetup, (key, value) => {
  return (key !== "" && value == meetup) ? undefined : value;
})
```

## Constraint Validation API

An `invalid` event (bubble:*false*) is triggered on every invalid field. API supports **button, fieldset, input, output, select, textarea & form**

```jsx
myform.noValidate=true; /*or*/ <form novalidate></form>
//disables default msgs of browser [not validation]

myform.addEventListener('submit', (e)=>{
	e.preventDefault(); //don't submit
	//rest of code
	form.submit(); // if all checks were valid
});
```

Inline validation -> listen events `input` `change` `focus` `blur`

**API methods**
- `checkValidity()` `reportValidity()` (check+msg)
- `setCustomValidity(msg)` : for report

**Field properties**
- `validationMessage` : error msg
- `validity` : an object with bool properties
  - patternMismatch , stepMismatch, typeMismatch (e.g. ❌ url)
  - tooLong, tooShort, valueMissing, badInput (unreadable)
  - rangeOverflow, rangeUnderflow
  - valid (`true/false`)
  - willValidate (`true` if not hidden, readonly or disabled)
  - customError (`true` if custom msg was set)

Html's `pattern` test with \\g \\m \\i flags disabled. So use JS.

## Sending Files

##### Files
```ts
x = ele.files //x -> name, type size
x = e.dataTransfer.files //prevent default, stop propagation prior

//using filereader
reader.error  
reader.readyState // 0 1 2
reader.result //depends on reading method

//filereader events
abort, error, loadstart, progress, load /*only if success*/ , loadend

//URL interface
URL.createObjectURL(x : File)
URL.revokeObjectURL(x) //memory management, do it upon onload on img, video
```

## Fetch

A *promise* based API to send/get network resources using http headers

```jsx
const response = fetch(url, options);

//using constructor
const heads = new Headers({ "Content-Type": "application/json"})
const request = new Request(url, heads) //request.url ; request["Cont..]
```

**Options object**
- `body: JSON.stringify(data)` 
- `headers: JSON.stringify(obj)` : set HTTP headers
- `mode` : origin policy -> `cors`, `no-cors`, or `same-origin`
- `referrerPolicy` : 
- `method` : (GET, POST, PUT, DELETE, etc)
- `cache` : use from cache? `no-cache` `force-cache` 
- `credentials` : send/get cookies ? -> `omit` `same-origin**` `include` 
- `signal : ctr.signal`: set abortcontroller

**Headers object**
- have a _guard_ to prevent malicious changes
- methods
	- `.append(key,value)`, Map methods
	- `.getSetCookie()` : only on nodejs

**Response objects**
- returned when fetch promises are resolved
- `res.ok` (t/f) `res.status` (404) `res.statusText` 
- fetch rejects only when a network error or CORS is misconfigured

```jsx
//methods on Request and Response to extract body [return Promise]
// can only run once
.arrayBuffer() .blob() .formData() .json() .text()
```

## Window

```jsx
let jsWindow = window.open(url,'Google', features); //like height,width
//now .open() new url & .close() 

jsWindow.opener //reference to window that opened it ; data-sharing

// size & position related
window.resizeTo(600,600)   // width, height
window.resizeBy(-100,-100) // relative W, H
window.moveTo(0,0)         // X, Y coords
window.moveBy(100,100)
```

#### window.location

```jsx
href //url string
pathname // after host/
search //search string [?...]
hash   // #title

.assign(url) //goto
.replace(url)  //navigate but no push in stack
.reload(/*true*/)   //reload from cache OR from server
```

```jsx
const urlParams = new URLSearchParams(location.search); 
for ([key, value] of urlParams) {  //iterable
}

urlParams.has('brand')  //true  ; parameter check
```

#### window.navigator

represents state and identity of user agent. "browser sniffing" and “fingerprinting” properties often return fake values.
```jsx
userAgentData.brands[0]  // {brand: 'Google Chrome', version: '119'}
userAgentData.mobile  //false//keys(), values(), entries()
userAgentData.platform // 'Linux'
connection.downlink  // 10   /mbps/
deviceMemory  //8
languages  // ['en-IN', 'en']
language  // 'en-IN'
```

API to send analytics to server
```jsx
subscribeBtn.onclick = () => {
    navigator.sendBeacon(url, data); //server handles this info
  }
//data can be any type (Blob, formData etc)
```


## History API

provides a way to manipulate browser history to create SPAs and non-reloading navigation

The **history stack** stores the browser’s session history (in tab or frame). To manipulate it, use  `window.history` object. 

```jsx
//control navigation
history.back()  //visit previous page
history.forward() //visit next page
history.go(-2) // go to 2nd last page.
history.go() // reload current page [0]
history.length // no. of pages in history stack
```

```js
//manipulate history [state object, title, append to URL]
history.pushState({page: 1}, "Introduction", "?page=1")
// and replaceState

`onpopstate` //fires upon navigation; has `state` prop for a copy of pushed/replaced state object

//correct way to handle
window.onpopstate = () => setTimeout(doSomeThing, 0);
```

These methods follow **same-origin** url policy.

## Cookies API

A **cookie** is a small piece of **data** exchanged between server and browser to create **statefulness** in requests and responses.
Main uses — session management, personalisation, tracking user.
Cookie types — **permanent** (have expiry date) and **session** **(no expiry; lasts 1 session)

`document.cookie` — accessor property to CRUD cookies.

```jsx
//can only set 1 cookie at time; sets name & id
document.cookie = "name=John; max-age=100;"
document.cookie = "id=3233; max-age=100;"

//decode URI (%20 -> ' ')
let cookies = decodeURIComponent(document.cookie)

// format cookies
document.cookie.split(';').map(coo => { 
	coo.trim(); 
	return coo.split('=') 
})

// find and return a cookie value
cookies.find(coo => coo.startsWith("name="))?.split("=")[1];
```

Parameters
```js
`expires=${dateobj.toUTCString()}` //default session
`max-age=100` //seconds after session end; if set to -ve, will expire
`domain=example.com` //domain to which cookie will be sent (same-origin)
`path=/path/to/host` //on domain
```

## LocalStorage and IndexDB

**Local storage vs. Cookies**
- Cookies are for client-server, Local storage are for client only
- Cookies are sent in *every HTTP headers* and can be disabled
- Cookie has a size limit of 4 Kb. Local Storage can be any.
- Cookie has a expiration date.

## Console and Dialog

```jsx
//**synchronous** modals - stop code execution until dismissed.
alert(msg) //one button
confirm(msg) // 2 buttons t/f
prompt(msg, def_value)  //null or val

//Writing log at different levels, colors and icons
console.debug(), console.error(), console.warn(), console.info()

console.log('Go to $o', 'goo.gl') //hyperlinking
console.log('%cBanana', ‘color:yellow;’)  //css styled output

//formated output
console.table(object or array)
console.dir(object)
console.keys(object) 
console.values(key_list)

//useful
console.assert(test_expr, "text_if_fail")
console.count('msg')  //logs msg: 1  msg: 2 ... count times code was visited
console.time(bob)     //create a new timer
console.timeEnd(bob)  //stop timer and logs it
console.timeStamp('label')  //log a event during recording
console.memory()    //return objects containing info on page memory use.

// log stack trace at that point
console.trace('msg') and console.exception('error_msg')

//create a group of console statements
console.group('label') or console.groupCollapsed('label') //initially collapsed
...
console.groupEnd()

console.profile('title for header') // Use JS profiler to print profile report
console.profileEnd()
```

## Canvas API

To create download for canvas as PNG
```js
$("a").$add("click", e => e.target.href = canvas.toDataURL());
```

