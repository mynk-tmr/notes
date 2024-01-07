## SVGs

SVG is a XML based markup language for vector images. Raster images are pixel based.
Pros:
  - unlike raster images, svg can scale without quality loss
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