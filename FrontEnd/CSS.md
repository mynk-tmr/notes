CSS is used to define styles for web pages. To apply style, 3 ways are there - **inline, internal css and external css**

**Declaration**: made of `selector` and a `rule-set`
* CSS selector is used to apply styles to 1 or group of elements
* CSS ruleset: block that tells browser how to apply styles on selected element.

**CSS Inheritance**: 
* controls what happens when no value is specified for a property on an element. e.g. `color : inherit`
* natural value : either 
	* `inherit` : set to computed style of parent's property
	* `initial`: set to some defined initial value
* `intial` (reset to spec default) ; `revert` (reset to user/browser style) ; `unset` (reset to natural value) ; `revert-layer` (reset to previous layer)
* To reset all properties, `all : unset`

**CSS Errors :** handled with a forgiving approach 
* ignored by parser
* In **shorthand**, skipping properties or using bad values **unset** such properties

**Length**: CSS lengths are numbers + unit e.g. `2vh`
* Absolute units : always same in any context, since they have fixed base
* Relative units : relative to something else, so changes based on context

```css
em  % of parent's font
rem % of root's font
vh, vw, vmin vmax % of viewport dimensions
min-content : widest child/word
max-content : enough to hold children/words 
fit-content : clamp b/w min & max
```

**Selectors**
* ID, class, Tag, Universal `*`
- **Group**: selects all elements in list `s1, s2` 
- **Combinators**: combine selectors. Only applies to 2nd selector.  `s1 s2, s1>s2, s1 + s2,  s1 ~ s2`  
- **Relative :** don't have any selector on LHS.   `+ img`   img next to any element
- **Attribute selectors** : select elements with given attribute value.
	- `a[href='.. i]` (case-insensitive)
	- `^= (startswith) $= (endswith) ~= (one of list) |='en' (en-* or en)`
- **Pseduoclass** `:` => select in specific state or position. e.g. `p:nth-child(3)`: can take index or "even" or "odd" keywords to target elements
- **Psuedoelement** `::` => select specific parts of element 

**Pseudo-class functions** : takes a selector list as its argument, and selects any element that can be selected by 
- `p:is(.red, #ouch)` : one of selectors
- `p:has(.red, #ouch)` : has a descendant selected by any 1
- `p:where(s1,s2)`: like `is` but has 0 specificity. Others use highest specificity in list
- `p:not(s1,s2)`: none of them

**Classes vs ID** - Classes are used to group styling, while IDs are used to target specific elements on a page. IDs must be unique.

**Box model** : defines how html elements are treated as **boxes**. Each box has 4 parts — margin, border, padding, and content. 
* margin isn't counted in actual box-size
- Standard Model — width and height pertain to **content** box.
- Alternate Model — they pertain to `border box`. Set by `box-sizing: border-box`

**Margin v/s Padding** : space between borders of element v/s space between margin and content of a box

**display** : controls how an element is displayed in normal flow. Boxes have outer & inner displays  
- `display: block flex`
-  inner : how elements inside box are laid out e.g. `flex`, `grid`, `normal`
- outer : how box is laid out e.g.`block`, `inline`, `inline-block` 

**position**: controls how an element is positioned on a page. 
* `static`  normal flow; no top-left support
* `relative` normal flow ; re-position wrt itself
* `fixed` removed from flow; place at (0,0) of viewport. Position wrt viewport
* `absolute` like fixed but w.r.t non-static ancestor container
* `sticky` static, until touches edge, then becomes absolute, requires top be set
* `will-change : transform` to improve sticky/absolute repaint

**Block Formatting Context**
* set by `display: flow-root` or `position : fixed / absolute`
* creates an "isolation" between inside and outside of an element. In a BFC, elements are laid out in **normal-flow**. 
	* Block elements are placed vertically while inline-elements horizontally
	* Blocks have width-height (default: 100% width) ; inline do not. They are as wide as content.
	* **inline** elements don't have **block** margins
