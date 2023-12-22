## Intro
#### Reactjs

- A JS library to build web and native UI.
- Has rich ecosystem which enables us to build full fledged production apps
- With react, we *describe* webpage as a tree of small reusuable *components* and react handles how to render them.

#### Setup

```shell
npm create vite@latest myapp -- --template react
```
Chrome extension called React Developer Tools.

Folder structure
- public : static assets like fonts, imgs, etc.
- src : codebase

**`/src`** 
- `App.jsx` : top-level component which is rendered on app startup.
- `index.css` : global styles for app
- `main.jsx` : displays component tree within a `root` element via `react-dom` library (or `react-native`)

```json
//eslintrc.cjs -> add
rules: {
	"react/jsx-uses-react": "off",
    "react/react-in-jsx-scope": "off"
}
```

## JSX (JS XML)

Syntax extension for JS that allows writing rendering logic (JS) and markup (HTML) together.
#### SYNTAX

- only 1 JSX tag is allowed to be returned. 
	- *fragments* `<></>` let you group a list of children without adding extra nodes to DOM.
- *camelCase* attrs (except `data-` and `aria-`) 
- close *void* tags
- every child element (in array) must have a unique *key*.
- `nullish, true, false` are not JSX showable, but `0` or `''` are shown.
- https://transform.tools/html-to-jsx

To add logic & dynamic values in JSX
```jsx
className='hello' //string or ""
src={title}  //passing value to attr
<b> Hello {user} </b> //as text
{expr} //any JS expression that RETURNS value
```

#### Under the hood

JSX is *transpiled* to a `React.createElement()` and it returns an `object`. JSX attributes map to `keys` of object.

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

```jsx
<p>{[1, 2, 3, 4]}</p> --> <p>{1}{2}{3}{4}</p>
{`${isRead}`} -> to render bools
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

## Props

- Components accept a single arg, a props object
- used by components to *pass data* (communicate) unidirectionally from parent to child
- 2 kinds of keys -> *standard* (predefined html-std attr) and *custom*
- reflect component data currently, to update, ask new from parent.
- Props are immutable. For interactivity, use **state**

```jsx
//prop forwarding
const Avatar = (props) => <Image {...props} />

//data spreading
<Profile {...db['ichi']} />

//default values
Avatar.defaultProps = some_obj 
```


```jsx
const Card = ({children}) => <div class='card'>{children}</div> 
//children prop is passed to parent component to refer to nested JSX inside it

<Card><Avatar /></Card>  //children = <Avatar />
<Card>Hello</Card>  //children = Hello 
```


passing handlers in props
```jsx
//WAY 1
const myfun = (url) => { window.location.href = url}
return <><Button handleClick={myfun}/></>

//in Button component
return <><button onClick={() => handleClick('some_url')}></button></>

//WAY 2, shift () => to myfun 
myfun = (url) => () => { window.location.href = url}
```


## State

State is a component’s memory. 

- declared using `useState()` Hook which return 
	- a[0] -> current state value
	- a[1] -> setter function to update state & trigger re-rendering
- when state changes, component is *destroyed* & recreated with **latest** value sent to `setter`
- Internally, React matches `useState pairs` of a component by their *order*.
- State is *private* to component
- State updates are *async*, `setter` updates DOM on **next** component render.
- state updates are *batched* (combined) for re-rendering

### Tips

- combine states that change together into 1
- don't use deep nesting or duplication
- don’t put values in state that can be calculated using existing values, state, and/or props.

```jsx
//don't mutate state when using useState
setPerson({ ...person, age: prev.age + 1 }); //correct

// state is 'fixed' in handler till it runs to completion
// use state-updater function to update state using previous
setPerson(prev => ({ ...prev, age: prev.age + 1 }));

//useImmer, to allow mutation-like syntax
setPerson(draft => {draft.age = draft.age+1 })

//don't use props as state
function Message({ initialColor }) {
  const [color, setColor] = useState(initialColor); 
  //color=init only for 1st call of Message
}
```

### Errors

- infinte rendering -> `setter` is called unconditionally


Props and state are both plain JavaScript objects. Props get passed to the component whereas state is managed within the component.

## React Reconciliation

The process through which React updates the Browser DOM fast and efficiently. It's a 3 step process

**Trigger** : a re-render is triggered when *state/prop* changes

**Render phase**
- On *first* render of component, *all nodes* are added to virtual DOM as per returned JSX
- On *re-renders*, React will calculate differing *attribute* *values* in JSX from previous render and update *nodes* with changes. (diffing algo)
- This process is recursive: if the updated component returns some other component, React will render that component next, 
- works best with *pure* function components

**Commit phase**
- react performs *minimum* operations on actual DOM needed to paint updated nodes, using *react-dom library

### Virtual DOM

- a lightweight in-memory representation of actual DOM kept by React
- created on each trigger and compared with previous VDOM to render changes
- **Diffing algorithm (O(n))**
	- node is same only if same `type` and same `attributes` & `styles`
	- uses *BFS* traversal and *Object.is()*


## Conditional Rendering

Rendering only a subset of components, based on app's state. Can be done with conditional flow JS stmts.
- prefer `?:` over `if-else` 
- prefer ENUMs over `switch` 
- prefer `&&` over `? val : null`. Example -> `msg > 0 && <p>New msg<p>`
- for nested conditions, either use *guard pattern* (`if(..) return`) or **HOCs**

#### KEYS
- list items are compared by React using their *key* for updating items that changed.
- Rules
	- siblings must have *unique* keys
	- keys mustn't change, ie., should be generated in *database*, and not in render logic.
- not sent in props
- if list never -/+/reorder, you can `key={index}` (default react)


To render **lists**, use `iterative` methods and keys.
```jsx
//inline
return <ul>{msg.map((txt,i) => <li key={i}>{txt}</li>)}</ul>

//using variable
msgList = msg.map((txt,i) => <li key={i}>{txt}</li>);
return <ul>{msgList}</ul>

//>1 node for each item
<Fragment key={i}>...</Fragment> )

//use JS in JSX with {IIFE}
return <div>{( () => {/*code*/} )()}<div>
```


Using **PROPS**
```jsx
const ListItem = ({txt, i}) => <li key={i}>{msg}</li>
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


Using **HOCs**
```jsx
const addLoader = (Component) => ({isLoading, ...props}) => 
	isLoading? <p>Loading ...</p> : <Component {...props} /> //spread

myComp = addLoader(myComp);
const App = ({ list, done }) => <><myComp list={list} isLoading={!done}/></>
```


Using **HOOKS**
```jsx

```

## Components

They are functions which can take some kind of input and return a React element. Components are *PasalCased*. You can use Strict Mode to find mistakes in your components (calls them twice)


## Hooks

_Hooks_ are special functions that are only available while React is [rendering](https://react.dev/learn/render-and-commit#step-1-trigger-a-render). They let you “hook into” different React features.
Rules 
1. Hooks can only be called from the top level of a functional component.
2. Hooks can’t be called from inside loops or conditions.
