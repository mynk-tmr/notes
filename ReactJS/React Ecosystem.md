## React proptypes

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

## Redux Toolkit

3 principles of redux
- a single *store* (object) holds app state
- dispatch *actions* to update state, when something happens in app. Created by factories
- define pure *reducers* for updating state based on actions

Implementing redux store
- `getState()` to access state
- `dispatch(actionfn)` for sending action objects
- `subscribe(listener)` to register and its return to unsuscribe

##### Boilerplate
```tsx
const store = createStore(combineReducers(R1, R2))
const R1 = function(state=initMoney, action) {..}
const R2 = function(state=initCakes, action) {..}

const unsub = store.subscribe(() => {..})
```

##### Middleware
to extend redux's functionality
```tsx
const logger = createrLogger()
store = createStore(R, applyMiddleware(logger, ..))
```