* external effects of BFC
	* considered **out-of-flow** w.r.t others
	* no external floats
	* No margin **collapsing** (child's margins expand BFC's *border box* and never leaks out)
* `<html>` creates BFC for document

**Logical properties** lets us control layout based on **writing** mode. They help in **i8n**
- `-inline-` : dimension parallel to text flow 
- `-block-` :  perpendicular to text flow 
- positions : `start` `end` `inset` , `inset-block`, `inset-inline`

**Margin collapsing**: 
* behavior where **block** margins of **block elements** are combined into a single margin
* Pre-requisite : margins must touch, elements are in normal flow
* How to prevent? use `<br/>, <hr/>`, set a `overflow`, create a BFC
* Rules
	- + margins - use largest
	- - margins - next sibling overlaps previous by larger of 2.
	- +- margins - A + B ; if -ve ; next overlaps previous

**float** : controls the alignment of an element. values - `left, right, none`. When an element is floated, other elements will flow around it.

**clear**: controls whether an element is allowed to float next to another element or not. Values `left, right, both, none`. It will be moved below any floated elements.

**z-index** : controls stacking order of elements. Higher on top. default `z-index : auto or 0`.  

**Stacking Context** : 
- each HTML element has its own and decides how its children are stacked in z-axis
- To change `z-index` of child, take it out of flow OR place it on a new layer with `opacity`, `will-change` , `transform` 
- `<select>` is always at top

**Transition** 
* to create and control animation effects when an element's property changes
* to specify multiple transitions, use a comma `,` 
* `transition-property` : specify property to transition when a change occurs.
* `transition-timing-function` : rate of change of the transition over time. 
```css
div {  
	transition: width 2s linear 1s; /* prop dur timing delay */
	transition: width 5s, height 2s, transform 9s; 
}
```

**Animations**
* lets an element gradually change from one style to another.
* v/s Transitions - can be looped, don't require interaction to run, and can combine complex state changes.
- `@keyframes` provides transition states and browser does interpolation.
- `animation-fill-mode`: specifies a style when animation is not playing (before it starts, after it ends, or both).	e.g. `forwards` mean retain last keyframe's style
- `animation-direction`: how to play animation on each iteration. `normal` (1st to last keyframe); `reverse`, `alternate`

```css
@keyframes example {}
animation: example 5s linear 2s infinite alternate;
	/* duration, TF, delay, iter-count, direction */
```

**Gradients** : 
* to display a bg with smooth transitions between two or more colors. e.g. `background-image`
* types - **linear** (creates a straight line transition) , **radial** (circular transitions)
```css
linear-gradient(blue, red 40%, orange) /* be blue at 0%, red at 40%, orange at 100% */
linear-gradient(red 30px, green) /* hard red from 0-30px */
linear-gradient(red , 30% , blue) /* use 30% as midpoint */
radial-gradient(red, yellow)

repeating-linear-gradient(blue, red 20px) 
repeating-radial-gradient(black 5px,white 5px 10px); /* hard repeats
```


**Flexbox** : a CSS3 layout module to align child elements in a container using a **main-axis** and **cross-axis**
* parent
	* `flex-wrap : no-wrap`: prevents flex items from wrapping to next line when they overflow the container, while `overflow: hidden` hides any content that overflows.
	* `gap, row-gap, column-gap` : spacing between flex items, both horizontally and vertically
	* `flex-direction` sets main axis, `justify-content` align items on main axis; 
	* `align-content` align **rows** on cross axis while `align-items` align **flex-items** 
*  child 
	* `flex-basis`: initial size of child
	* `flex-grow` : how a flex-item grows wr.t other elements. Ratio of empty space added to size.
	* `flex-shrink`: how a flex-item shrinks. Ratio of negative space subtracted from size.
	* `order : 3` : change ordering of item
	* `flex : initial` : default flex behavior. `0 1 auto`

