
https://www.patterns.dev/react/
https://www.smashingmagazine.com/search/?q=react
[Update state from props](https://www.robinwieruch.de/react-derive-state-props/)
https://www.robinwieruch.de/react-styled-components/
https://www.robinwieruch.de/react-state/

**Composition**: used to create new components from existing ones. This makes code reusable, testable & scalable.
### Slot pattern

- Using `props.children` in child
	- refers to *innerJSX* that container component placed within child
- Using custom props `avatar={<Avatar />}`
- decouples component from its content and makes it more reusable
##### Children API 
- `Children.count(children)` 
- `Children.forEach(children, fn, thisArg?)`
- `Children.map(children, fn, thisArg?)`
- `Children.only(children)` : t/f if only 1 child
- `Children.toArray(children)` : always before vanilla array methods
- *fn(child, index) {}*
- every renderable (also empty ones) count as child, array is spread into N children 

### Compound pattern

- creates highly cohesive components with minimal coupling (like `<ul><li>`)
- shares implicit state by explicit parent-child relationship (avoid prop drilling)
```jsx
//we only have to import Flyout component, then build compound component
<FlyOut>
  <FlyOut.Toggle />
  <FlyOut.List>
	<FlyOut.Item>Edit</FlyOut.Item>
	<FlyOut.Item>Delete</FlyOut.Item>
  </FlyOut.List>
</FlyOut>
```

**Flyout.jsx**
```jsx
const Ctx = createContext();
export const FlyOut = ({children}) => { //top level provides state and setter
  const [open, setter] = useState(false);
  return <Ctx.Provider value={{open, setter}}>{children}</Ctx.Provider>
}

const Toggle = () => { //toggle icon
  const {open, setter} = useContext(Ctx);
  return <div onClick={() => setter(!open)}><Icon/></div>
}

const List = ({ children }) => {
  const { open } = useContext(Ctx);
  return open && <ul>{children}</ul>;
}

const Item = ({children}) => <li>{children}</li>

FlyOut.Toggle = Toggle; FlyOut.List = List; FlyOut.Item = Item;
```

#### Render Prop
- A function prop that render JSX using inputs.
- implement the pattern "child as a function"
- benefits
	- helps to move part of rendering logic to parent
	- abstracts dynamic functionality

```jsx
const App = () => <Amount rProp={(amount) => <Euro amount={amount} />} />
const Amount = ({rProp}) => <>Converting ... {rProp(100)} </>
//can also send it as children
```

## Higher Order Components

Component which takes a component as input and returns it with extended functionalities.

```jsx
const addLoader = (Component) => ({isLoading, ...props}) => 
	isLoading? <p>Loading ...</p> : <Component {...props} />

myComp = addLoader(myComp);
```

[React's Higher-Order Components](https://www.robinwieruch.de/react-higher-order-components/) 