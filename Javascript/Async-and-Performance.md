---

---

Links
- [You-dont-know-JS (Generators)](https://github.com/getify/You-Dont-Know-JS/blob/1st-ed/async%20%26%20performance/ch4.md)

## Concepts

Core JS is **synchronous**, code is executed line by line and there's no 'wait' feature. 

**Async !== Parellism** :- happens later vs. happening simultaneously. EL doesn't support parallelism ; webworkers do.

**Concurrency** :- when 2 or more "processes" happen in same window of time (rapid context switches) which gives illusion of parallelism. Event loop supports concurrency.

**Run to completion** :- JS code is broken up into two or more chunks (e.g `functions`) , where the first chunk runs completely _now_ and the next chunk runs _later_, in response to an *event*. <u>Macro-tasks peform modification on top of previous state, micro-tasks run within same state</u>

##### Interactive concurrency

```jsx
ajax( URL, foo );  
ajax( URL2, bar ); 

//we don't know foo, bar execution order but if they don't share variables or affect each other, it's FINE !!!

// but if they do ; we then must avoid unpredictable output

// CHECK the order
function func(data) {
	if(data.url  == URL) res[0] = data; 
	if(data.url  == URL2) res[1] = data; 
}

// GATE -> call method only when all values are available
let index=0;
function func(data) { res[index++] = data }
if (data1 && data2 ) {
	func(data1)
	func(data2)
}

// LATCH -> first to execute wins. To avoid multiple callback invokes by 3rd party utility
let done = false;
paytm.sendMoney(amount, cb)
jio.sendMoney(amount, cb)

function cb() {
	done = true; debitMoney(); 
}
```

##### Cooperative Concurrency

```jsx
//split heavy tasks into CHUNKS 
let i = 0;
void function count() {
	do { i++; } while (i % 1e3 ! =  0)
	if( i < 1e7) 
		setTimeout(count); //0 delay trick ; at end of task queue
		//or queueMicrotask(count) ; end of mt-queue
}()

//use zero delay to postpone a func until event fully propagates
```


## Callbacks

Callbacks are *fundamental* async pattern in JS. It is a function that event loop "calls back" into stack at a **later** time. This pattern has many flaws.

- **Inversion of Control** : they implicitly give control over to another party to invoke the _continuation_ of your program.
- **Non-linearity** : callbacks express async flow in a non-sequential way, which makes code much harder to understand and maintain. 
- **Pyramid of Doom** : deeply nested timers to perform a series of async operations. 

All these result in **Callback Hell** where code is unmaintable, unpredictable, full of latent bugs and prone to edge-cases. To avoid them, we now have 

**Promises** , that use *split-callback* design, and solve 1,3 and imperfectly 2.

**Generators** let you 'pause' individual functions without pausing the state of the whole program. They don’t follow run to completion.

`async/await` wrap generators and promises in a higher level syntax.


## Promises

Intro
  - it's an object that **encapsulates** a future value ; waits until the value is settled, then executes 1 _callback handler_ asynchronously using microtask queue.
  -  `[[PromiseResults]]` 

### Creating Promises

Legacy way 1 (`constructor()`)

```js
const P = new Promise((resolveCB, rejectCB) => {
  if (something) resolveCB(x);
    else rejectCB(y); 
  return xyz ; //ignored
}); 

/* x = non-promise ; P fulfilled/rejected with value x 
  if x is promise or error occurs, behave like then() */
```

Legacy way 2 (`thenables` )

```jsx
const obj = {
  then(onFulfilled, onRejected) { /*code*/ }
}
```

Modern way

`Promise.resolve(val)` :- flattens nested promises into 1 promise. If val is 
  - promise : returns that promise (new === original, unlike then, etc.)
  - thenable : call it's then()
  - otherwise : promise *fulfilled* with val

`Promise.reject(reason)` :- returns a _new_ Promise that wraps reason. Reason can be promise, primitive, anything else.. , preferably use `Error`.


Both can be called on non-promises with mock interface.

```js
class NotPromise {
  constructor(executor) {
    executor(
      (val) => log('hi',val), /* resolve's   */
      (reason) => log('bye', reason), /* reject's */
    );
  }
}
Promise.resolve.call(NotPromise, "foo"); // hi foo
Promise.reject.call(NotPromise, "bar"); // bye bar
```

### Consuming Promises

**Attach handlers** 

- `obj.then(F,R)` : executes F or R async-ly based on object's state. Return a *promise P*  based on callback's return
  - non-promise --> P fulfilled immediately with value = return or undefined.
  - promise --> P resolve to it (hard-linked to its eventual state & value)
  - error --> P rejected with throw *val* = `reason`

- `obj.catch(R)` : shorthand for `then(null,R)`. Used to specify common error-handler for a then chain.

- `obj.finally(cb)` : calls cb with param `undefined` and returns a promise

```jsx
cb = (val) => /* code */ 

//then is chainable
func().then(a,b).then(c,d)
/* or */ pro1 = func().then(a,b) ;  pro2 = func().then(c,d);

// sequential async ops
func().then(a).then(b).catch(g).finally(x)

// parallel async ops
Promise.allSettled( [f(), g(), h()] ).then(doIt) //f,g,h return promises
```

__Floating promise__ :- previous then handler started a promise but did not return it, there's no way to track its settlement anymore. Next `then` handler will be called *early* with `undefined` value.


**Error-handling**

```jsx
// DELEGATION - inner catch must re-throw to outer
func().then(a).catch(inner).finally(x).catch(outer)

// PRECISION
f().then(x => g(x).then(h).catch(err1)) //handles g,h errors
	 .then(z).catch(err2) //handles f,z errors
```

**Promise Bubbling**

When promise is rejected, 2 events fire on `window`
 - `rejectionhandled` : rejection was handled with a `.then()` or `.catch()` on promise
 - `unhandledrejection` : rejection was not handled by current macrotask & its microtasks.  It "bubbles" to top of call stack and host throws an `error` 

These events have props : `reason` (of rejection) and `promise` (the original). To catch bubbling,

```jsx
//in browser
window.addEventListner('unhandledrejection' , dealwithit);

//in nodejs, notice camel
process.on("unhandledRejection", (reason, promise) => { /code/ });
```

**Promise Concurrency**

4 static methods are available to run promises or operation in parallel. They take iterable of promises & return *1 promise* that will resolve based on how those promises are settled.

```js
//general syntax
Promise.all(arr).then(f).catch(g)

// Promise.all -- all OR 1st reject
f( [val_p, val_q, val_r] ) ; g(reason)

//Promise.any -- 1st ful OR all reject
f(value) ; g(error_obj) ; error_obj.errors /* array of reasons*/

//Promise.race -- as soon as 1 settles
f(value) ; g(reason) 

//Promise.allSettled -- always fulfill
f( [data_p, data_q, data_r] ) 
data_p.status //'fulfilled' or 'rejected'
data_p.value  // of promise
data_p.reason // of rejection if any

// if iterable is empty
`all() & allSettled()` => immediately fulfillled
`any()`  => immediately rejected
`race()` => forever pending
```

**Helper methods**

```jsx
// compose async ops
const chainOps = (...fs) => (initPromise) => 
  fs.reduce((pr, f) => pr.then(f) , initPromise)

/* using */ chainOps(...funcs)(pr)

// can be done with await
let acc = pr;
for (const f of [...fs]) acc = await f(acc)
```

**Converting callback based API to promise**

```jsx
//setTimeout
const wait = (ms) => new Promise(f => setTimeout(f, ms))
wait(1000).then(dothis)

//XMLHttpRequest
const myFetch = (url) => new Promise( (resolve, reject) => {
  const xhr = new XMLHttpRequest();
  xhr.open('GET', url);
  xhr.onload = () => resolve(xhr.responseText);
  xhr.onerror = () => reject(xhr.statusText);
  xhr.send();
})
myFetch(url).then(dothis)
```


## Async Await

