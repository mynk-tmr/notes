**React :** a library to build web and native UI. Webpage is a tree of small reusuable components and react handles how to render them.

**JSX or JS XML**
* syntax extension that allows writing rendering logic and markup together.
* Key notes: use **Camelcase**
* Supports any renderable viz. **react element, arrays, fragments, portals, primtives**
* Empty Nodes? **Nullish, Boolean** ; not empty strings
* JSX is **transpiled** to `React.createElement()`

```jsx
//render boolean
`${myvale}`

//without JSX
React.createElement('h1', {id:'heading', onClick: handler }, 'hello'); //hello is children
```

**React Component** : 
* Building blocks of React App. A component encapsulate one piece of UI.
* a class/constructor function for a React Element. Rendering a component means creating an instance 
```jsx
 object = {type, props : [Object] } // props has children key too
```

**React's root**
* top-level element that is the parent of all other components. 
* Used as entry point for React to takeover DOM handling. 

```jsx
const root = ReactDOM.createRoot(#root); //SSR use hydrateRoot
root.render(<App/>); //clear everything in root
root.unmount(); //destroy Rendered tree and makes root unusable
```

**Virtual DOM**
- a lightweight in-memory representation of actual DOM kept by React
- created on each trigger and compared with previous VDOM to reconciliate
- **Diffing algorithm (O(n))** : how VDOM is compared using **BFS-search**. If nodes are **referentially same,** they are considered equal (**`Object.is()`** strategy ).

**React Reconciliation**: 
* process through which React updates DOM fast and efficiently. It's a 3 phase process
* **Trigger** : a re-render is triggered when reactive value change.
* **Render**: 
	- involves running component definitons in subtree with triggerer as root
	- On *first* render of component, all nodes created are added to virtual DOM
	- On *re-renders*, React checks differing nodes and update *virtual nodes*. (using diffing algo)
- **Commit**: react performs minimum operations on actual DOM needed to paint updated nodes using **react-dom** 

**Conditional Rendering**
* rendering only a subset of components, based on app's state. 

**Keys**
- used to give an **identity** to elements in lists. Each sibling must have **unique constant** key
- how react identifies **position** of element in render tree. If key changes, component remounts
- non-index key -> fix re-order bugs

**Portals**
* lets a component render some of its children into a different place in the DOM. 
* only changes the physical placement of the DOM node. It acts as child node of the React component that renders it (bubbling etc.)

```jsx
<div id="modal-dom-location" ref={setNode} />
const Modal = () => node && createPortal(<Container/>, node)
```

**Props**
- object used by components to pass data down the tree. Two types - standard and user-defined e.g. `id` and `lastActive`
- To communicate up, use callback props, render props, children function

**State**
- a component's private data that may change over re-renders. 
- props are managed by ancestor, state is managed within component

**Refs**
- objects that are **mutable** but don't trigger re-renders. Unlike state, ref updates are **synchronous**
- provide a way to 
	- access DOM/React nodes for vanilla manipulation
	- store `setInterval` ids to clear later

**Ref callback** : a fn passed to `ref` attribute of JSX element, viz called when React attaches/detaches the node (`node/null`) or when fn object changes
- wrap in `useCallback` to customise when to run
- setters can be safely used as refcb, since setter is stable

```jsx
function scrollTo(node) {
  node?.scrollIntoView();
}

<li ref={index+1 == list.length? scrollTo : null} ></li>
```

**Lifting State up**
- process of shifting **shared** state among components to their closest common ancestor
- How to do? lift state and pass down setters and render props

**Render Props** : 
* A function prop that renders JSX from its inputs. 
- helps to move part of rendering logic to parent and avoid prop drilling
- Cons: No access to life-cycle methods

**Prop drilling** 
- case where all components between container and target must forward props
- causes re-render of every forwarding component

**React Events**
* a wrapper over DOM events with more features like `isDefaultPrevented()` `isPropagationStopped()`
* handlers can do **side-effects**, unlike rendering functions. 
* under the hood, React attaches event handlers at root, but this is not reflected in React events.  `e.currentTarget` may =/= `e.nativeEvent.currentTarget`
- All events propagate in React except `onScroll`. 
- `onClick` (triggered during bubble), `onClickCapture` (during capture) 

