##### AJAX Asynchronous JavaScript and XML
- a technique in which a web app fetches content from server by making async HTTP requests, and uses new content to update parts of page
- Used for SPAs. 
- Initially,  implemented using the [`XMLHttpRequest`](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) interface, now [`fetch()`](https://developer.mozilla.org/en-US/docs/Web/API/fetch) 

##### Single Page APP
- A single webpage sent by server that makes _incremental_ updates to itself over user session
- It performs _client-side routing_ to eliminate the need to fetch new pages from server and avoid costly repaints
- CSR enables developers to manipulate *history* stack without making a document request to the server.

[10 rendering patterns](https://www.youtube.com/watch?v=Dkx5ydvtpCA)

##### Progressive web Apps
- web apps built and improved with modern APIs to enhance UX.
- Benefit : with a single codebase, we can utilise broad reach of web apps and rich capabilities of platform-specific apps

## Server Components

What 
- allow you to write UI that can be rendered and cached on server.
- 3 server rendering strategies: static, dynamic, streaming

Benefits
- Data Fetching in secure & low latency manner
- caching rendered output 
- performance : moving non-interactive UI to server, means smaller bundle is sent to client
- First Contentful Paint (FCP) is fast, SEO is better

### How are Server Components rendered?

On server, Next.js uses React's APIs to orchestrate rendering. The rendering work is split into chunks: by individual route segments and Suspense Boundaries.

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