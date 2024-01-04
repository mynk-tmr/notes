##### Reactjs
- A JS library to build web and native UI.
- Has rich ecosystem which enables us to build full fledged production apps
- With react, we *describe* webpage as a tree of small reusuable *components* and react handles how to render them.

## JSX (JS XML)

Syntax extension for JS that allows writing rendering logic (JS) and markup (HTML) together.
#### SYNTAX
- *camelCase* attrs (except `data-` and `aria-`) 
- supports any renderable viz. **react element, arrays, fragments, portals and JS primtive types**
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
- involves running component definitons in subtree
- On *first* render of component, *all nodes* are added to virtual DOM as per returned JSX
- On *re-renders*, React calculates differing *attribute* *values* in JSX from previous render and update *virtual nodes*. (diffing algo)

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
- React identifies a React element with *key* for updation & state managment.
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
- props are managed by *ancestor*, state is managed within *component*
- props are more performant
##### Refs
- stateful objects that are *mutable* and don't trigger re-renders
- unlike state, ref updates are *synchronous*
- provide a way to access 
	- DOM/React nodes for manipulation
	- timeout ids to clear
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
- **higher-order**: takes component(s) as input and returns an enhanced version of them

**Composition**: used to create new components from existing ones. This makes code reusable, testable & scalable.
##### Children 
- `props.children` in child = *innerJSX* that container component placed within child
- static `Children` methods
	- `count(children)` 
	- `forEach(children, fn, thisArg?)` where fn (child, index)
	- `map(children, fn, thisArg?)`
	- `only(children)` : t/f if only 1 child
	- `toArray(children)` : always before vanilla array methods
- every renderable (also empty ones) count as child, array is spread into N children 

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
setter is stable identity

useState(initializer) //its return serves as init, runs only on mount
setState(updater) //uses previous state to return next state
//both must be PURE FUNCTIONS
```

##### How state updates (batching)
- they are shallowly *merged* in order of occurrence, at end of event. So component re-renders *only once*
- state updates only on **next** re-render, not in current run.
- if setter is called during render (**outside handler**), component immediately re-renders after returning. Only component's own setter can be invoked during render.

##### State management
- tied to **key** of component instance. By default, key is *position* in render tree.
- non-index key -> fix re-order bugs
- State resets when 
	- diff component at given position
	- change in key (subtree re-mounts)
```jsx
//reset state for test? <Tag> : <Tag>
test? <Tag key={1}>:<Tag key={2}> 
{test && <Tag>}{!test && <Tag>}
```

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
	- run side-effects **after** rendering
	- sync component with external systems like **server, API, browser DOM, timeouts**
- dependency must be *reactive* value.
	- `state/prop/context/ref.current` + any value derived using them
	- these might change during re-renders. Those that don't change are *stable* (calc by React)
- Best practices
	- 1 Effect => 1 Sync
	- separate non-reactive logic into *Effect events*
	- DON'T use **object/functions** in Dep. (Object.is always =/=)
- handles `componentDid--Mount, Update, Unmount` logic but if code is to run before browserpaint, use **useLayoutEffect**

### useRef hook

```jsx
//using initialiser
const ref = useRef(null);
ref.current ??= initialiser();

//useEffect guards
```

*drill refs* from container to DOM-level nodes
```jsx
//ref.current = button HTMLElement ; reset to null upon unmount
const Button = forwardRef(function (props, ref) {
	return <button ref={ref}> Click </button>
})

//to restrict operations on ref; within Button add
const realRef = useRef(null);
useImperativeHandle(ref, () => ({ //return object
	click() { realRef.current.click() }
}))
return //button with ref={realRef}
```

*sync state update* to ref update
```jsx
flushSync(() => {
  setTodos([ ...todos, newTodo]); //update todo & DOM synchronously
});
listRef.current.lastChild.scrollIntoView(); 
```

### useContext hook
- solves *prop drilling* 
	- case where all components between container and descendant must forward props
	- causes re-render of every forwarding component
- enable components to render differently based on context
- To override a context, wrap children with different context value
- Use cases : theming, current user data, routing 

```jsx
export const Ctx = createContext(null);

//to make props available to entire component tree
//always give object to value
<Ctx.Provider value={props}> 
	<Tree/>
</Ctx.Provider>

//get CLOSEST Ctx.Provider value
const {prop1, prop2} = useContext(Ctx); 
```

### useReducer hook
- adds a *reducer* function to manage complex state transitions
- returns current *state* and *dispatcher* that internally calls `setState` with reducer's return

### useLayoutEffect hook
![[Pasted image 20240104064255.png]]



### useSyncExternalStore
- use to update component when **external** data (like server data or browser API object) changes value
- returns a **snapshot** of data

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
const countRef = useRef(0); 
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

## Memoization

#### React's memo API
- returns a **HOC** that won't run if props don't change.
- memoize data-heavy components like long lists such that only new items render. 
- equivalent of `shouldComponentUpdate` of class
- needs `useMemo` & `useCallback` to keep object/function props same.
```jsx
export const MemoList = React.memo(List);
export const MemoListItem = React.memo(ListItem);
//entire tree must be memoised
```

#### useMemo, useCallback hooks
- used for *internal optimisation* of component, unlike memo API
- 1st memoize returns of expensive functions calls. (>1ms)
- 2nd memoize *function objects* instead of values
- updates cached value when dependencies change
- wrap fn *returned* by custom hooks in `useCallback`
```jsx
const val = useMemo(fn, dep_array) //must be pure, take no args
const fn = useCallback(fn, dep_arr) //no restrictions

//allowed
const MemoList = useMemo(<List items={list}/>, [list]);
```

#### Other ways

1. using `{children}`; they aren't re-rendered
2. localising state as much possible
3. don't abuse sideeffects