## Slot pattern

decouples component from its content and makes it more reusable
either `{children}` or `{slot}`

## Render props

A function prop that render JSX using inputs. Implement the pattern "child as a function"
- helps to move part of rendering logic to parent
- avoids prop drilling
- no name collisions like in HOC pattern

Cons: No access to life-cycle methods ; wrapper hell

```jsx
const Card = (props) => <>
	{props.renderHead()}
	{props.children()} </>

const Greet = (head, text) => 
	<Card renderHead={() => <h1>{head}</h1> }>
		{text => <p>{text}</p>}
	</Card>
```

## Compound pattern

Components are given explicit relationship and work together to form a useful UI piece. 
Goal is to 
- build cohesive components but with minimal coupling.
- avoid prop drilling by implicit state sharing

```jsx
const Ctx = createContext()
const FlyOut = ({ children }) => { //top level provides state and setter
  const [open, setter] = useState(false);
  return <Ctx.Provider value={{open, setter}}>{children}</Ctx.Provider>
}

const Toggle = () => { //toggle icon
  const { open, setter } = useContext(Ctx);
  return <div onClick={() => setter(!open)}>{open? '-' : '+'}</div>
}

const List = ({ children }) => {
  const { open } = useContext(Ctx);
  return open && <ul>{children}</ul>;
}

const Item = ({ children }) => <li>{children}</li>

FlyOut.Toggle = Toggle; FlyOut.List = List; FlyOut.Item = Item;
```

```jsx
<FlyOut>
      <FlyOut.Toggle />
      <FlyOut.List>
      <FlyOut.Item>Edit</FlyOut.Item>
      <FlyOut.Item>Delete</FlyOut.Item>
      </FlyOut.List>
</FlyOut>
```


## ref callback

This pattern combines useRef+useEffect

- a fn passed to `ref` attribute, viz called when 
	- React attaches/detaches the node (`node/null`) 
	- when fn object changes
- wrap in `useCallback` to customise when to run
- setters can be safely used as refcb, since setter is stable

```jsx
function scrollTo(node) {
  if (!el) return;
  el.scrollIntoView({ behavior: "smooth" });
}
//in li
<li ref={index+1 == list.length? scrollTo : null} ></li>
```

PORTALS
```jsx
//use a ref callback to grab location
<div id="modal-dom-location" ref={setModalNode} />

//use context/props to spread modalNode to tree

//create portals to location
const Modal = () => modalNode && createPortal(<Ele/>, modalNode)
```

## Hooks pattern

- make data flow explicit (avoid wrapper hell)
- share stateful logic, but not state
- rerun on each render
- hooks communicate by passing reactive values
- wrap handlers received by them into `useEffectEvent` for linter
- extract logic using useEffect for future-proofing




## HOC pattern

Why?
- cover edge cases in components
- same conditional render logic for many
- guard component from modifications by container

Best Practices
- HOC with higher priority/critical checks should enclose lower ones
- HOC must always forward `props` / `refs`
- newC = reduceRight(f,g,h)(OldC)
- use a `config` object/value to customise -> hoc(config, comp)

```jsx
import {useState} from 'react'

const withLoader = (url, Component) => (props) => {
	const [data, setData] = useState(null);
	const ref = (node) => {
		if(node) fetch(url).then(res => res.json()).then(setData);
	}
	return data? <Component {...props} payload={data} /> : <p ref={ref}>Loading ....</p>
}

export const DogImg = withLoader('https://dog.ceo/api/breeds/image/random', 
	function({payload, ...props}) { //from HOC
		return <img {...props} src={payload.message}/>
});

export const Quote = withLoader('https://api.quotable.io/quotes/random', 
	function({payload, ...props}) {
	return <blockquote {...props}>{payload[0].content}</blockquote>
})
```

## Fetching data patterns

3 states : `data`, `loading`, `error` are needed in fail safe api-calling component.

__Form fetch__
```jsx
<form onSubmit={handler}>
//don't forget preventDefault()
```

__Within useEffect of custom Hook__
```jsx
//pattern 1 (fetchall-then render)
Promise.all(f1, f2, f3)
.then( responses => responses.map(res => res.json()))
.then( datas => x/* set states */);

//pattern 2 (render-as-you-fetch)
f1.then(set1); f2.then(set2); f3.then(set3) 

//setting all at one place in top-level avoid waterfalls 
//ie. when conditionally rendered child starts fetching only when parent returns it
```

__ContextProviders__
```jsx
export { f1provider, useF1 } //create for f2 and f3 too

//wrap subtree with all 3 and get ctx values with use*
```

__Hidden Child__
```jsx
{isLoading && 'loading...'}
<div style={{display : isLoading && "none"}}><Child /><div />
```

__Outside Reactroot__
for critical resources only 