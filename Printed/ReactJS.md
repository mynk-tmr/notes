##### Reactjs
- A JS library to build web and native UI. With react, we describe webpage as a tree of small reusuable components and react handles how to render them.

## JSX (JS XML)

Syntax extension for JS that allows writing rendering logic (JS) and markup (HTML) together.
#### SYNTAX
- *camelCase* attrs (except `data-` and `aria-`) 
- supports any renderable viz. **react element, arrays, fragments, portals and JS primtive types**
- `nullish, bools` are considered *empty* nodes, but `0` or `''` aren't

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
//outputs
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

**Trigger** : a re-render is triggered when *state/prop/context* value changes.

**Render phase** 
- involves running component definitons in subtree with triggerer as root
- On *first* render of component, *all nodes* are added to virtual DOM as per returned JSX
- On *re-renders*, React checks differing nodes and update *virtual nodes*. (using diffing algo)

**Commit phase**
- react performs *minimum* operations on actual DOM needed to paint updated nodes, using *react-dom library

##### Virtual DOM
- a lightweight in-memory representation of actual DOM kept by React
- created on each trigger and compared with previous VDOM to render changes
- **Diffing algorithm (O(n))**
	- equal nodes have equal `type` and value of `attributes` & `styles`
	- uses *BFS* traversal and *Object.is()*


## Conditional Rendering

Rendering only a subset of components, based on app's state. Can be done with conditional flow 
For nested conditions, either use *guard pattern* (`if(..) return`) or **HOCs**
#### KEYS
- React identifies a React element with *key* for updation & state managment.
- Rules
	- siblings must have *unique* keys
	- keys mustn't change, ie., should be generated in *database*, and not in render logic.

## Portals

_Portals_ let your components render some of their children into a different place in the DOM. It is done via  `createPortal` function. It returns a React node.

```jsx
<div>  
	<SomeComponent />  
	{createPortal(renderable, domNode, key?)}
</div>
```

A *portal* only changes the physical placement of the DOM node. It acts as child node of the React component that renders it (bubbling etc.)

## Components

Building blocks of React App. A component encapsulate one piece of UI.

#### Handling Data 
##### Props
- object used by components to *pass data* **down** the component tree (parent-> child)
- To communicate **up**, *callback* props are used.
- have 2 kinds of keys -> *standard* (`src` predefined) and *custom*
##### State
- a component's *private* data that may *change* over re-renders. It starts with def_value when component mounts.
- in practice, use small serialisable object as state
- props are managed by *ancestor*, state is managed within *component*
##### Refs
- stateful objects that are *mutable* and don't trigger re-renders
- unlike state, ref updates are *synchronous*
- provide a way to  
	- access DOM/React nodes for vanilla manipulation
	- store setInterval ids to clear later
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

**Strict mode**
- call `DidMount` -> `WillUnmount` ->  `DidMount` to see if they are mirrors. 
- calls `render` twice to check accidental side effects

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