**Component Types**
- **controlled** (driven by props)
- **uncontrolled** (driven by own state)
- **functional** : functions which consume inputs and return a React element
- **class components**:  classes which implement `render()` and optionally other *lifecycle* *methods* of `React.Component` as their base class. Use only for *error-handling* and implement `getSnapshotBeforeUpdate`
- **container**: provides state and behavior to its children components.
- **higher-order**: takes component(s) as input and returns an enhanced version of them

**Pure Components**
* given same props and internal state, they render same React elements.
* It doesn't NO side-effects to outside world (itself is allowed)
* designed to prevent unnecessary re-renders, by shallow comparison of props and state
* To do in FC, use `React.memo() / useMemo / useCallback` 
* To do in class components, extend `PureComponent` instead of `Component`

**Lazy Loading**
* optimize performance by splitting your code and loading components on demand. 
* Using React.lazy and React.Suspense 

```jsx
const Try = lazy(() => import('./Try'));
const Show = () => <Suspense fallback={f}><Try/></Suspense>
```


**`children prop`**
- special prop used to pass child elements or components to a parent component.
- every renderable (also empty ones) count as child, array is spread into N children

```jsx
import {Children} from 'react'
Children.count(children)
Children.only(children) //true if len=1
Children.forEach(children, fn) MAP
Children.map(children, fn)
Children.toArray(children) //to use native JS methods
```

**Lifecycle methods** run during phases
- **mounting**: creation+insertion in DOM tree
	- `constructor` => `getDerivedStateFromProps` => `render` => `componentDidMount` 
- **updating**: re-render due to change in reactive object
	- `gDSFP` => `shouldComponentUpdate` => `render` => `getSnapshotBeforeUpdate` => `componentDidUpdate`
- **unmounting**: removed from DOM and memory  (*commit*) 
	- `componentWillUnmount` : for cleanups ; should mirror componentDidMount
- **error**: when descendant throws error
	- `getDerivedStatefromError(error)` => `componentDidCatch(error,info)`
	- `info.componentStack`: stack trace

**GET** methods run in **render** phase. **Did/Will** methods run in commit phase and can have side-effects.

**Strict mode**
* helps in writing future-proof resilient code.
* applied by wrapping component tree with the <React.StrictMode> component
* It mounts and remounts component to see if **cleanups** are done properly and there's no **side-effects** in render phase.

**Render phase**
* `static getDerivedStateFromProps(props, state)` : returns new state or null. Used when new state depends on over-time changes in prop/state
* `shouldComponentUpdate(nextProps, nextState)` : prevent re-renders. Not called on first render or forceUpdate. No effect on `children`

**Pre-commit (can read DOM)
* `getSnapshotBeforeUpdate(prevProps, prevState)`: snapshot DOM before update. Use when when new items shouldn't push away prev ones

**Commit phase**
* `componentDidMount` : used for setting values based on DOM, network resources, subscribe to events
* `componentDidUpdate`: same as above

**Batching updates**
* process of grouping multiple state updates into a single update to optimize performance.
* reduces no.of DOM operations needed.
* Happens for updates set by **same** handler, whether sync or **async** or event-based
- updates are **shallowly** **merged** in order of occurrence

To prevent batching, use 
```js
const handleClick = () => {
  flushSync(() => set('todo')); //updates state synchronously
  //rest of code
};
```

**Hooks**
* special functions available while **rendering**. They are used to 
	* extract resuable logic (custom hooks)
	* create state and mirror lifecycle methods
* Rules : call from top level of a functional component and not inside control flow (loops, if-else)

**`useState`** : to create state. It returns a stateful variable and state setter. Uses initial value only on first render. 

**`useEffect`**: used to run **side-effects** after render. 
* Helps to sync component with external systems like server, timeouts, dom events/changes. 
* uses dependancy array and cleanup returns
* emulates  `didMount, didUpdate, willUnmount` ; but runs after browser paint (class methods ran immediately after rendering)

**`useLayoutEffect`**: 
* runs **before** browserpaint but after set/unset of **refs** 
* used to read DOM updates before react actually committs them. e.g. tooltip position / scroll amount

![[Pasted image 20240104064255.png]]

**`useRef`** : to create `ref` objects, a value that persists across renderings or refer to DOM elements

