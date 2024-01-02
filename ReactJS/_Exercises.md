
**Correct timers in React**
```jsx
function Clock() {
	const [time, setter] = useState(1);
	useEffect( () => {
		let updater = () => setter(time+1);
		setTimeout(updater, 1000) 
	}, 
	[timer])
	return <h1>Time is {time}</h1>
}
```

## Custom hooks

```jsx
const useToggle = (init) => {
  const [flag, setFlag] = useState(init);
  const toggler = useCallback(() => {
    setFlag(prev => !prev);
  }, []);
  return [flag, toggler]
};
```

```jsx
const useLocalStorage = (key) => {
  const [value, setValue] = useState(localStorage.getItem(key) || '');
  useEffect(() => {
    localStorage.setItem(key, value);
  }, [value]);
  return [value, setValue];
};
```

```jsx
function useReducer(reducer, initArg, initialiser) {
  const [state, setState] = useState(initArg ?? () => initialiser(initArg));
  function dispatch(action) {
    setState(prev => reducer(prev, action));
  }
  return [state, dispatch];
}
```

