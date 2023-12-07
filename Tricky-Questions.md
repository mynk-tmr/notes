```jsx
console.log(a); //error
function foo() { console.log(a,c) } // 'a' undefined {hoisted}
let a = "a"; 
foo(); 
var c = "c";
```

```jsx
let promise = Promise.reject("Promise Failed!");
setTimeout(
	() => promise.catch(() => alert('caught')), //new task
	1000);
window.addEventListener('unhandledrejection', event => alert(event.reason));
// bubbled, fire event

//Promise failed -> 1s -> caught
```

Never rely on ordering of callbacks across Promises
```jsx
var p3 = new Promise( (rs) => rs("B") );
var p1 = new Promise( (rs) => rs(p3)  ); //resolve to p3 ASYNCLY 
var p2 = new Promise( (rs) => rs("A") );
p1.then( v => log(v) ) 
p2.then( v => log(v) )

// A B  <-- not  B A  as you might expect
```

