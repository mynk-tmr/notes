##### CLI
```shell
npm create vite@latest myapp -- --template react
```

##### Folder structure
- `/public` : static assets like fonts, imgs, etc.
- `/src` : codebase 
	- `App.jsx` : top-level component, rendered on app startup.
	- `index.css` : global styles for app
	- `main.jsx` : displays component tree within a `root` element

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
2. Create heirarchy tree
3. Build a static version
4. Decide minimum state data that app needs
	- a data should be *state* if it isn't prop, can't be computed from other data and changes over time. 
	- don't deepnest or duplicate ; combine co-updating states
6. decide where states live 
7. pass down props, state, handlers etc..