**Grid** : a layout system that allows you to create 2D grid layouts for web pages.
* **explicit** grid: created by specifying size & number of rows/columns with `grid-template-*
* **implicit** grid: created by specifying only size. Number is calculated by CSS. `grid-auto-*`
* **1fr** : % of free space
* **auto-fill v/s auto-fit**: used with `repeat` function to create tracks. `auto-fill` doesn't distribute extra space among tracks, but `auto-fit` does

```css
#parent {
  grid-template-rows: 1fr fit-content(100px);
  grid-template-columns: [first] 10px [second] 20px [last];
  grid-template-columns: repeat(auto-fill, minmax(100px, 1fr)); 
  place-items: center end; /* within CELL in block & inline */
  place-content: space-evenly center;  /* distribute row & col tracks */
}

#grid-item {
  grid-row: 1/-1 or first / last;
  grid-column: 1 / span 3 or span 3 / 4;
  place-self: end center; 
  order: -1; /* place at 1st cell (default is 0 for all) */
}
```

**Grid template areas**
```css
#templating {
  grid-template-areas:
    '.     header header' 
    'aside main main';
}

#main {
	grid-area: main;
}
```

**CSS 2D Transforms** 
* a set of properties that allow you to transform an element in 2d, without affecting nearby elements.
- `transform`: specifies the transformation functions to be applied to an element
- `transform-origin`: specifies the point around which the transformation should occur
- `translate(x,y)`: moves an element along the x-axis and/or y-axis
- `rotate(deg)`: rotates an element clockwise or counterclockwise around a given point
- `scale(x,y)`: increases or decreases the size of an element
- `skew(x_deg,y_deg)`: 

```css
transform : scale(1, 0.3) translate(30px, 49px) //multiple transforms on 1 element
```

**CSS 3D transforms**
```css
translate3d(x,y,z) scale3d() skew3d() rotate3d() 
translateX() translateY() translateZ()


transform-style: preserve-3d; /* child elements will also be transformed */
perspective: 100px ; /* defines how far child elements are away from viewer. lower value --> more intensive 3D effect */

perspective-origin: left; /* position from where user is looking */
backface-visibility: hidden; /* hide object when it's not facing user */
```

**CSS Colors**
* RGBA includes an alpha value that represents the opacity of the color. `alpha` ranges from 0 (transparent) and 1 (opaque)
* Color contrast is how much the colors of the text and the background stand out from each other.

**CSS Filters**
* to adjust rendering of an element in various ways - color, blur or sharpen, brighten or contrast
* use cases: images, backgrounds, borders

```css
filter: blur(5px);
backdrop-filter: blur(5px);
filter: drop-shadow(3px 3px 2px cyan); /*shadow in image's shape*/
```

**CSS Sprite**
* a technique used to combine multiple images into a single file, reducing no. of HTTP requests required to load the page
```css
background: url('sprite.svg') no-repeat;

.img1 = background-position: 0 0;
.img2 = background-position: 75px 0;
```

**Css variables / custom properties**
* Uses : theming, creating responsive designs, and making changes to global styles
* apply to element + descendants. 

```css
div {
	--color : brown;
	background: var(--color)
}
```

```js
getComputedStyle(div).getPropertyValue("--my-var");
div.style.setProperty('--bg', 'blue', /* 'important' */);
```

**WRITE all @rules at top of file.**

**@supports** : to apply styles based on whether browser supports a CSS feature. Also called feature query
```css
@supports not (display: grid) and (display: flex) {
	..declarations
}
```

**@media** : 
* to apply styles based on a mediaquery. 
* queries allow to apply CSS styles based on user's device features & current state
* `not`: inverts the meaning of an entire media query.
* `only`: prevents older browsers that do not support media queries with media features to apply rules
* `min-width`  "apply CSS rule-set if the viewport is at least this wide"

```css
@media screen, print {} //or
@media not screen and (orientation: landscape) //first AND, then NOT
```

use a descending pixel value if you're using max-width and **ascending** for **min-width**
```less
@media (max-width: 412px) 
@media (max-width: 360px)
```

**window.matchMedia()** 
returns a MediaQueryList object. It can be used to implement logic based on media queries.
```js
let mq = matchMedia("(max-height: 480px)") //a query string
mq.matches //one time check if media matches
mq.addEventListener('change', (ev) => {
	if(ev.matches) doSomething()
})
```

**@container** : apply styles to an element based on the size of element's container.
* create a container with name and context `size, inline-size, normal` (basis of query)
```css
.post {
  container: sidebar / inline-size; 
}

