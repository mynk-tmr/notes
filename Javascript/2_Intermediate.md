#### Decorator Functions

take another function and adds “features” to it without affecting function’s body. We can combine multiple decorators

```jsx
//simple cache decorator, very useful with recursion
function addCache(orgFunc) {
  const cache = new Map();
  return function(key) { 
    if (!cache.has(key)) cache.set(key, orgFunc.call(this,key)); 
    return cache.get(key);
  };
}
```

To create decorators that keep _access to wrapped function properties_, use a **Proxy** object to wrap a function

[Proxy and Reflect](https://javascript.info/proxy#proxy-apply)

---

**Function Currying**

A technique that transforms a function of multiple arguments into several functions of a single argument in sequence `f(a,b,c) === f(a)(b)(c)`

Currying is possible because of **closures —** a returned function has access to previous argument(s) in chain.

Uses

- generate single purpose functions that are easy to maintain and debug
- flexible invocations ⇒ sum(1,2)(3) or sum(1)(2)(3) etc…
- **function composition :** a technique to use a HOF to chain functions in particular order. It improves readability and reusability of code, since you can chain same set of functions in various ways to create distinct HOFs

---

Links :

Lambda calculus

1. This [Medium article](https://medium.com/@axdemelas/lambda-calculus-with-javascript-897f7e81f259)
2. This [Medium article](https://medium.com/functional-javascript/lambda-calculus-in-javascript-part-1-28ff63824d4d) a tutorial on lambda calculus in JavaScript.
3. This [blog post](https://www.willtaylor.blog/an-introduction-to-lambda-calculus-explained-through-javascript/) explains how to apply its concepts to JavaScript.