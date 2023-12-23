https://www.robinwieruch.de/react-render-props/
https://www.patterns.dev/react/
[Update state from props](https://www.robinwieruch.de/react-derive-state-props/)
https://www.robinwieruch.de/react-styled-components/
https://www.robinwieruch.de/react-state/

#### Composition

used to create new components from existing ones. Relies on user-defined props or pre-defined `props.children`. Composition -> reusable, readable, and extendable components.

```jsx
// specific components render generic ones with extra props & markup
SpecialCard = ({children, ...extras}) => <Card {...extras}>{children}</Card>

Card = ({children}) => <div class='card'>{children}</div> 
//when only 1, use children, refers to nested JSX within component

//if multiple components, use SLOT pattern
 <Profile
    user={user}
    avatar={<AvatarRound user={user} />} //place it anywhere as {avatar}
    biography={<BiographyFat user={user} />} // {biography}
  />
```

A specialized component is a component that is built from its accepted props to handle one specific case.
A container component, is a component that provides the state and behavior to its children components.
[React's Higher-Order Components](https://www.robinwieruch.de/react-higher-order-components/) can be used for Component Compositions too.

#### Render Props

- are functions which render JSX using inputs.
- implement the pattern "child as a function"
- helps to move part of rendering logic to parent

```jsx
const App = () => <Amount rProp={(amount) => <Euro amount={amount} />} />
const Amount = ({rProp}) => <>Converting ... {rProp(100)} </> //can be state var

//refactored way
const App = () => <Amount>{(amount) => <Euro amount={amount} />}</Amount>
const Amount = ({children}) => <>Converting ... {chilren(amount)}</> 
```


#### Prop Drilling

- problem where every component between source & target has to *forward props*, even when they won't use them. Solved by **Context API**

```jsx
const Ctx = createContext();
return <Ctx.Provider value={propList}> {/*code */} </Ctx.Provider>
//propList available in entire component tree created via code
//propList may have shared states, so better state mgmt too

//access whatever needed in component with
const {prop1, prop2} = useContext(Ctx);  
```


#### Custom Hooks

To extract logic from a component and convert it into a reusable hook.

```jsx
import { useState, useCallback, useMemo } from "react";

export useToggle = () => {
  const [status, setStatus] = useState(false);

  const toggleStatus = useCallback(() => {
    setStatus((prevStatus) => !prevStatus);
  }, []);
  
  return [status, toggleStatus]
};

//to use we can
const { status: darkmode, toggleStatus: toggleDarkmode } = useToggle();
const { status: expanded, toggleStatus: toggleExpanded } = useToggle();
```

