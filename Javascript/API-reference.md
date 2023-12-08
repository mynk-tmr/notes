
## Concepts

[same-origin policy](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy) means that a script can only request resources from the same origin it was loaded from (ie. server). To bypass this, we enable **CORS** 

**Cross-Origin Resource Sharing** ([CORS](https://developer.mozilla.org/en-US/docs/Glossary/CORS)) is an [HTTP](https://developer.mozilla.org/en-US/docs/Glossary/HTTP)-header based mechanism that allows a *server* to indicate any *origins* other than its own from which a browser permits loading resources

## JSON

JSON is a **text**-based data format following JavaScript object syntax.

JSON exists as a **string**. Converting a string to a native object is called _deserialization_, while converting a native object to a string for network transmission is called _serialization_.

JSON can be a single string, number etc. JSON keys requires double-quotes.

```jsx
obj = JSON.parse('{"name":"John", "age":30}'); 
arr = JSON.parse('["name":"John", "age":30]'); //arr[0] 
JSON.parse(data, (key,val) => {} ) // transform values [must return]

str = JSON.stringify(obj) //or array
localStorage.setItem("cook", str); //store
localStorage.getItem("cook");

//date fields
JSON.stringify({dt : Date.now()}) // auto-evaluated
parsedObj.age = new Date(parsedObj.age);
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
readAsArrayBuffer()	//ArrayBuffer containing binary data.
readAsBinaryString() //string.
readAsDataURL()	//URL string 
readAsText() //contents

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
  .then(response => response.json()) //get a promise for data 
  .then(data => console.log(data)) //once available, use

//to post must include this
 headers: { "Content-Type": "application/json"}
```

**Options object**
- `mode` : origin policy -> `cors`, `no-cors`, or `same-origin`
- `referrerPolicy` : 
- `method` : (GET, POST, PUT, DELETE, etc).
- `cache` : use from cache? `no-cache` `force-cache` 
- `body: JSON.stringify(data)` : to POST data
- `headers: JSON.stringify(heads)` : set HTTP headers
- `credentials` : `omit` `same-origin**` `include` ; how to send/get credentials
- `signal : ctr.signal` : signal whose .abort() will abort fetch.

**Response properties**
- `res.ok` (t/f) `res.status` (404) 

Fetch library [axios](https://axios-http.com/) 