```jsx
ref.current ??= initialiser();

//forwardRef
const Button = forwardRef(function (props, ref) {
	return <button ref={ref}> Click </button>
})

//sync ref and state updates
flushSync(addNewTodo);
ref.current.lastChild.scrollIntoView(); 
```

**`useImperativeHandle`**: to create a custom interface between a child and its parent component
```jsx
const fakeRef = { click() { ref.current.click() } }
const safeRef = useImperativeHandle(ref, () => fakeRef)
```

**`useContext and createContext`**
* context : an object available in a render subtree. Prevents prop drilling and context-based rendering (light/dark theme, userData, router)
- To **override** a context, wrap children with different context value
- Cons: can cause all consumer components to re-render on updates

```jsx
const Ctx = createContext(); //Provider Wrapper Component
useContext(Ctx); //use the context value of a Context
```

**`useReducer`**: complex state mgmt using pure reducers that map old state + action object => new state
Returns dispatcher (of actions) and current state.

```jsx
const reducer = (state, action) => return newstate
action = {type, payload} 
```

**`useSyncExternalStore`**: to subscribe component to **external** data/event. Re-runs everytime data mutates. It returns a **snapshot** of data

```tsx
useSyncExternalStore(subscriber, getter)
interface Subscriber {
	(getter) : unscriber
}
```

**`useTransition`**: used to update state(s) without blocking UI e.g. long tabs, tables
* function you pass to `startTransition` executes immediately and must be synchronous (no setTimeout etc)
* can be interrupted by other state updates

```jsx
const [isPending, startTransition] = useTransition();
const update = (tab) =>  {
	startTransition(() => setActiveTab(tab)) 
}
```


**Basic Class component**
```jsx
class Button extends React.Component {
  state = { edit: true };
  handler = () => this.setState({ edit: false }); //Arrow, else bind
  render() {}
}
```

**APIs** (used within class methods)
```jsx
this.setState(updater, afterRender) //queues state update before next render
updater = (state, props)
this.forceUpdate(callback)
ref = React.createRef(0)
```

**Properties**
```jsx
static defaultProps
static propTypes
static contextType = SomeCtx; // render(), access as this.context
```

**Error handling**
- *error boundary* : a component to display fallback UI on errors in descendants
- can be created only with class; implements error lifecycle methods

```jsx
class ErrorBoundary extends Component {
  state = { hasError: false };
  static getDerivedStateFromError(err) {
    return { hasError: true };
  }
  render = () =>
    this.state.hasError ? this.props.fallback : this.props.children;
}
```

**React's memo API** (for data-heavy components)
- returns a **HOC** that won't re-render if new and old props are referentially equal.
- equivalent of `shouldComponentUpdate` of class. Must memoise every child too

```jsx
const MemoList = React.memo(List);
//or
MemoList = useMemo(<List items={list}/>, [list]);
```

**`useMemo and useCallback`**
- to memoise returns of Functions and Functions themselves
- updates cached value when dependencies change
```jsx
useMemo(fn, dep_array) //must be pure, take no args
useCallback(fn, dep_arr) //no restrictions
```

**Slot** : used to pass components to children, so they can place them anywhere. Either `{children}` or `{custom}`

**Compound Pattern** : components are given explicit relationship and work together. Avoids prop drilling using context.

**Higher Order Componets** : add features to components like error-handling, conditional rendering, protection mechanisms. 
Best Practices
- HOC with higher priority should enclose lower ones
- newC = reduceRight(f,g,h)(OldC)

```jsx
const withLoader = (url, Component) => (props) => {
	//fetch data
	return data? <Component {...props} payload={data} /> : <p>Loading ....</p>
}

const DogImg = withLoader('https://dog.ceo/api/breeds/image/random', 
	function({payload, ...props}) { 
		return <img {...props} src={payload.message}/>
});
```

**How to fetch data ?**
* 3 states : `data`, `loading`, `error` are needed in fail safe api-calling component.
* done in **`useEffect`** and needs  **`useState`**
* Patterns
	* fetchAll then render : `Promise.all`
	* render-as-you-fetch: `f.then , g.then`
	* Hidden child to prevent waterfall rendering

```jsx
<div style={{display : isLoading && "none"}}><Child /><div />
```

