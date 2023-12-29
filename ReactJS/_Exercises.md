
**Correct timers in React**
```jsx
function Clock() {
	const [timer, setter] = useState(1);
	useEffect( () => {
		let updater = () => setter(timer+1);
		setTimeout(updater, 1000) 
	}, 
	[timer])
	return <h1>Time is {timer}</h1>
}
```

