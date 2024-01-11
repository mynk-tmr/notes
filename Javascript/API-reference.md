
## Concepts

[same-origin policy](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy) : a script can only request resources from the same origin it was loaded from (ie. server). To bypass this, we enable **CORS** 

**Cross-Origin Resource Sharing** ([CORS](https://developer.mozilla.org/en-US/docs/Glossary/CORS)) is an [HTTP](https://developer.mozilla.org/en-US/docs/Glossary/HTTP)-header based mechanism that allows a *server* to indicate other *origins* from which a browser permits loading resources

## JSON

JSON is a **text**-based data format following JavaScript object syntax. JSON exists as a **string**. 

2 methods
- `JSON.stringify` to convert object/array into JSON (_serialization_)
- `JSON.parse` to convert JSON back into object/array (_deserialization_)

JSON can be a single primitive or array. JSON keys requires double-quotes. Skips keys for `function, symbol, undefined`

```jsx
JSON.parse('{"name":"John"}'); 
arr = JSON.parse('["name":"John"]'); //arr[0] 
JSON.parse(data, (key,val) => {
	//code
	return newValue; // transform values including nested
} ) 

localStorage.setItem("cook", json); //store
localStorage.getItem("cook");

//date fields
JSON.stringify({dt : date_obj}) // auto-evaluated
parsedObj.age = new Date(parsedObj.age)
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


## Files API 


```js
var g = div.file = inputFilebtn.files[0];  // obj exposed
g.name, g.type //file name, file mime string
if(g.size > 75 * 1024)  //check if size > 75Kb

//custom file btn ; hide inp
cstm_btn.onclick = inp.onclick();

//using drag & drop
function drop(e) {
  e.stopPropagation(); e.preventDefault();
  dealwithit(e.dataTransfer.files);
}

// upload & play video
inp.onchange = () => $('video').src = URL.createObjectURL(inp.files[0])
$('video').onload = () => URL.revokeObjectURL(video.src) //memory management

```

`FileReader` object lets webpage asynchronously read contents of files selected by user

```js
//thumbnail preview of selected file
const reader = new FileReader();
reader.readAsDataURL(files[0]);   //start async reading
reader.onload = (e) => img.src = reader.result

//readonly props 
x.error  // error_obj
x.readyState // 0 1 2 ['', loading, loaded]
x.result //loaded file

//value of result depends on read function used
.readAsArrayBuffer()	// binary data.
.readAsBinaryString() //string.
.readAsDataURL()	//URL string 
.readAsText() //contents

//events on filereader
abort, error, loadstart, progress, load /*only if success*/ , loadend
```

## Fetch API

A *promise* based API to send/get network resources using http headers

```jsx
const response = fetch(url, options);

//using constructor to create Request, Headers object
const heads = new Headers({ "Content-Type": "application/json"})
const request = new Request(url, heads) //request.url ; request["Cont..]
fetch(request)
```

**Options object**
- `body: JSON.stringify(data)` : to POST data
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
	- `.append(key,value)`, `.delete(key)`, `.forEach(cb)` , `.get(key)` `.set(key,value)` , `.has(key)`
	- `.getSetCookie()` : only on nodejs
	- `.entries()`, `.keys()`, `.values()`

**Response objects**
- returned when fetch promises are resolved
- `res.ok` (t/f) `res.status` (404) `res.statusText` 
- fetch rejects only when a network error or CORS is misconfigured

```jsx
//methods on Request and Response to extract body [return Promise]
// can only run once
.arrayBuffer() .blob() .formData() .json() .text()
```

Fetch library [axios](https://axios-http.com/) 

**Uploading Form data**  `body : formData` 

```jsx
const formData = new FormData();
const fileField = $('input[type="file"]');

formData.append("username", "abc123");
formData.append("avatar", fileField.files[0]);

//for mutiple files
for (const [i, photo] of [...fileField.files].entries()) {
  formData.append(`photos_${i}`, photo);
}

new FormData(form, submitter) //creates formData from form fields 'name':'value' and submit button's 'name':'value'
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
search //search string [?...]
hash   // #title

assign(url) //navigate to URL, push in history stack, no reload for hashchange
location.href=url ; //same as assign
replace(url)  //navigate but no push in stack
reload(/*true*/)   //reload from cache OR from server
```

```jsx
const urlParams = new URLSearchParams(location.search); //iterable
for ([key, value] of urlParams) {  // [props are delimited by &]
}
//keys(), values(), entries()
urlParams.has('brand')  //true  ; parameter check
```

#### window.navigator

represents state and identity of user agent. "browser sniffing" and “fingerprinting” properties often return fake values.
```jsx
userAgentData.brands[0]  // {brand: 'Google Chrome', version: '119'}
userAgentData.mobile  //false
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

HTTP headers
- server (multiple `Set-cookie`) and browser (single semi-colon joined `Cookie` )
- `Set-Cookie: id=a3fWa; Secure; HttpOnly` ⇒ send only over https and make invisible to javascript

Cookie types — **permanent** (have expiry date) and **session** **(no expiry; lasts 1 session)

#### Client-side cookie manipulation

`document.cookie` — accessor property to create, read, and delete cookies.

```jsx
//can only set 1 cookie at time; sets name & id
document.cookie = "name=John; max-age=100;"
document.cookie = "id=3233; max-age=100;"

//name=John; id=3233;
console.log(document.cookie) 

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
`max-age=100` //seconds after session end; set to -ve expire
`domain=example.com` //domain to which cookie will be sent (same-origin)
`path=/path/to/host` //on domain
```


## LocalStorage and IndexDB

**Local storage vs. Cookies**
- Cookies are for client-server, Local storage are for client only
- Cookies are sent in every HTTP headers and can be disabled
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