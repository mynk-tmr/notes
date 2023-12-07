---

---

Links
- [You-dont-know-JS (Generators)](https://github.com/getify/You-Dont-Know-JS/blob/1st-ed/async%20%26%20performance/ch4.md)

## Concepts

Core JS is **synchronous**, code is executed line by line and there's no 'wait' feature. 

**Async != Parellism** :- happens later vs. happening simultaneously. EL doesn't support parallelism ; webworkers do.

**Concurrency** :- when 2 or more "processes" happen in same window of time (rapid context switches) which gives illusion of parallelism. Event loop supports concurrency.

**Run to completion** :- JS code is broken up into two or more chunks (e.g `functions`) , where the first chunk runs completely _now_ and the next chunk runs _later_, in response to an *event*. <u>Macro-tasks peform modification on top of previous state, micro-tasks run within same state</u>

#### Interactive concurrency

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
paytm.sendMoney(amount, cb)
jio.sendMoney(amount, cb)
let done = false;
function cb() {
	if(!done) debitMoney(); 
	done = true;
}
```

#### Cooperative Concurrency

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

- **Inversion of Control** : they handover the control & flow of code to other api (usually 3rd party) with minimal ways to tackle unpredicted behaviour
- **Non-linearity** : callbacks express async flow in a non-sequential way, which makes code much harder to understand and maintain. 
- **Pyramid of Doom** : deeply nested timers to perform a series of async operations. 
- **Race conditions** : when some async task may finish synchronously

All these result in **Callback Hell** where code is unmaintable, unpredictable, full of latent bugs and prone to edge-cases. To solve, we have
- **Promises** , that use *split-callback* design
- **Generators** let you 'pause' individual functions without pausing the state of the whole program. They don’t follow run to completion.
- `async/await` wrap generators and promises in a higher level syntax.


## Promises

An object that **encapsulates** a future value ; waits until the value is settled, then executes 1 _callback handler_ asynchronously using **microtask** queue.

Solutions to callback hell
 - **IoC** :- we don't pass callback into 3rd party API, instead request a promise. To ensure we only get promise, use `Promise.resolve(api_call)`
 - **PoD** :- no deep nesting 
 - **races** :- always run on mtqueue, even when errors


Internals :- `[[PromiseResults]]` 

#### Creating Promises

```jsx
// constructor
P = new Promise( (resolve, reject) => { 
	if(A) resolve(value); 
	if(B) reject(reason); 
	return 3; //ignored, just for control flow
	// if error, rejected with reason = err
})

// thenables (mock interface)
const obj = {
  then(onFulfilled, onRejected) { /*code*/ }
}

// modern syntax --> don't require EXECUTOR to be called
Promise.resolve(value)
Promise.reject(reason)

```

*A promise is resolved/rejected once and fixed forerver* 
```jsx
const r = (x) => x*4  // no effect
const P = new Promise((f,r) => {
    r(3);
    r(9); f(12) //no effect
})
P.catch(console.log) //3 
```


`resolve() v/s Promise.resolve()` : if value is 
- non-promise :- create a promise *fulfilled* with value;
- thenable :- call *onFulfilled* recursively until a definite value is found
- promise :- 
	- `resolve()` => create **new** promise that will *resolve* to original (immutably linked to its eventual state & value) 
	- `Promise.resolve()` => return **original** promise (to avoid deeply nested promises)

`reject(), Promise.reject()` : if reason is
- thenable :- call *onRejected* (no unwrapping)
- others :- create a **new** rejected promise that wraps reason (no resolution)

```jsx
var a = Promise.resolve(1);
var b = Promise.resolve(a);
var c  = Promise.reject(a);
b.then(console.log)  //1 (==a)
c.then(console.log)  //Promise {1} 
```


Using modern syntax on promise-likes
```js
onF = (val) => console.log('fulfilled',val)
onR = (re) => console.log('rejected', re)

class Mock {
  constructor(executor) {
    executor(onF , onR)
  } 
}
Promise.resolve.call(Mock, "foo"); // fulfilled foo
Promise.reject.call(Mock, "bar"); // rejected bar
```

#### Consuming promises - then, catch, finally

- Done by attaching callback handlers that execute when promise is settled.
- these methods call internal `then(onF,onR)` of given object.
	- a thenable might call both `onF,onR` , hence we use `Promise.resolve(p)` to ensure only 1 is called.
- they return **new** promise *fulfilled* with `resolve(cb_return_value)` OR *rejected* with reason = `throw value` 
- Usage
	- `obj.then(onF, onR)` 
	- `obj.catch(onR)` : shorthand for `then(null,onR)`. 
		- specifies common error-handler for *preceding* then chain.
	- `obj.finally(cb)` : to process something once promise is settled. Like `then(cb,cb)` but
		- cb takes NO parameter and it's return is ignored
		- finally returns a promise *fulfilled* with `resolve(obj)` OR *rejected* with reason = `throw value` 


```jsx
//then is chainable
func().then(a,b).then(c,d)
/* or */ pro1 = func().then(a,b) ;  pro2 = func().then(c,d);

// sequential async ops
func().then(a).then(b).catch(g).finally(x)

// parallel async ops
Promise.allSettled( [f(), g(), h()] ).then(doIt) //f,g,h return promises
```

__Floating promise__ :- previous then handler started a promise but did not return it, there's no way to track its settlement anymore. Next `then` handler will be called *early* with `undefined` value.


#### Error-handling

When a promise is rejected, 2 events can fire on `window` 
- each has `event.promise` and `event.reason` (the promise & its reason)
- these are
	- `rejectionhandled` : rejection was handled with a `.then()` or `.catch()` on promise. Used to see **intermediate errors** in a promise chain. !!! really useful.
	- `unhandledrejection` : rejection was not handled by current macrotask & its microtasks. It **"bubbles"** to top of call stack and host throws an `error` 

try-catch-finally blocks are synchronous (happen NOW), so can't work on Promise.

```jsx
// DELEGATION - inner catch must return reject OR re-throw
func().then(a).catch(inner).finally(x).catch(outer)

// PRECISION
f().then(x => g(x).then(h).catch(err1)) //handles g,h errors
	 .then(z).catch(err2) //handles f,z errors
```

```jsx
//in browser
window.addEventListner('unhandledrejection' , dealwithit);

//in nodejs, notice camel
process.on("unhandledRejection", (reason, promise) => { /code/ });
```

#### Promise Concurrency

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

#### Helper Methods

```jsx
// compose async ops
const chainOps = (...fs) => (initPromise) => 
  fs.reduce((pr, f) => pr.then(f) , initPromise)

/* using */ chainOps(...funcs)(pr)

// can be done with await
let acc = pr;
for (const f of [...fs]) acc = await f(acc)

// a utility to set time-limit for functions
function within(delay) {
	return new Promise( (f, reject) =>
		setTimeout( () => reject(), delay ));
}
/*using*/ Promise.race( [ foo(), within(2000)] ).then(F,R)
```

#### Converting callback based API to promise

```jsx
//setTimeout
const wait = (ms) => new Promise(resCb => setTimeout(resCb, ms))
wait(1000).then(dothis)
wait(1000).then( () => {
	doA() ; return wait(1500)//for chaining delay
	}) 

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

Drawbacks of Promises
- single value or reason => needs array/object wrapping to send >1
- only way to pinpoint errors is 1 eventListener on window
- promisifying api takes effort => use a library
- 