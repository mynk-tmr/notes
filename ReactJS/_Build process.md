
##### Folder structure
- `/public` : static assets like fonts, imgs, etc.
- `/src` : codebase 

```json
//eslintrc.cjs -> add
rules: {
	"react/jsx-uses-react": "off",
    "react/react-in-jsx-scope": "off"
}
```

##### THINKING IN REACT
1. Break UI into smallest components 
	1. give name, and code each state (test all)
	2. https://transform.tools/html-to-jsx
2. Create heirarchy tree
3. Build a static version
4. Decide minimum state data that app needs
	- a data should be *state* if it isn't prop, can't be computed from other data and changes over time. 
	- don't deepnest or duplicate ; combine co-updating states
5. decide where states live 
6. pass down props, state, handlers etc..