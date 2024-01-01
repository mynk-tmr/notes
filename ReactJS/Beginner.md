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
- supports any **renderable** viz. react element, arrays, fragments, portals and JS primtive types
- `nullish, true, false` are considered *empty* nodes, but `0` or `''` aren't

```jsx
<p>{[1, 2, 3, 4]}</p> --> <p>{1}{2}{3}{4}</p>
{`${isRead}`} -> to render bools

//use JS in JSX with {IIFE}
return <div>{( () => {/*code*/} )()}<div>
```

#### Under the hood
- A component is class/constructor function for a React Element
- When component is **rendered** (`<Tag />`), an instance is created by transpiled JSX with `React.createElement(type, props, ...children)` 

```jsx
<h1 id="jsx" onClick={handler}> hello </h1>
React.createElement('h1', {id:'jsx', onClick: handler }, 'hello');
 {
	type: "h1",
	props: {
		id: "jsx",
		onClick: handler,
		children: "hello" //if multiple appear as [{}, {}]
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
root.render(<App/>);
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


## Components

Building blocks of React App. A component encapsulate one piece of UI.

#### Handling Data 
##### Props
- object used by components to *pass data* **down** the component tree (parent-> child)
- To communicate **up**, *callback* props are used.
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
- Under the hood, React attaches event handlers at root, but this is not reflected in React event objects. For example, `e.currentTarget` may =/= `e.nativeEvent.currentTarget`. For polyfilled events, types may differ

##### Synthetic events
- [read extra features](https://react.dev/reference/react-dom/components/common#animationevent-handler)

#### Types of Components

- **controlled** (driven by props)
- **uncontrolled** (driven by own state)
- **functional** : functions which consume inputs and return a React element
- **class components**: 
	- JS classes which implement `render()` and optionally other *lifecycle* *methods* of `React.Component` as their base class. 
	- use only for *error-handling* and implement `getSnapshotBeforeUpdate`
- **specialized**: built from props to handle specific cases.
- **container**: provides state and behavior to its children components.


## Component Lifecycle

Lifecycle methods run during phases
- **mounting**: creation+insertion in DOM tree
	- `constructor` => `getDerivedStateFromProps` => `render` => `componentDidMount` 

- **updating**: re-render due to change in reactive object
	- `gDSFP` => `shouldComponentUpdate` => `render` => `getSnapshotBeforeUpdate` => `componentDidUpdate`

- **unmounting**: removed and destroyed  (*commit*) 
	- `componentWillUnmount` : for cleanups ; should mirror componentDidMount

- **error**: when descendant throws error
	- `getDerivedStatefromError(error)` => `componentDidCatch(error,info)`
	- 1st -> *render* stage; returns new *state object* using error
	- 2nd -> *commit* stage; used to log errors / remedies
		- in dev mode, errors will always bubble to window
		- `info.componentStack` <- stack trace


**--- Render (pure) ---**
```jsx
 constructor(props) {
    super(props);
    this.state = ... //don't setState here
    this.handler = this.handler.bind(this);
  }
```

```jsx
static getDerivedStateFromProps(props, state)
/* invoked pre-render
should return new state object or null
used when new state depends on over-time changes in prop/state*/
<Transition />
```

```jsx
shouldComponentUpdate(nextProps, nextState)
/* returns true/false to hint React to re-render
	- not called on first render or forceUpdate 
	- for optimisation only (don't do deep copy/JSON stringify)
	- children are unaffected by return */
```

**--- Pre-commit (can DOM read) ---**
```jsx
getSnapshotBeforeUpdate(prevProps, prevState)
/* immediately before commit stage
	- use to capture snapshot of DOM before update
	- use when new items shouldn't push away prev ones
*/
```

**--- Commit (side-effect allowed) ---**
```jsx
componentDidMount()
/* immediately after mount, use this for 
	- inits/setState based on DOM node e.g. tooltip position
	- network request objects
	- subscriptions */
