```jsx
console.log(a); //error
function foo() { console.log(a,c) } // 'a' undefined {hoisted}
let a = "a"; 
foo(); 
var c = "c";
```

```jsx
let promise = Promise.reject(new Error("Promise Failed!"));
setTimeout(() => promise.catch(err => alert('caught')), 1000);
window.addEventListener('unhandledrejection', event => alert(event.reason));

//Promise failed -> 1s -> caught
//microqueue empty but rejection wasn't handled
```