@container sidebar (min-width: 400px) {
	...declarations
}
```

**@layer** 
* lets you group styles into named layers, controlling their order and interactions.
* solves the issues of specifity of rules. Conflicts between layers are resolved by using higher-priority layer styles.

```css
/* establish the order -> lowest to highest */
@layer base, components, utilities

/* add styles to layers in any order */
@layer utilities {
  .hidden { display: none; }
}

/* styles imported into to layer */
@import url('example.css') layer(<layer-name>);

/* nested layers. use as base.x */ 
@layer base {
	@layer x {}
}
```


**CSS Cascade** : algorithm for resolving conflict between styles. The rules flow down cascade, filtered out at each step, until 1 rule is left
* **direct** (not inherited)
* **changes**: @media transition animation
* **origin** : !browser !user !dev dev user browser
* **inline-style**
* **layers**: !first !last !unlayered unlayered last first
* **specificity**:  a 3-digit value given to each selector. The one with highest value wins.
* **order**: later wins

Specificity values
*  ID
* Class, Attribute, PseudoClass
* Element, PseudoELement
* **`* and >  , ~`***  : 0 

**Web safe vs Fallback fonts**
* Safe fonts - commonly installed on most devices and web browsers. 
* Fallback fonts are alternative fonts specified in case the primary font is not available 

**@font-face**: lets you load font using a url and specify its settings. We do not have to use "web-safe" fonts anymore.
```css
@font-face {  
	font-family: myFirstFont;  
	src: url(sansation_bold.woff);
	font-weight: bold
}
```


**Types of design in CSS**
* **fixed** : uses absolute units like px
* **fluid** : use relative units (%) to adapt to screen size 
* **responsive** : a single fluid layout that adapts to different screen sizes e.g. flexbox
* **adaptive**:  using different layouts for different sizes e.g. breakpoints, alternate stylesheets

**How to make responsive images**
```css
width: 100%; /* fit img in container ; scale up & down */
height: auto;
max-width: 100%; /* img never larger than original size */
object-fit: contain; /* specify how replaced element fits in its container */
```

`contain` : maintain aspect ratio (letterboxed)
`cover`: maintain AR but scale to cover container
`fill`: don't maintain AR

**Optimise image loading** : lazy loading ; reduce size like WEBP ; lower image resolution

**CSS funtions**: take in arguments and return a CSS value. eg.
* `calc(10px+50%)` : to perform math operations on values
* `min(100%, 100px) or max`: return min of values
* `clamp(40px, 13%, 100px)`: returns middle value until boundaries are reached. Else, returns boundaries
* `url('path')`: link a image path
* `image-set('one.ico' 1x, 'two.ico' 2x)`: to load images based on screen resolution or size

**Overflow in CSS**
* `overflow` : 
	* specify how to handle content when it overflows an elment's box.
	* to work, container must be block; have set height or max-height or white-space nowrap.
* `word-wrap : break-word` : split/break long words and wrap them into the next line.
* `word-break` : specify how a word should be broken when reaching the end of a line. `break-all` means break the words at any character to prevent overflow 
* `visibility:hidden`: tag is not visible, but the space is allocated for it on the page. 
* `display:none` : tag will not appear at all and No space allocated

```css
text-overflow: ellipsis;
word-break: break-all;
``` 

**`content` property:** 
* used to insert content before/after an element using `::before` & `::after`psuedoclasses
* `content: url() / 'some error'` : fallback text

**Create a custom cursor**: `cursor : url('path')`

**Background properties**
```css
background: url('') center / cover no-repeat;
```

```css
background-image: url(), url(); //top to bottom
background-position: center 100px ; // (x,y) box-relative
background-size: 300px , cover; //for img1, img2
background-repeat: space;

