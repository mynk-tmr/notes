**Optimise Animations**
- Animate, Transition properties that <u>don't reflow</u> page to improve performance.
- place *reflow-repaint* properties on *separate* layer using for e.g. `will-change: color` 
- More layers -> more memory (so careful)
- Place animating element in high `z-index` or it will trigger repaint of its layer sharers

**Optimising Handlers** : rate-limit the execution of listeners when there's a rapid series of events

**Script Loading Optimisation**
* used to embed a client-side script. `<script src='path' type='text/javascript' />`
* parsing is blocked by loading & execution of script
* Loading strategy (boolean attributes)
	* **`async` :** block & execute only after complete download, and before window's `onload` event. Use for critical ones like analytics
	* **`defer`**:  execute after page parsing, before window's `DOMcontentLoaded`. Less critical
* How to load 3rd party scripts?
	- via main script : `import('path').then(analytics => analytics.init())` 
	- for 3rd party scripts, `preconnect` & `dns-prefetch` can be used
	- lazy loading with `Intersection Observer`
	- using CDNs
	- cache script with webworker