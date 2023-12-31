##### Reactjs
- A JS library to build web and native UI.
- Has rich ecosystem which enables us to build full fledged production apps
- With react, we *describe* webpage as a tree of small reusuable *components* and react handles how to render them.

## JSX (JS XML)

Syntax extension for JS that allows writing rendering logic (JS) and markup (HTML) together.
#### SYNTAX

- only 1 JSX tag is allowed to return.
	- *fragments* `<></>` group elements without adding extra nodes.
- *camelCase* attrs (except `data-` and `aria-`) 
- close *void* tags
- `nullish, true, false` are not JSX showable, but `0` or `''` are shown.
- values supported: `"hello", 0, {expr}, {var}`

```jsx
<p>{[1, 2, 3, 4]}</p> --> <p>{1}{2}{3}{4}</p>
{`${isRead}`} -> to render bools

//use JS in JSX with {IIFE}
return <div>{( () => {/*code*/} )()}<div>
```

#### Under the hood

JSX is *transpiled* to `React.createElement()` and it returns an `object`. JSX attributes map to `keys` of object.

```jsx
React.createElement(type, props, ...children) //syntax
//type, children -> html tags, react components
//props -> object having element attrs (null, if none)

h1 = <h1 id="jsx" onClick={() => log(1)}>hello</h1>
->
h1 = React.createElement('h1', {id:'jsx', onClick: () => log(1) }, 'hello');
-> {
	type: "h1",
	props: {
		id: "jsx",
		onClick: () => log(1),
		children: "hello"
	}
}
```


## React-dom & React libraries

```jsx
render(reactNode, domNode, callback); //deprecated
render(<App/>, document.getElementById('root'));
```

`createRoot(node,options?)`: create a root node to render React components. It is entry for React to takeover DOM handling.

```jsx
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<App/>); //takes in reactNode, reactElement or string, number, null, undefined.
root.unmount(); //destroy rendered tree, root is unusable now
```

`render()` clears all existing html at first call, updates on successive calls. On initial load, it might take long to download script, then render so we use **SSR frameworks**

Server-rendered apps use `hydrateRoot` instead

## React Reconciliation

The process through which React updates Browser DOM fast and efficiently. It's a 3 step process

**Trigger** : a re-render is triggered when *state/prop/context* changes.

**Render phase**
- On *first* render of component, *all nodes* are added to virtual DOM as per returned JSX
- On *re-renders*, React will calculate differing *attribute* *values* in JSX from previous render and update *virtual nodes*. (diffing algo)
- *recursive* : if updated component returns a component, React will render that component next.

**Commit phase**
- react performs *minimum* operations on actual DOM needed to paint updated nodes, using *react-dom library

##### Virtual DOM
- a lightweight in-memory representation of actual DOM kept by React
- created on each trigger and compared with previous VDOM to render changes
- **Diffing algorithm (O(n))**
	- equal nodes have equal `type` and value of `attributes` & `styles`
	- uses *BFS* traversal and *Object.is()*


## Conditional Rendering

Rendering only a subset of components, based on app's state. Can be done with conditional flow JS stmts.
- prefer `?:` over `if-else` 
- prefer ENUMs over `switch` 
- prefer `&&` over `? val : null`. Example -> `msg > 0 && <p>New msg<p>`
- for nested conditions, either use *guard pattern* (`if(..) return`) or **HOCs**
#### KEYS
- list items are compared by React using their *key* for updation.
- Rules
	- siblings must have *unique* keys
	- keys mustn't change, ie., should be generated in *database*, and not in render logic.
- not sent in props
- if list never -/+/reorder, you can `key={index}` (default react)

### EXAMPLES

To render **lists**, use `iterative` methods and keys.
```jsx
//inline
return <ul>{msg.map((txt,i) => <li key={i}>{txt}</li>)}</ul>

//using variable
msgList = msg.map((txt,i) => <li key={i}>{txt}</li>);
return <ul>{msgList}</ul>

//>1 node for each item
<Fragment key={i}>...</Fragment>
```


Using **PROPS**
```jsx
const ListItem = ({txt, i}) => <li key={i}>{txt}</li>
const List = ({msg}) => <ul>{
	msg.map( (txt,i) => <ListItem txt={txt} i={i}/>)
}</ul>

const ShowMsg = (msg) => <><h1>Messages</h1><List msg={msg}/></>
```


