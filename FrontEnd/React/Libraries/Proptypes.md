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