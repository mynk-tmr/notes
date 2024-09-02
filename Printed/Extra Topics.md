**Throttling** and **debouncing** are techniques used to **rate-limit** a function call, particularly in cases where frequent events can lead to excessive calls and poor performance.

**Debouncing**
* ensures function is called only after a specific **delay** since event's last occurence.
* Implement: **track delay** between **2 events** and run function if time interval exceeds
* Use cases: where action has to be taken after some period of **inactivity** e.g. search, autocomplete
* Cons: if function takes too long, it may call API repeatedely
* Calculation 
	* if `D = debounce rate`, `N = no. of events`, `P = time period`
	* event frequency `E = P / N` and no. of calls = `ceil(E/D)`

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
- Place animating element in high `z-index` or it will trigger repaint of its layer sharers

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

