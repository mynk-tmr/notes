**Throttling** and **debouncing** are techniques used to **rate-limit** a function call, particularly in cases where frequent events can lead to excessive calls and poor performance.

**Debouncing**
* ensures function is called only after a specific **delay** since event's last occurence.
* Implement: **track delay** between **2 events** and run function if time interval exceeds
* Use cases: where action has to be taken after some period of **inactivity** e.g. search, autocomplete
* Cons: if function takes too long, it may call API repeatedely
* Calculation 
	* if `D = debounce rate`, `N = no. of events`, `P = time period`
	* event frequency `F = P / N` and no. of calls = `ceil(F/D)`

**Throttling**
* ensure function is called at a **steady** rate, regardless of event frequency
* Implement: use timestamp to **track last call** and run function if time interval has exceeded
* Use cases: where events must be handled at **regular intervals**.  e.g. scroll, resize, infinte scrolling, game apps
* Cons: if function takes too long, it may break
* Calculation 
	* if `T = throttle rate`, ideal no. of calls is `L = ceil(P/T)`, but actual no. is `min(N, L) `

```js
function debounce(func, wait) {
  let timer;
  return function(...args) {
      clearTimeout(timer);
      timer = setTimeout(() => func.apply(this, args), wait)
  }
}

function throttle(func, delay) {
  let lastCall = 0;
  return function (...args) {
    const now = Date.now();
    if (now - lastCall >= delay) {
      func(...args);
      lastCall = now;
    }
  };
}
```

**AJAX (Async JS & XML)** : a technique in which a web app fetches content from server by making async HTTP requests, and uses new content to update parts of page.

**Fetch API** 
* a promise based API to send or retrieve network resources.
* Features
	* supports many data formats -> JSON, XML. 
	* has support for CORS, service workers, caching.
* Basic fetch request : `fetch(request) OR fetch(url, options)`. Options is used to configure request

```js
fetch('https://api.example.com/data', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify(data)
})
```

**Request / Response Objects**
* API takes a Request Object : `new Request(url, options)` and returns a Response object when a request is resolve
* Both objects have properties (config fields) methods to extract body (returns **promise**)
	* `.json()` : parses to JSON
	* `.blob()` `.formData()` `.text()`
* fetch rejects only when a network error or CORS is misconfigured. To see error, use `res.ok` `res.status` (http code) and `res.statusText`

**Headers Object**
* allows you to set HTTP headers using a **map-like** interface. But some headers can't be set due to in-built guards
* To set custom headers, use `.append(x, val)` method, or include them in fetch options.

**How to handle CORS ?**
* `mode: 'cors'` option tells the browser to send request with CORS. It will include the `Origin` header with the request.
* CORS allows client-side script to communicate with a server *different* from one that loaded it. 
- Client sends a cors request to cross server  : `OPTIONS` verb with `Origin` header. The server responds with a header `Access-Control-Allow-Origin : <domains>` specifying if origin is allowed
- CORS policy is handled by server.

**How to cancel Request ?**
* using `AbortController` object. When it's `.abort()` is called, it generates `abort` event on `.signal` property
* use `signal` option in fetch config to listen to event.

```js
async function fetchwithAbort(url, options, delay) {
	let ctr = new AbortController()
	setTimeout(() => ctr.abort(), delay);
	return await fetch(url, {...options, signal : ctr.signal})
}
```

**How to handle concurrent requests** : Using static Promise methods
```js
const allResponses = Promise.all(res1, res2, res3)
```


**Files API** : lets us read and display user-selected files.
* read as `element.files` or `e.dataTransfer.files` => returns array of Blobs
* `files` props : `name` `size` `type`
* Blob interface represents a file-like object of immutable, raw data; they can be read as text or binary data
* **FileReader** : an object that can read contents of a file asyncly. `new FileReader()`
	* Properties : `error` `readyStatus` (0 1 2) `result`
	* Events that fire on it: `loadstart` `progress` `load` `error`
	* Methods: `readAsDataURL(Blob)` 

**Show user-sslected image**
```ts
inputBtn.onchange = (e) => {
	let file = e.files?.[0];
	if(!file) return
	let url = URL.createObjectURL(file)
	img.src = url
	img.onload = () => URL.revokeObjectURL(url) 
}
```


```js
let jsWindow = window.open(url,'Google')
jsWindow.close()
window.resizeTo(600,600)   // width, height
window.resizeBy(-100,-100) // relative W, H
```

