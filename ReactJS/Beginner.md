## Intro
#### Reactjs

- A JS library to build web and native UI.
- Has rich ecosystem which enables us to build full fledged production apps
- With react, we *describe* webpage as a tree of small reusuable *components* and react handles how to render them.
- React builds *virtual DOM* from component tree. It's a lightweight in-memory representation of CTREE
- When *state* of a component changes, it's corresponding *NODE* is updated. Then **entire** virtual DOM is compared with previous version and nodes are updated by **react-dom** library. 

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
- `nullish, true, false` are not JSX showable
- https://transform.tools/html-to-jsx

To add logic & dynamic values in JSX
```jsx
className='hello' //string or ""
src={title}  //passing value to attr
<b> Hello {user} </b> //as text
{expr} //any JS expression that RETURNS value, like ${expr}
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


## 


They are functions which can take some kind of input and return a React element.