## Promises

Intro
  - an object that represents eventual completion or failure of an async operation. 
  - has 3 states — *pending, fullfiled, rejected*
  - stores a value `[[PromiseResults]]` provided at time of settling it. 
  - when settled, executes 1 _callback handler_ asynchronously via microtask queue.


### Creating Promises

Legacy way 1 (`constructor()`)

```jsx
const P = new Promise((resolveCB, rejectCB) => {
  if (something) resolveCB(x);
    else rejectCB(y); 
  return xyz ; //ignored
}); 

/* x = non-promise ; P fulfilled/rejected with value x 
  if x is promise or error occurs, behave like then() */
```

Legacy way 2 (`thenables` ie. objects with then)

```jsx
const obj = {
  then(onFulfilled, onRejected) {
    //code 
  }
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


`obj.then(F,R)` : executes F or R async-ly based on object's state. Return a *promise P*  based on callback's return
  - non-promise --> P fulfilled immediately with value = return or undefined.
  - promise --> P resolve to it (hard-linked to that's eventual state & value)
  - error --> P rejected with value = given in throw and a `reason`

`obj.catch(R)` : shorthand for `then(null,R)`. Used to specify common error-handler for a then chain.

`obj.finally(cb)` : calls cb with param `undefined` and returns a promise, as soon as *promise obj* is settled

```jsx
//callback design, promise's value is passed to cb
cb = (val) => /* code */ 

//then is chainable
func().then(a,b).then(c,d)
/* or */ pro1 = func().then(a,b) ;  pro2 = func().then(c,d);

// sequential async ops
func().then(a).then(b).catch(g).finally(x)

// parallel async ops
Promise.allSettled(f(), g(), h()).then(doIt) //f,g,h return promises
```

__Floating promise__ :- previous then handler started a promise but did not return it, there's no way to track its settlement anymore. Next `then` handler will be called *early* with `undefined` value.


_Error-handling_

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
 - `unhandledrejection` : rejection was not handled by current macrotask & its microtasks.  

These events have props : `reason` (of rejection) and `promise` (the original). To catch bubbling,

```jsx
//in browser
window.addEventListner('unhandledrejection' , dealwithit);

//in nodejs, notice camel
process.on("unhandledRejection", (reason, promise) => { /code/ });
```

**Promise Concurrency**

4 static methods are available to run promises or operation in parallel. They take iterable of promises & return *1 promise* based on how those promises are settled.

```js
Promise.all([p,q,r]).then(f([ret_p, ret_q, ret_r])).catch(g(ret_rejected)) 

Promise.any([p,q,r]).then(f(ret_fulfilled)).catch(g(error_obj))
error_obj.errors /* array of  rejection reasons*/

Promise.race([p,q,r]).then(f(ret_fulfilled)).catch(g(ret_rejected)) /* as soon as 1 settles */

Promise.allSettled([p,q,r]).then([Tp, Tq, Tr]) /* never rejected */
Tp.status //'fulfilled' or 'rejected'
Tp.value  // of promise
Tp.reason // of rejection if any

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