useState(initializer) 
setState(updater)  //both must be PURE FUNCTIONS
```

##### How state updates (batching)
- they are shallowly *merged* in order of occurrence, at end of event. So component re-renders *only once*
- state updates only on **next** re-render, not in current run.
- if setter is called during render (**outside handler**), component immediately re-renders after returning. Only component's own setter can be invoked during render.

##### State management
- tied to **key** of component instance. By default, key is *position* in render tree.
- non-index key -> fix re-order bugs
- State resets when 
	- diff component type at given position
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
}, []) //dep array
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
useEffectSkipMount
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

*sync state update* to ref updates (vaz synchronous)
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
- To **override** a context, wrap children with different context value
- Use cases : theming, current user data, routing 
- Cons
	- can cause all consumer components to re-render on updates

### useReducer hook
- adds a *reducer* function to manage complex state transitions. It must be *pure* and map old state and *action* object to a new state
- returns current *state* and *dispatcher* that internally calls `setState` with reducer's return
- reducers are easy to test ; bugs are always in action usage

### useLayoutEffect hook
![[Pasted image 20240104064255.png]]

### useSyncExternalStore
- use to update component when **external** data/event changes value
- returns a **snapshot** of data

```jsx
useSyncExternalStore(subscribe, getter)
//subscribe(getter) {..} -> to sub & unsub (return) getter from event
```

### useTransition
- used to update state(s) without blocking UI
- uses
	- some Tab renders for long & blocks UI ? --> `useTransition`
	- Building a Suspense-enabled router (see docs)
- `useDeferredValue` instead when
	- transition depends on some prop or a custom Hook value
	- for controlled inputs 

```js
const [isPending, startTransition] = useTransition();
//isPending : T/F 
const update = (tab) =>  {
	startTransition(() => { //mark as non-blocking
		setActiveTab(tab) //can be interrupted by other state updates
	}) 
}
```

Notes
- function you pass to `startTransition` executes immediately and must be synchronous (no setTimeout etc)
- To call `startTransition` asyncly, place it inside `setTimeout` or `await` before it

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

## Memoization

#### React's memo API
- returns a **HOC** that won't re-render if props are equal 
- memoize data-heavy components like long lists such that only new items render. 
- equivalent of `shouldComponentUpdate` of class
- needs `useMemo` & `useCallback` in container to achieve referential equality of props
```jsx
export const MemoList = React.memo(List);
export const MemoListItem = React.memo(ListItem);
//entire subtree must be memoised
```

#### useMemo, useCallback hooks
- used for *internal optimisation* of component, unlike memo API
- 1st memoize returns of expensive functions calls. (>1ms)
- 2nd memoize *function objects* instead of values
- updates cached value when dependencies change
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

## Patterns

**Slot** : places a component in a slot in another. Either `{children}` or `{custom}`

**Render Props** : A function prop that render JSX using inputs. Implement the pattern "child as a function"
- helps to move part of rendering logic to parent
- avoids prop drilling
- no name collisions like in HOC pattern

Cons: No access to life-cycle methods ; wrapper hell

```jsx
const Card = (props) => 
	<>
	{props.renderHead('Title')}
	{props.children('Body')} 
	</>

const Greet = () => 
	<Card renderHead={head => <h1>{head}</h1> }>
		{text => <p>{text}</p>}
	</Card>
```

**Compound** : components are given explicit relationship and work together.
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

 **Ref callback** : combines useRef+useEffect
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
const [mnode, set] = useState(null);
return <div id="modal-dom-location" ref={set} />

//use context/props to spread mnode to tree

//create portals to location
const Modal = () => mnode && createPortal(<Ele/>, mnode)
```

**Hooks**
- make data flow explicit (avoid wrapper hell)
- share stateful logic, but not state
- rerun on each render
- hooks communicate by passing reactive values
- wrap handlers received by them into `useEffectEvent` for linter

#### Higher Order Components

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

## Fetching data

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

## Proptypes

PropTypes is React’s *internal mechanism* for adding type checking to component props. Components expose `propTypes` property to use this.

```jsx
import {string, array} from 'prop-types';

Hello.propTypes = {
  name: string, //& other primitives , any
  items: array, //& object, func
  renderable: node.isRequired, 
  Relement: elementType,
  slot: instanceOf(Avatar), //
  enum : oneOf(['Red', 'Green', 'Blue']),
  word: oneOfType([string, number]),
  specialarray : arrayOf(number),
  specialobject: objectOf(string),
  prototype : shape({ active: bool , font: string }),
  equal : exact({ active: bool , font: string }),
  // custom validator. It should return an Error
  hexcode: (props, hxc, componentName) => {
	  if(!props[hxc].startsWith('#')) return new Error('invalid hexcode')
  }
};
```
You can compose methods together

Composing custom validators
```jsx
MyComp.propTypes = {
  label: string.isRequired,
  total: validate(isNumeric, isNonZero)
}

function validate(...validators) { //each take 1 value & return bool
	return function(props, name, Cname) {
		validators.every(check => {
			return	check(prop[name])) || new Error(`${prop[name]} is invalid`)
	}
}
```

 Typescript validates types at _compile time_, whereas PropTypes are checked at _runtime_. Use `typescript-to-proptypes` library for conversions

## React Router

3 main jobs of React Router:
- Subscribing and manipulating the history stack
- Matching the URL to your routes
- Rendering a nested UI from route matches

### Route

Route couples URL segments to components, loaders and data mutations. A `route` is 1 object with keys to declare coupling info to React. A route array is `routes` or `<Routes>` container which best-matches a nested route from current location

```jsx
<Route
  path='' //segment to match (:id dynamic , id? optional , id/* splat ( id/..)

  index  //default outlet
  element={<App/>} OR Component={App}  //coupled element
  children={routes} //nested routes
  caseSensitive 
  id  //for react
  loader={fn} action={fn}
  errorElement={<Err/>} OR ErrorBoundary={Err}
  handle={} //for match object

  lazy={() => import("./props")} //import and spread non-route matcher props [ie. not index, path, children, caseSensitive]

  shouldRevalidate={fn} //opt out of data revalidation upon action/useRevalidator/change in url or params for rendered route

/>

//getting splats
let { org, "*": splat } = params;
```