**`window.location`** : used to access and change window url.
* methods : `assign(url)` , `replace(url)` `reload(true)` => GOTO url ; reload from cache/server (F/T)
* props : `href` `pathname` `search` `hash`  (url string ; host/*,  ?, #)

```jsx
const urlParams = new URLSearchParams(location.search); //map like object
for ([key, value] of urlParams) 
urlParams.has('brand')
```

**`window.navigator`**: represents state and identity of user agent. "browser sniffing" and “fingerprinting” properties often return fake values.
```jsx
userAgentData.brands[0]  // {brand: 'Google Chrome', version: '119'}
userAgentData.mobile  //false
userAgentData.platform // 'Linux'
connection.downlink  // 10   /mbps/
deviceMemory  //8
languages  // ['en-IN', 'en']
language  // 'en-IN'
```

To send analytics to server
```jsx
navigator.sendBeacon(url, data); //data can be any type (Blob, formData etc)
```

**History API** : 
* provides a way to manipulate browser history to create **SPAs**
* **history stack** stores the browser’s session history (in tab or frame). To manipulate it, use  `window.history` object. 
* These methods follow **same-origin** url policy.
* `window.onpopstatechange` : fires upon navigation; has `state` prop for a copy of pushed/replaced state object

```jsx
//control navigation
history.back()  //visit previous page
history.forward() //visit next page
history.go(-2) // go to 2nd last page.
history.go() // reload current page
history.length // no. of pages in history stack
```

```js
//manipulate history [state object, title, append to URL]
history.pushState({page: 1}, "Introduction", "?page=1")
history.replaceState(...)
window.onpopstate = () => setTimeout(doSomeThing, 0);
```


**Cookie** 
* a small piece of **data** exchanged between server and browser to create **statefulness** in requests and responses.
* Main uses — session management, personalisation, tracking user.
* Cookie types — **permanent** (have expiry date) and **session** **(no expiry; lasts 1 session)
* `document.cookie` — accessor property to CRUD cookies.

```jsx
//can only set 1 cookie at time; sets name & id
document.cookie = "name=John; max-age=100;"
document.cookie = "id=3233; max-age=100;"

//decode URI (%20 -> ' ')
let cookies = decodeURIComponent(document.cookie)

// format cookies
cookies.split(';').map(coo => coo.trim().split('=') )
```

**Cookies**
```js
`expires=${dateobj.toUTCString()}` //default session
`max-age=100` //seconds after session end; if set to -ve, will expire
`domain=example.com` //domain to which cookie will be sent (same-origin)
`path=/path/to/host` //on domain
```

**Local storage vs. Cookies**
- Cookies are for client-server, Local storage are for client only
- Cookie has a size limit of 4 Kb. Local Storage is 5 MB for site.
- Cookie has a expiration date.

**Web storage objects**
* `localStorage` and `sessionStorage` allow to save key/value pairs in the browser.
- `setItem(key, value)` – store key/value pair.
- `getItem(key)` – get the value by key.
- `removeItem(key)` – remove the key with its value.
- `clear()` – delete everything.
- `length` – the number of stored items.

Storage objects are not iterable. But
```js
keys = Object.keys(localStorage)
for(key in keys) localStorage.getItem(key)
```

Features of `localStorage` are:
- Shared between all tabs and windows from the **same** origin.
- does not expire (persist on drive)
- use **JSON** format

`sessionStorage` exists only within the current browser tab.
- shared between iframes in the same tab
- data survives page refresh, but not closing/opening the tab.

When the data gets updated, `window.onstorage` is fired
- `key` – the key that was changed (`null` if `.clear()` is called).
- `oldValue` – the old value (`null` if the key is newly added).
- `newValue` – the new value (`null` if the key is removed)
- Event triggers on all `window` objects where the storage is accessible, except the one that caused it
- That allows different windows from the same origin to exchange messages.

**Synchronous** modals - pause code execution until dismissed.
```js
alert(msg) confirm(msg) // 2 buttons t/f
prompt(msg, def_value)  //null or val
```

**Console API**
```js
//Writing log at different levels, colors and icons
console.debug(), console.error(), console.warn(), console.info()

//debugging
console.keys() console.values()
console.assert(test_expr, "text_if_fail")
console.count('msg')  //logs msg: 1  msg: 2 ... count times code was visited
console.trace('msg') //stack trace
```

**Working of browser**
- Web browsers perform two main tasks: **parsing** and **rendering**.
- During **parsing**, it converts bytecode into **nodes**, and organise them in tree-like data structure DOM
- After making DOM, it **renders** DOM, ie. each **presentational node** is displayed on screen

**Rendering Waterfall**
- **style calculation**: CSS rules are applied to each jelement node to form CSSOM.
- **layout creation**: calculate position and geometry of elements (vector boxes) 
- **paint**: vector boxes are *rasterized* (to pixels) and put onto *layers* . This is done to improve repaints of elements likely to change by isolating changes to specific layer only
- **composition** : layers are placed in correct order. By default, 1 layer exists. 

**3 types of UI change**
- `Reflow` : changes in properties that trigger *layout* re-creation. **Expensive** process. 
- `Repaint` : no change in layout, but pixels are updated 
- `Recompose` : seperate layer is changed. e.g. `opacity`, `transform`. Cheapest, uses GPU and run in seperate thread from main

**Layer creations happen for**
- container whose child has layer ; new stacking contexts
- `3D` stuff ; `<canvas>` with 3D (WebGL)
- `<video>` with accelerated video decoding
- accelerated CSS `filters`

**Optimise Animations**
- Animate, Transition properties that <u>don't reflow</u> page to improve performance.
- place *reflow-repaint* properties on *separate* layer using for e.g. `will-change: color` 
- More layers -> more memory (so careful)

**Script Loading Optimisation**
* used to embed a client-side script. `<script src='path' type='text/javascript' />`
* parsing is blocked by loading & execution of script
* Loading strategy (boolean attributes)
	* **`async` :** block & execute only after complete download, and before window's `onload` event. Use for critical ones like analytics
	* **`defer`**:  execute after page parsing, before window's `DOMcontentLoaded`. Less critical
* How to load 3rd party scripts?
	- via main script : `import('path').then(analytics => analytics.init())` 
	- for 3rd party scripts, `preconnect` & `dns-prefetch` can be used
	- lazy loading with `Intersection Observer`
	- using CDNs
	- cache script with webworker

**Library vs. Framework** 
* libraries are files containing **custom** functions that you can use. e.g React.
* frameworks are packages of HTML, CSS, JavaScript, and other technologies that you use to write an entire web application from scratch
* A developer calls a method from a library. A framework calls the developer's code.

**MVC** : an architectural pattern which aims to separate app into 3 main logical components
- **Model** manages data and business logic. When data state changes, it notifies *View* to update UI or *Controller* if update needs extra logic
- **View** handles display of data to user (templating / rendering)
- **Controller** routes commands to model and view when requested by client

**Working of web server**
- A server using HTTP is called **web server.**
- Server runs a **web application** that contains logic about how to respond to requests based on `routes` (`http-verb+uri`). Each `route` can have one or many request `handler` functions.

**Single Page App = CSR + AJAX**
- A single webpage sent by server that makes incremental updates to itself over user session
- Doesn't fetch new pages from server and avoid costly repaints
-  **CSR** : Manipulate history stack without making a document request to the server.

**Progressive web Apps**
- web apps built and improved with modern APIs to enhance UX.
- Benefit : with a single codebase, we can utilise broad reach of web apps and rich capabilities of platform-specific apps

**Server Components** 
- allow you to write UI that can be rendered and cached on server.
- 3 server rendering strategies: **static**, **dynamic**, **streaming**
- Benefits
	- Data Fetching in secure & low latency manner
	- performance : moving non-interactive UI to server, means smaller bundle is sent to client
	- First Contentful Paint (FCP) is fast, SEO is better

**API** : set of rules and interfaces for communication between softwares
**SDK**: A comprehensive suite of tools and resources for developing software, often including APIs.

**HTTP headers** allow passing additional info in each http message.

**HTTP Verbs/Methods**
- Specifies what action has to be followed to get the requested resource. 
- Methods
	- GET retrieve data.
	- HEAD like GET, but only ask for headers (no body)
	- POST create a resource on server
	- PUT replaces a resource with sent payload
	- PATCH applies partial modifications to a resource
	- DELETE resource
	- CONNECT for tunneling 
	- OPTIONS retrive cors-related configuration
	- TRACE performs a message loop-back test 
- **idempotent** : a method that can be called multiple times without changing result of resources
- **safe** : do not change any resources internally. Their result can be cached
- GET, TRACE, HEAD, OPTIONS are *safe and idempotent* 
- PUT and DELETE methods are only *idempotent*. 
- POST and PATCH methods are neither safe nor idempotent (resource changes)

**HTTP staus codes** : standard codes that refer to task status at server
- 1xx - informational responses
- 2xx - successful responses E.g `200` for GET/PUT, `201` POST, `204` DELETE
- 3xx - redirects. E.g. `301`  for GET/HEAD, `308` redirect POST etc, `304` sent cached data
- 4xx - client errors. E.g. `404` resource not found, `403` forbidden, `400` bad request,
- 5xx - server errors. E.g. `502` upstream server didn't send response

**5 Core components of HTTP Request**
- Method/Verb
- URI
- HTTP Version
- Request Header − request metadata & client info
- Request Body − actual message content

**4 core components of HTTP Response**
- status code
- HTTP Version
- Response Header
- Response Body

**protocol** is a set of rules and standards for exchanging data.

**ip-address**: unique ID assigned to each device on a network. IPv4 is 32bit. IPv6 is 128bit. Machines can use both via dual addressing

**domain name**: human-readable alias for IP address. It consists of **labels** separated by dot (.)
**domain name system (DNS)**: A protocol used to **translate** domain names into IP addresses.

**port**: unique no. that identifies an app or service running on a device.
**socket** : ip + port number ; represents a specific endpoint for communication.

**package** : a directory with js modules or libraries. Must have `package.json` file. 
**package manager** : automates managing packages & dependencies. **npm** is default manager of nodejs.

**module bundler** : creates a single static file from multiple files. It builds a dependency graph from `entry` point(s) and then combine them into a single `output` file.

**Webpack**: a module bundler & compiler for JavaScript and other front end assets.

**transpiler** : converts code from other languages to ones browser understands. e.g. Babel

**task runner**: To automate different parts of the build process like testing, linting, compiling etc. Commonly **npm scripts** are used.

**version control system** :  software tool that help software teams manage changes to source code over time and revert back when required.

**Git** : a distributed version control tool. It records changes in a project as **snapshots** in a database called *repository*

**Test runners** : tools that execute a pre-defined test script and auto-generate results. e.g. Jest