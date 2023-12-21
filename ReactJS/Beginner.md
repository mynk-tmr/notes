##### Reactjs

- A JS library to build web and native UI.
- Has rich ecosystem which enables us to build full fledged production apps
- With react, we *describe* webpage as a tree of small reusuable *components* and react handles how to render them.
- React builds *virtual DOM* from component tree. It's a lightweight in-memory representation of CTREE
- When *state* of a component changes, it's corresponding *NODE* is updated. Then **entire** virtual DOM is compared with previous version and nodes are updated by **react-dom** library. 

##### Setup

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

#### Components


