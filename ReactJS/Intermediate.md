https://www.robinwieruch.de/react-render-props/
https://www.patterns.dev/react/
[Update state from props](https://www.robinwieruch.de/react-derive-state-props/)
https://www.robinwieruch.de/react-styled-components/
https://www.robinwieruch.de/react-state/

#### Slot pattern

used to compose react components together by passing them as props.
```jsx
//when only 1, just use {children}, refers to nested JSX within component
const Card = ({children}) => <div class='card'>{children}</div> 

<Card><Avatar /></Card>  //children = <Avatar />
<Card>Hello</Card>  //children = Hello 

//when multiple, pass them as props
 <Profile
    user={user}
    avatar={<AvatarRound user={user} />} //place it anywhere as {avatar}
    biography={<BiographyFat user={user} />} // {biography}
  />

```

#### Render Props

- props that are functions which render JSX using inputs.
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
