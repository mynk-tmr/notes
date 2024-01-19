## SVGs

SVG is a XML based markup language for vector images. Raster images are pixel based.
Pros:
  - can scale without quality loss
  - text in svg remains accessible (SEO)
  - each image component can be styled differently via CSS/JS
Tips
  - save svg as `plain` to save space.
SVG libraries : _svg.js_ & _d3.js_

#### Embedding SVGs
makes page less cacheable & can delay loading of other elements
```xml
<svg>
  <use href="#my_svg" />
</svg>
<!-- near file end -->
<svg>
  <symbol id="my_svg"> `your code here` </symbol>
</svg>
```

#### Basic SVG syntax
```jsx
<svg xmlns="..." viewbox="0 0 100 100">
  <rect class="one" fill="blue"/>
  <text> Demo SVG </text>
  
  Alt text for svg
</svg>

//elements `rect`, `text`
// attributes `fill`, `color`
// classes to target in CSS/JS
// viewbox : viewable area coords (<= svg's width*height)
```

Note: length is only `px` and written unit-less 
##### SVG elements
`rect` : x, y, rx, ry, width, height ;
`circle` : cx, cy, r   [rx=width ry=height]
`line` : x1, y1, x2, y2
`polygon` : points  ["10,50, 40,70, 70,50 "]
`polyline` : points
`text` : x, y  {use _CSS_ for text/font styling}
`tspan` : x,y   {nested in `text` to subgroup text}

#### SVG styling
SVG's presentational attributes become properties in CSS
Using `CSS variables`, JS can manipulated their values

all coords : `x, y` etc..  [svg2.0]   | `font-*` `text-decoration`
`color` : for `stroke` (border)       | `letter-spacing` `word-spacing`
`fill` : bgcol                                 | `overflow`
`stroke-width` : border-width       | `direction`
`display, opacity, visibility, mask`  |  _clip-path ,transition, animation_

## Pixel to Screen Pipeline

- Trigger visual changes, though: [CSS animations](https://web.dev/learn/css/animations), [CSS transitions](https://web.dev/learn/css/transitions), and [the Web Animations API](https://developer.mozilla.org/docs/Web/API/Web_Animations_API) , Javascript
- **Style calculations:** This is the process of figuring out which CSS rules apply to which HTML elements based on matching selectors
- **Layout:** calculate the geometry of elements and layout
- **Paint:** filling in pixels on screen. Done onto multiple surfaces, often called layers.
- **Composite:** layers are applied in correct order so that the page renders as expected

Each of these parts of the pixel pipeline represents an opportunity to introduce jank in animations, or delay the painting of frames. It's therefore important to understand exactly which parts of the pipeline your code triggers, and to investigate if you can limit your changes to only the parts of the pixel pipeline that are necessary to render them.

If you change a "layout" property like width, height, or its position (such as the `left` or `top` CSS properties), the browser needs to check all other elements and "reflow" the page. Any affected areas will need to be repainted, and the final painted elements will need to be composited back together.

If you changed a "paint-only" property for an element in CSS—for example, properties such as `background-image`, `color`, or `box-shadow`—the layout step is not necessary to commit a visual update to the page. By omitting the layout step—where possible—you avoid potentially costly layout work that could have otherwise contributed significant latency in producing the next frame.

If you change a property that requires _neither_ layout or paint, the browser can jump straight to the compositing step. This is the cheapest and most desirable pathway through the pixel pipeline for high pressure points in a page's lifecycle, such as animations or scrolling
https://www.udacity.com/course/browser-rendering-optimization--ud860

**CSS transitions** provide a way to control animation speed when changing CSS properties.
you should keep your animations to only affecting `opacity` and `transform` if you want absolute best performance

Events : 
- `transitionrun` (delay) `transitionstart`, `transitionend /cancel`  (non-cancelable)
- `transitionend` event is fired in both directions (to & back)
- Properties
	- [`propertyName`](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Transitions/Using_CSS_transitions#propertyname) : name of the CSS property whose transition completed.
	- [`elapsedTime`](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Transitions/Using_CSS_transitions#elapsedtime) : a float indicating the number of seconds the transition had been running at the time the event fired. Unaffected by [`transition-delay`](https://developer.mozilla.org/en-US/docs/Web/CSS/transition-delay).
	- `pseudoElement` to which it's assigned e.g. '::before'

Using animations with `auto` may lead to unpredictable results
[discrete animation type](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_animated_properties#discrete) means that properties will flip between two values 50% through animating between the two.

Care should be taken when using a transition immediately after:

- adding the element to the DOM using `.appendChild()`
- removing an element's `display: none;` property.

This is treated as if the initial state had never occurred and the element was always in its final state. The easy way to overcome this limitation is to apply a `setTimeout()` of a handful of milliseconds before changing the CSS property you intend to transition to.

Consider providing a mechanism for pausing or disabling animation, as well as using the [Reduced Motion Media Query](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-reduced-motion) (or equivalent [User Agent client hint](https://developer.mozilla.org/en-US/docs/Web/HTTP/Client_hints#user-agent_client_hints) [`Sec-CH-Prefers-Reduced-Motion`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Sec-CH-Prefers-Reduced-Motion)) to create a complimentary experience for users who have expressed a preference for less animation.

Meeting the 10 millisecond threshold is crucial for animations