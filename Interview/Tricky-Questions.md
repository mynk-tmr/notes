
```jsx
console.log(a); //error
function foo() { console.log(a,c) } // 'a' undefined {hoisted}
let a = "a"; 
foo(); 
var c = "c";
```


## Promises


```jsx
var a = Promise.resolve(1);
var b = Promise.resolve(a);
var c  = Promise.reject(a);

b.then(console.log)  //1
c.catch(console.log)  //Promise {1} ; no unwrapping
```

```jsx
// 1 step in resolution require 1 tick

var a = Promise.resolve('a') // a = 'a' after 1 tick
var a2 = new Promise( rs => rs(a)); //a2 = 'a' after 2 ticks 
var a3 = Promise.resolve(a) //a3 = a, so 'a' after 1 tick

a2.then(() => alert('a2')) //after 2 ticks, log 'a2'
a3.then(() => alert('a3')) // after 1 tick, log 'a3'
alert('hello') // log 'hello'

//output --> hello a3 a2
```

```jsx
let p = Promise.reject('why');
setTimeout(
	() => promise.catch(() => alert('caught')), //new task
	1000);
window.addEventListener('unhandledrejection', event => alert(event.reason));

//why -> 1s -> caught
```
