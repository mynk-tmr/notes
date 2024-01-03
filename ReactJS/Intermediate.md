
https://www.patterns.dev/react/
https://www.smashingmagazine.com/search/?q=react
[Update state from props](https://www.robinwieruch.de/react-derive-state-props/)
https://www.robinwieruch.de/react-styled-components/
https://www.robinwieruch.de/react-state/

### Compound pattern


```jsx

```

**Flyout.jsx**
```jsx

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