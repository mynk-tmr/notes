https://www.robinwieruch.de/react-render-props/
https://www.patterns.dev/react/
[Update state from props](https://www.robinwieruch.de/react-derive-state-props/)
https://www.robinwieruch.de/react-styled-components/
https://www.robinwieruch.de/react-state/

#### Composition

used to create new components from existing ones. Relies on custom props or standard `props.children`. Composition -> reusable, testable, and extendable components. 

Principle : specific components render generic ones with extra props & markup

```jsx
Card = ({children}) => <div class='card'>{children}</div> 
//when only 1, use children, refers to nested JSX within component

//if multiple components, use SLOT pattern
 <Profile
    user={user}
    avatar={<AvatarRound user={user} />} //place it anywhere as {avatar}
    biography={<BiographyFat user={user} />} // {biography}
  />
```

#### Render Prop
- A prop whose value is a function that render JSX using inputs. 
- implement the pattern "child as a function"
- helps to move part of rendering logic to parent

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
const App = ({ list, done }) => <><myComp list={list} isLoading={!done}/></>
```

[React's Higher-Order Components](https://www.robinwieruch.de/react-higher-order-components/) 