```

```jsx
componentDidUpdate(prevProps, prevState, snapshot)
/* immediately after update, use this for
	- acting on DOM, networking (only if prevs changed)
	- this.state/props refer to new ones
```

**NOTES**
- When [Strict Mode](https://react.dev/reference/react/StrictMode) is on, in development React will call `componentDidMount`, then immediately call [`componentWillUnmount`,](https://react.dev/reference/react/Component#componentwillunmount) and then call `componentDidMount` again. This helps you notice if you forgot to implement `componentWillUnmount` or if its logic doesn’t fully “mirror” what `componentDidMount` does.
- When [Strict Mode](https://react.dev/reference/react/StrictMode) is on, React will call `render` twice in development and then throw away one of the results. This helps you notice the accidental side effects


## Hooks

_Hooks_ are special functions available while [rendering](https://react.dev/learn/render-and-commit#step-1-trigger-a-render). They let you “hook into” different React features. To extract *resuable logic* from a component, create **custom hooks**

Rules
1. call from top level of a functional component.
2. not inside flow control stmts (*conditional hooks are disallowed*)

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
- handles `componentDid--Mount, Update, Unmount` logic but if code is to run before browserpaint, use **useLayoutEffect**

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

### useContext hook
- solves *prop drilling*, where every component between source & target has to forward props
- makes *lifting state* easy 

```jsx
export const Ctx = createContext(null);

//to make props available to entire component tree
<Ctx.Provider value={props}> 
	<Tree/>
</Ctx.Provider>

//access whatever needed in component with
const {prop1, prop2} = useContext(Ctx);  
```

### useMemo hook
- to store values derived from expensive computation
- equivalent of `shouldComponentUpdate` of class

### useSyncExternalStore
- use to update component when **external** data (like server data or browser API object) changes value

```jsx
//custom hook to see online status
export const useOnlineStatus = () => 
	useSyncExternalStore(subscribe, getSnapshot)

//get immutable snapshot of data at a point
const getSnapshot = () => navigator.onLine ;

//subscribe to events/changes ; return a function that unsuscribes
function subscribe(cb) {
	window.addEventListener('online', cb)
	window.addEventListener('offline', cb)
	return function() {
		window.removeEventListener('online', cb)
		window.removeEventListener('offline', cb)	
	} 
}
```


## Class components

**Basic**
```jsx
class Button extends React.Component {
  state = { edit: true };
  handler = () => this.setState({ edit: false }); //Arrow, else bind
  render() {}
}
//constructor not required
```

**APIs** (used *within* class methods)
```jsx
this.setState(updater, afterRender) //queues state update for next render
this.setState(obj) //Merged with state object
updater = (state, props) => //code  ; the prev ones
afterRender = () => //like componentdidUpdate()

this.forceUpdate(callback) //force re-render
```

**Properties**
```jsx
class Mycomp {
	static defaultProps = {};  
	static contextType = SomeCtx; //read 1 context
		//in render(), access as const ctx = this.context
	static propTypes = {
		name: PropTypes.string,
	}
	countRef = React.createRef(0);
}
```

```jsx
//functional equivalent
const ctx = useContext(SomeCtx);

const countRef = useRef(0); //or
const [countRef, _] = useState(() => React.createRef(0))
```

**Error handling**
- *error boundary* : a component to display fallback UI on errors in descendant React component
- can be created only with class; implements *error lifecycle* methods
- in function components, use `react-error-boundary` package

```jsx
class ErrorBoundary extends Component {
  state = { hasError: false };
  static getDerivedStateFromError(err) {
    return { hasError: true };
  }
  render = () =>
    this.state.hasError ? this.props.fallback : this.props.children;
}

//use it as 
<ErrorBoundary fallback={<p>Error occured</p>}>
	<Test/>
</ErrorBoundary>
```

**Mixins** were used to reuse class components

