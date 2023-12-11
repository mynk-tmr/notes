
## Concepts

[same-origin policy](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy) : a script can only request resources from the same origin it was loaded from (ie. server). To bypass this, we enable **CORS** 

**Cross-Origin Resource Sharing** ([CORS](https://developer.mozilla.org/en-US/docs/Glossary/CORS)) is an [HTTP](https://developer.mozilla.org/en-US/docs/Glossary/HTTP)-header based mechanism that allows a *server* to indicate other *origins* from which a browser permits loading resources

## JSON

JSON is a **text**-based data format following JavaScript object syntax. JSON exists as a **string**. 

2 methods
- `JSON.stringify` to convert object/array into JSON (_serialization_)
- `JSON.parse` to convert JSON back into object/array (_deserialization_)

JSON can be a single primitive or array. JSON keys requires double-quotes. Skips `function, symbol, undefined` keys

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

//custom
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
  - patternMismatch , stepMismatch
  - tooLong, tooShort, valueMissing, badInput (unreadable)
  - rangeOverflow, rangeUnderflow
  - typeMismatch (e.g. ❌ url)
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
//readonly props 
x.error  // error_obj
x.readyState // 0 1 2 ['', loading, loaded]
x.result //loaded file

//value of result depends on read function used
.readAsArrayBuffer()	//ArrayBuffer containing binary data.
.readAsBinaryString() //string.
.readAsDataURL()	//URL string 
.readAsText() //contents

//thumbnail preview of selected file
const reader = new FileReader();
reader.readAsDataURL(files[0]);   //start async reading
reader.onload = (e) => img.src = reader.result


//events on filereader
abort, error, loadstart, progress, load /*only if success*/ , loadend
```

## Fetch API

A *promise* based API to send/get network resources. 

```jsx
fetch(url, {mode: 'cors'}) //returns promise for response
  .then(response => response.json()) //extract body(data) 
  .then(data => console.log(data)) //once available, use

//to post must include this
 headers: { "Content-Type": "application/json"}

//using constructor to create Request, Headers object
request = new Request(url, {mode: 'cors'})  //fetch(request)
heads = new Headers({ "Content-Type": "application/json"}) // headers : heads
```

**Options object**
- `body: JSON.stringify(data)` : to POST data
- `headers: JSON.stringify(header)` : set HTTP headers
- `mode` : origin policy -> `cors`, `no-cors`, or `same-origin`
- `referrerPolicy` : 
- `method` : (GET, POST, PUT, DELETE, etc). Still sends response for succ/fail
- `cache` : use from cache? `no-cache` `force-cache` 
- `credentials` : send/get cookies ? -> `omit` `same-origin**` `include` 
- `signal : ctr.signal`: set abortcontroller


**Headers object**
- have a _guard_ to prevent malicious changes
- methods
	- `.append(key,value)`, `.delete(key)`, `.forEach(cb)` , `.get(key)` `.set(key,value)` , `.has(key)`
	- `.getSetCookie()` : only on nodejs
	- `.entries()`, `.keys()`, `.values()`


**Request Object**
- props same as in *options* object //read-only//
- + `.url` 


**Response objects**
- returned when fetch promises are resolved
- `res.ok` (t/f) `res.status` (404) `res.statusText` 
- fetch rejects only when a network error or CORS is misconfigured


```jsx
//methods on Request and Response to extract body [return Promise]
// can only run once
.arrayBuffer()
.blob()
.formData()
.json()
.text()

new Request(t, init) //creating customised copies ; Use this to get BODY again
```


Fetch library [axios](https://axios-http.com/) 

**Uploading Form data** 

`body : formData` 

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