background-clip: text; /* show bg only behind text */
background-blend-mode : overlay
```

**How to control scrolling of image in background ?**
Using `background-attachment`. Values are -
* `fixed` : attached to viewport ie., will never scroll.
* `local`: attached to element's content ie. scrolls alongwith it
* `scroll`: attached to element's border ie. willn't scroll with content

**Typography**
```css
font-family: system-ui, "Roboto", sans-serif, "Apple Color Emoji"; 
letter-spacing: .5rem; 
word-spacing: .3rem; 
line-height: .5rem; /* set space b/w text lines in a element */
text-transform: capitalize; 
text-indent: 2rem each-line;  /*2rem tab*/
writing-mode: vertical-lr; /* vertical letters, LtoR newlines */
text-decoration: line-through red wavy, overline blue dotted;
```

**CSS preprocessor**
* scripting language that extends the capabilities of CSS
* It makes it easier to write long CSS code.
* preprocessor generates CSS code from source code written in a higher-level scripting language
* post-processor takes existing CSS code and applies transformations or optimizations to it
* preprocessor is used during development, while a post-processor is used after development to optimize performance

**BEM (Block Element Modifier)** 
* popular naming convention for writing CSS. It divides CSS into blocks, elements, and modifiers to make the code more maintainable and scalable.
* **Block**: standalone component (parent) e.g. `.btn`
* **ELement**: A child of the block that performs a specific function within it e.g. `.btn__icon`
* **Modifier**:  A flag on a block or element that changes its appearance or behavior. e.g. `.btn__icon--large`

**CSSOM** 
* in-memory representation of the CSS stylesheets applied to a document. 
* It allows JavaScript to interact with styles and make dynamic changes to them.
* use cases : dynamic styles, custom themes, change css rules

```js
const stylesheets = document.styleSheets;
const cssRules = stylesheets[0].cssRules;
for(rule of cssRules) {
	if (rule.selectorText === 'p')
	    rule.style.backgroundColor = 'yellow';
}
```

**CSS SEO optimisation** : minimise code bloat to improve load time, using relevant classnames, avoid inline styles

CSS is **scalable and maintainable** for large project by:
1. Using proper naming convection for ID and classes.
2. Using preprocessor like sass, less, etc.
3. Using performance enhancing techniques like lazy-loading, etc.

**Scrollbar styling**
Pseudo-elements : `::-webkit-scrollbar` , `::-webkit-resizer`
```css
scrollbar-color: red cyan; /* thumb ,  track */
scrollbar-width: thin; /* none (hide but still scrollable) */
scrollbar-gutter: stable both-edges; /* no layout shift on oveflow */
````

**How to create scroll snaps**
```less
// FOR CONTAINER
scroll-behaviour: smooth;
scroll-snap-type: y mandatory; //if proximity, snap to closest edge
scroll-padding: 2rem; //gap b/w child & edge

//FOR CHILDREN
scroll-snap-align: center; // which edge? start/center/end
scroll-margin: 10px;  // gap from edge
scroll-snap-stop: always; // for mobile devices
```

```css
div:only-child div:only-of-type
div:empty /* (whitespace allowed) */
div:nth-child(3n+2) /*every 3rd child starting from 2*/

div:fullscreen div:modal div:picture-in-picture div:playing div:paused 
video::backdrop 

div:focus-visible  div:focus-within
div:lang(en) div:dir(ltr)

a:visited a:link  p:target  /* select p when <a> directs user to p */
p:target-within  /* even if directed to any child in it */

div::first-letter div::first-line span::selection 

li::marker { content: '%'; }
```