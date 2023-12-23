polyfill useState
```jsx
let hooks = [], i = 0; //component's hooks, and curr_index

function useState(init) {
  let pair = hooks[i];
  if (pair) { i++; return pair; }
  pair = [init, (newState) => { pair[0] = newState; updateDOM() }];
  hooks[i++] = pair;
  return pair;
}
```

