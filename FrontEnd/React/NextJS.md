Next.js is a React framework for building full-stack web applications. It abstracts and automatically configures bundling, compiling, etc.

Main Features
- *Routing* : file-system based router built on top of Server Components
- *Rendering* : CSR, SSR, ISG, SSG and Streaming
- *Data Fetching*:  async/await in Server Components, and an extended `fetch` API 
- *Optimisations* : built in img, script, font, core web vitals
- *TypeScript* : out of box

## Pages and Layout

- `page` is UI that is unique to a route. 
- `layout` is UI that is shared between multiple routes. 
	- On navigation, layouts preserve state, remain interactive, and do not re-render.
	- `children` required (will be nested `layout.js`, else `page.js`)
- `template` are like layouts but they are re-mounted upon navigation, ie, state is **not** preserved, and effects are re-synchronized.
	- useful for logging page views
	- rendered between a layout and its children