### Loaders and Actions

Route actions are the "writes" to route loader "reads". They provide a way for apps to perform data mutations via mutation submissions

```js
const store = { text: `Secret`, showText : false }

function dataloader({params}) {
	return store;
}

async function action({request, params}) {
	let data = await request.formData();
	store.showText = Boolean(data.get('show')); 
	return {res : 'ok'}
}
//loader re-runs to validate after action returns
```

### Components

```jsx
<Link 
	to='..'  //url path (relative, absolute (/) , search params)
	relative='base' // to this
	reloadDocument  //server request
	preventScrollReset // on click, not back/forth
	replace  //history.replaceState
	state={{}}  //history.state  [accessed via useLocation()]

```

```jsx
<NavLink //special link with state awareness ; extra props
	caseSensitive
	aria-current  //on active
	className, style, children  /* can be a render prop like ({ isActive, isPending, isTransitioning }) => value */
>
```

An `<Outlet/>` in parent route component renders matched child route or nothing

```jsx
<ScrollRestoration> <Root /> </ScrollRestoration>
//restore scroll when navigated back to
//create entries of key-value [5&8/3i, (x,y)]. For custom key creation, use 
getKey={(location, matches) => location.pathname} //path based keys (properly works)
```

```jsx
< Form 
	action='..' //pathname , suffix ?index to submit to index route,
	method='GET' //POST PUT PATCH DELETE ; GET is used to URLSearchParams
	navigate={false} //don't navigate, but still revalidate
	fetcherKey='my-key' //for react
	>

//has all `Link` props too
```

### HOOKS

```jsx
const {width, height} = useParams(); //extract dynamic segments

const [params, setter] = useSearchParams(); // get & set ? params
params.get('category') ; setter(newparams)

useBeforeUnload(fn, dep_arr)
useLocation() //get window.location
useNavigate() //get history.go(), second arg takes config obj of keys -- replace, state, preventScrollReset, relative

const errors = useActionData() //return of action fn
const ctx = useOutletContext()  // <Outlet value={ctx} />
const rev = useRevalidator()  //rev.revalidate() 
```

Show blocker UI to confirm navigation 
```jsx
const block = useBlocker(fn); //takes in {curr_Loc , next_Loc} and return T/F 
{block.state === 'blocked' && <ConfirmUI />} //UI can call block.proceed() or block.reset() [for yes/no]
```

To make transitions, loading animations, optimistic UI
```jsx
const navigation = useNavigation();
navigation.state; //idle, submitting (non-get), loading
navigation.location; //to
navigation.formData; //from non-GET submission, for get use location.search
navigation.json; //if useSubmit json
navigation.text; // "" text
navigation.formEncType; // ""  
navigation.formAction; // pathname where form was
navigation.formMethod; // POST etc
```

To submit form via code
```jsx
const submit = useSubmit() // sumbit(value) can be FormData, URLSearchParams, [[key, value],] OR

submit(value, { method: "post", encType: "text/plain" })
//2nd arg is same as props of `<Form/>`
```

## ReactTs

```ts
//ISOMORPHIC COMPONENT

const Text = <E extends React.ElementType = 'div'>(props : TextProps<E>) => {}

type TextProps<E extends React.ElementType> = TextOwnProps<E> & Omit<React.ComponentProps<E>, keyof TextOwnProps<E>>

type TextOwnProps<E extends React.ElementType> = { as? : E, /* rest */}
```

```tsx
//typing events
e : React.MouseEvent<HTMLButtonElement>

//css type
React.CSSProperties

//discriminated types
type UpdateAction = { type: 'up' | 'down' ; payload : number}
type ResetAction = {type: 'reset' }
type ReducerAction = UpdateAction | ResetAction

//non-null assertion to skip optional chaining
useRef<HTMLDivElement>(null!) 

//with class components
class Button extends Component<ButtonProps, ButtonState> {...}

//restricting props with never
type MyProp = { children? : never}

//type safety, ...rest support for wrapper components
//we can use Omit to get better type
type ButtonProp = {variant : 'red' | 'blue' } & React.ComponentProps<'button'>

//extracting props of a type
props : React.ComponentProps<typeof Suspense>

//generic props
const List = <T extends {},>(props : ListProps<T>) => {...};
```