Using **ENUMS** (object with key value pairs for mapping)
```jsx
Notify = (text, status) => <>{ 
	{ //ENUM
		info: <Info text={text} />,
        warning: <Warning text={text} />,
        error: <Error text={text} />,
	}[status] //'info', 'error' etc.
}</>

//OR
const getNotification = (text) => ({ /*ENUM key:values*/  });
Notify = (text, status) => <>{getNotification(text)[status]}</>
```


## Components

They are functions which consume inputs and return a React element. Components are *PasalCased*. `use strict;` -> (called twice)

#### Handling Data 
##### Props
- object used by components to *pass data* **down** the component tree (parent-> child)
- have 2 kinds of keys -> *standard* (`src` predefined) and *custom*
##### State
- a component's *private* data that may *change* over re-renders. It starts with def_value when component mounts.
- in practice, use small JSON-serialisable object as state
##### Props vs. State
- props are managed by *ancestor*, state is managed within *component*
- props are more performant
- use `props` to pass render & interactivity logic, use `state` to control interactivity
##### Lifting State up
- process of shifting *shared* state among components to their closest common *ancestor*, and control them with *props*
- Things to pass via props -> <u>shared state, setter handler and rendering logic (optional)</u>
- Don't lift unshared state

#### Handling Events

##### Handlers
- All events propagate in React except `onScroll`. 
- `onClick` (triggered during bubble), `onClickCapture` (during capture) 
- Handlers can have *side-effects* like changing state, unlike rendering functions.
- take in React *synthetic* event object
	- wrapper over dom one with more features
	- `e.nativeEvent`: underlying dom e_obj
	- `isDefaultPrevented()`
	- `isPropagationStopped()`
- 3 types
	- `{handleMe}` : use if specific
	- `{(e) => handleUs(e, ...data)}` : use to call universal handler with extra info
	- `{handleProp}` : callback handler are used to *communicate up* the component tree. Acquired from `props` 
- Under the hood, React attaches event handlers at root, but this is not reflected in React event objects. For example, `e.currentTarget` may =/= `e.nativeEvent.currentTarget`. For polyfilled events, types may differ

##### Synthetic events
- [read extra features](https://react.dev/reference/react-dom/components/common#animationevent-handler)

#### Types of Components

- **controlled** (driven by props)
- **uncontrolled** (driven by own state)
- **stateful** : have state & props; should only control interactivity
- **stateless** : only props; should only render


## Hooks

_Hooks_ are special functions available while [rendering](https://react.dev/learn/render-and-commit#step-1-trigger-a-render). They let you “hook into” different React features.

Rules 
1. call from top level of a functional component.
2. not inside loops or conditions.

### useState hook

used to create a `state` in functional components
```jsx
const [state, setState] = useState(0); //init value used only on mount
//a[0] current state
//a[1] setter to update state & schedule re-rendering

useState(initializer) //its return serves as init, runs only on mount
setState(updater) //uses previous state to return next state
//both must be PURE FUNCTIONS
```

##### useImmer 
- to allow mutation-like syntax
- `setPerson(draft => {draft.age = draft.age+1 })`

##### How state updates (batching)
- they are shallowly *merged* in order of occurrence, at end of event. So component re-renders *only once*
- state updates only on **next** re-render, not in current run.
- if setter is called during render (**outside handler**), component immediately re-renders after returning. Only component's own setter can be invoked during render.

##### State management
- change `key` to reset component + descendants (re-mounts)

### useEffect hook

```jsx
useEffect( () => {
	//sync logic
	
	return () => { 
	//cleanup & stop previous sync
	//also run on unmount 
	}
}, []) //dep array (when any dep changes)

//[] only 1st mount, no [] -> every re-render
```

- used to 
	- run side-effects (things apart from rendering)
	- sync component with external systems like **server, API, browser DOM, timeouts**
- dependency must be *reactive* value.
	- `state/prop/context` + any value derived using them
	- `ref object` from useRef (not `ref.current`)
	- these might change during re-renders. Those that don't change are *stable* (calc by React)
- Best practices
	- 1 Effect => 1 Sync
	- separate non-reactive logic into *Effect events*
	- DON'T use **object/functions** in Dep. (Object.is always =/=)
- 

### useRef hook
- to create `ref object` whose changing doesn't trigger re-render
- good alt to useEffect
- *guards* to change useEffect behaviour
```jsx
const ref = useRef(0); //only on mount -> ref.current=0

//useEffect but only on change ; toggle inside useEffect 
const firstmount = useRef(true);

//useEffect but only 1st change
const firstchange = useRef(true);
```

### useMemo hook
- to store values derived from expensive compuation of state/props
- 