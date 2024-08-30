Next.js is a React framework for building full-stack web applications. It abstracts and automatically configures bundling, compiling, etc.

Main Features
- *Routing* : file-system based router built on top of Server Components
- *Rendering* : CSR, SSR, ISG, SSG and Streaming
- *Data Fetching*:  async/await in Server Components, and an extended `fetch` API 
- *Optimisations* : built in img, script, font, core web vitals
- *TypeScript* : out of box

## Server Rendering

### How are Server Components rendered?

On server, Next.js uses React's APIs to do rendering. The rendering work is split into chunks: by individual route segments and Suspense Boundaries.

Each chunk is rendered in two steps:
- React creates **RSC Payload**. 
	- It is a compact binary representation of rendered RSC tree. 
	- contains:
		- rendered RSC result
		- placeholders for RCCs & JS files
		- props passed from RSC to RCC
- Next.js uses it and RCC instructions to render HTML on the server.

Then, on client:
- HTML is used to show non-interactive preview of route (init load)
- RSC Payload is used to reconcile the 2 trees, and update DOM.
- JavaScript instructions are used to hydrate RCCs and make app interactive.

### Server Rendering Strategies

Handled by Nextjs, we only choose when to cache/revalidate specific data
##### Static Rendering (Default)
- routes are rendered at build time, or in the background after data revalidation. 
- The result is cached and can be pushed to CDN
- useful for non-personalised content, static blog, product page
- If a dynamic function or uncached data request is discovered, Next.js will switch to dynamically rendering the whole route
##### Dynamic Rendering
- routes are rendered for each user at request time.
- good for personalised content, search results, rendering based on request data
- fetched data can be cached (not route)
- *Dynamic Functions*
	- `cookies(), headers()` 
	- `searchParams`

##### Streaming
- enables you to progressively render UI from server. 
- work is split into chunks and streamed to the client as it becomes ready
- allows the user to see parts of the page earlier
- built into app router

## Pages and Layout

- `page` is UI that is unique to a route. 
- `layout` is UI that is shared between multiple routes. 
	- On navigation, layouts preserve state, remain interactive, and do not re-render.
	- `children` required (will be nested `layout.js`, else `page.js`)
- `template` are like layouts but they are re-mounted upon navigation, ie, state is **not** preserved, and effects are re-synchronized.
	- useful for logging page views
	- rendered between a layout and its children