**PropTypes** is React’s internal mechanism for adding **runtime** type checks to props. Components expose `propTypes` property to use this.

```jsx
import {string, array} from 'prop-types';

Hello.propTypes = {
  name: string, //& other primitives , any
  items: array, //& object, func
  renderable: node.isRequired, 
  Relement: elementType,
  slot: instanceOf(Avatar),
  enum : oneOf(['Red', 'Green', 'Blue']),
  word: oneOfType([string, number]),
  specialarray : arrayOf(number),
  specialobject: objectOf(string),
  prototype : shape({ active: bool , font: string }),
  equal : exact({ active: bool , font: string }),
  
  // custom validator. It should return an Error
  hexcode: (props, key, componentName) => {
	  if(!props[key].startsWith('#')) return new Error('invalid hexcode')
  }
};
```


**3 main jobs of React Router:**
- Subscribing and manipulating history stack
- Matching the URL to routes
- Rendering a nested UI from route matches

**Route** 
* object that couples URL segments to components, loaders and data mutations. 
* A route array is `routes` or `<Routes>` container which best-matches a nested route from current location

```jsx
<Route
  path='' //segment to match :id dynamic , id? optional , /* splat
  index  //default outlet
  element={<App/>} OR Component={App}  //coupled element
  children={routes} //nested routes
  loader={fn} action={fn}
  errorElement={<Err/>} OR ErrorBoundary={Err}
  lazy={() => import("./barrel")} //import and spread non-route matching props
/>
```

```js
const {id, "*": splat} = useParams(); //extract dynamic & splat segments
const [params, setNewParams] = useSearchParams();
useLocation() //get window.location
```

**Loaders and Actions**
* Each route can define a "loader" function to provide data to route element before it renders.
* Also, action are the "writes" to route loader "reads". They provide a way for apps to perform data mutations via mutation submissions
* loader re-runs to validate data after action returns

```js
loader = ({params}) : data
action = ({request, params}) : response

useActionData() //return of action fn
const rev = useRevalidator()  //rev.revalidate() 
```

```jsx
<Link 
	to='..'  //url path (relative, absolute (/) , search params)
	relative='base' // to this
	reloadDocument  //server request
	preventScrollReset // on click, not back/forth
	replace  //history.replaceState
	state={{}}  //history.state
```

For imperative link, **`useNavigate(to, config_of_props)`**

Special link with state awareness. It can pass a **render prop**.
```jsx
<NavLink className, style, children />
({ isActive, isPending, isTransitioning }) => value
```

**Outlet**: used in parent route's component to display nested matched route component. Can be used as a context provider
```jsx
const ctx = useOutletContext()
```

```jsx
< Form 
	action='..' //pathname , suffix ?index to submit to index route,
	method='GET' //POST PUT PATCH DELETE ; GET is used to URLSearchParams
	navigate={false} //don't navigate, but still revalidate
	>
// has all `Link` props too
```

**To submit form via code**
```jsx
const submit = useSubmit()
submit(value, { method: "post", encType: "text/plain" })
```

**Show blocker UI to confirm navigation** 
```jsx
const block = useBlocker(fn); //takes in (curr, nextLoc) and return T/F 
{block.state === 'blocked' && <ConfirmUI />} 

block.proceed() or block.reset()
```

**To make transitions, loading animations, optimistic UI**

```jsx
const navigation = useNavigation();
navigation.state; //idle, submitting (non-get), loading
navigation.location; //to

//has props to get Form non-GET submission
.formData .json .text .formAction etc.

for get use location.search
```

**Typescript Support**

```tsx
e : React.MouseEvent<HTMLButtonElement>
React.CSSProperties

//discriminated types
type UpdateAction = { type: 'up' | 'down' ; payload : number}
type ResetAction = {type: 'reset' }
type ReducerAction = UpdateAction | ResetAction


useRef<HTMLDivElement>(null!) 

//with class components
class Button extends Component<ButtonProps, ButtonState> {...}

//restricting props with never
type MyProp = { children? : never}

//extend inbuilt prps
type ButtonProp = {variant : 'red' | 'blue' } & React.ComponentProps<'button'>

//extracting props of a type
props : React.ComponentProps<typeof Suspense>

//generic props
const List = <T,>(props : ListProps<T>) => {...};
```
