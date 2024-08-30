
- Web browsers perform two main tasks: **parsing** and **rendering**.
- During **parsing**, it converts `bytes → characters → tokens → nodes`, and organise them in tree-like *data structure* DOM
- After making DOM, it **renders** DOM, ie. each node is displayed on screen

![[Pasted image 20231228224715.png]]

#### Rendering Waterfall
- **style calculation**: CSS rules are applied to DOM node to form CSSOM. It only contains presentational elements
- **layout creation**: calculate geometry of elements (vector boxes) and positions.
- **paint**: vector boxes are *rasterized* (to pixels) and put onto *layers* . This is done to improve repaints of elements likely to change by isolating changes to specific layer only
- **composition** : layers are placed in correct order. By default, 1 layer exists. 
##### When visual changes happen
- `Reflow` : changes in properties that trigger *layout* re-creation. **Expensive** process. 
- `Repaint` : no change in layout, but new pixels are needed. 
- `Recompose` : seperate layer is changed. e.g. `opacity`, `transform`. Cheapest, uses GPU and run in seperate thread from main
- Tips
	- Animate, Transition properties that <u>don't reflow</u> page to improve performance.
	- place *reflow-repaint* properties on *separate* layer using for e.g. `will-change: color` 
	- More layers -> more memory (so careful)
	- Place animating element in high `z-index` or it will trigger repaint of its layer sharers

Layer creations happen for
- container whose child has layer ; new stacking contexts
- `3D` stuff
- `<video>` with accelerated video decoding
- `<canvas>` with 3D (WebGL)
- accelerated CSS `filters`