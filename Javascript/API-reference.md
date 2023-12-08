
## Concepts

[same-origin policy](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy) means that a script can only request resources from the same origin it was loaded from (ie. server). To bypass this, we enable **CORS** 

**Cross-Origin Resource Sharing** ([CORS](https://developer.mozilla.org/en-US/docs/Glossary/CORS)) is an [HTTP](https://developer.mozilla.org/en-US/docs/Glossary/HTTP)-header based mechanism that allows a *server* to indicate any *origins* other than its own from which a browser should permit loading resources

## Constraint Validation API

Setup
```jsx
myform.noValidate=true; //disable auto-validation
myform.addEventListener('submit', validateForm);

function validateForm(e) {
	e.preventDefault();
	if(e.target.checkValidity()) form.submit(); //no auto validation
	else {
		//
	}
}
```

An `invalid` event (bubble:*false*) is triggered on every invalid field. API supports --> **button, fieldset, input, output, select, textarea & form**

```js
//inline 'onchange' & submit 'onsubmit'

inp.onchange = (e) => {
  e.target.checkValidity() // true/false
  e.target.setCustomValidity('msg') //custom error msg, sets state to invalid
  e.target.reportValidity() // true/false + show error msg
}
```

`validationMessage` : error msg
`validity` : an object with bool properties
  - patternMismatch , stepMismatch
  - tooLong, tooShort, valueMissing, badInput (unreadable)
  - rangeOverflow, rangeUnderflow
  - typeMismatch (e.g. ❌ url)
  - valid 
  - willValidate (true if will be validated)

`pattern=` test with \g \m \i flags disabled


## Files API 


```js
var g = div.file = inp.files[0];  // obj exposed
g.name, g.type //string name, string mime
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

`FileReader` object lets webpage asynchronously read contents of files selected by user. Readonly props -> `error` (error obj), `readyState` (0=notloaded, 1=loading, 2=loaded) & `result` (loaded data)

```js
//thumbnail preview of selected file
const reader = new FileReader();
reader.readAsDataURL(files[0]);   //start async reading
reader.onload = (e) => img.src = reader.result

//value of result depends on read function used
readAsArrayBuffer()	//ArrayBuffer containing binary data.
readAsBinaryString() //string.
readAsDataURL()	//string 
readAsText() //contents


//events on filereader
abort, error, loadstart, progress, load /*only if success*/ , loadend
```
