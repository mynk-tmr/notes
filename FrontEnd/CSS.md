
```css
/* a css rule*/
selector {
	declarations
}
```

```css
/* css units */
integer , number (2.3) , length (2.3vh), % (of parent)
angle (90deg or 0.25 turn)
color (hex, rgb, hsl)
image -- url('path'), *gradient , image-set('one.ico' 1x, 'two.ico' 2x)
position -- top, left, 100px etc. (center is default)
identifier -- like red
string -- `hello`
```

Absolute units : always same in any context, since they have fixed base
Relative units : relative to something else, so base changes

## Cascade and Specificity

`Cascade` : algorithm that decides which rule to apply among several candidates.

Perform these steps, stop if 1 rule is decided.
- `@media` is true 
- comes from `transition > !important > animation` 
- If !imp : `agent > user > author` AND ... `inline` > `layer 1 to N` > `unlayered`
- If NOT !imp : `author > user > agent` AND ... `inline` > `unlayered` > `layer N to 1`
- pick `most specific` rule
- pick `later` defined one

**Specificity** : (0,0,0,0). Add 1 for each selector
  1. direct (1), inherited (0)
  2. id 
  3. attribute, class, pseudo-class
  4. element, psuedo-element

## Custom properties (--)

When you declare custom property, they are visible only to selected element and all its descendants. That's why `:root` is chosen.  Access `var(--custom, default1, default2...)`

```js
// get variable from inline style
div.style.getPropertyValue("--my-var");

// get variable from wherever
getComputedStyle(div).getPropertyValue("--my-var");

// set as inline style 
div.style.setProperty('--bg', 'blue', /* 'important' */);
```

## Grid and Flexbox

_Flexbox_ concepts :
  - main axis and cross axis
  - growth, shrink, basis

_Grid_ concepts :
 - grid tracks : rows or columns defined using `grid-template` 
 - grid lines,  grid gutter
 - grid cell & grid area (collection of adjacent grid cells)
 - ==explicit v/s implicit grid==
   - explicit : when we define grid tracks with `grid-template`
   - implcit : when css define new grid tracks for placing extra content. Their size is set by `grid-auto-*`

## CSS Selectors

##### Common
- Element `#id, .class, tag, universal (*)`
- Group `s1, s2` (apply to all args)
- Combinators (apply to 2nd arg) `s1 s2, s1>s2, s1 + s2,  s1 ~ s2`  
- Relative (1st arg implied)  `+ img`  <--- img next to any element
- Psuedo-class (:) => select in specific state
- Psuedo-element (::) => select specific parts of element
##### Attribute selectors
- `a[href='.. i]` (case-insensitive)
- `^= (startswith) $= (endswith) ~= (one of list) |='en' (en-* or en)`

##### Psuedo Selectors
- `p:is(.red, #ouch)` --> match p if any 1 is valid 
- `p:has(.red, #ouch)` --> match p if it has a *descendant* with .red or # ouch
- `p:where(s1,s2)` --> like is() ; but 0 specificity ; others compute to most specific arg
- `p:not(s1,s2)` --> not all of them
- `p::part(foo)`  --> match p shadow DOM with part="bye foo bar"
- `p::slotted(span)` --> p with a span in slot shadow dom

## Normal Flow & Box Model

- CSS treats dom nodes as **boxes** and lay them out in normal flow by default. 
- Elements are laid out in order they appear in markup
- Boxes have **outer & inner displays**  
	- `display: block flex`
	-  inner : how elements inside box are laid out e.g. `flex`, `grid`, `normal`
	- outer : how box is laid out e.g.`block`, `inline`, `inline-block` (block with no-linebreak)
- Standard Model — width and height pertain to **content** box.
- Alternate Model — they pertain to `border box`. Set by `box-sizing: border-box`
- Sub-boxes in box -- `Content` < `Padding` < `Border` < `Margin`
- *Block elements*
    - force line-breaks before and after
    - Take up full width of their container.
    - Respect width & height
    - MBP will push other elements
- *Inline elements*
    - appear on same line, unless content overflows or is too big.
    - their size = content. They can wrap around other elements.
    - Don’t respect WH
    - MBP will apply but only **Left and right** will **push** other elements and content.

##### Out of flow
- when ele is not `display: block or inline` , is `fixed, absolute, float, multi-col, table-cell`
- Creates a new **Block Formatting Context (BFC)** (inner layout) separate from rest of page. 
- `<html>` is out of flow and creates BFC for document
- `flow-root` gives container its new BFC

## Layout and Spacing

#### Logical Properties & Values
- lets us control layout based on **writing** mode.
- `-inline-` : dimension parallel to text flow 
- `-block-` :  perpendicular to text flow 
- `start` : instead of *left*, `end` : instead of *right* like in `text-align`
- `inset` , `inset-block`, `inset-inline`

![[Pasted image 20240119160145.png]]

#### Margin Collapsing
Only applies in `normal flow` and to `block` margins that are `touching`
- + margins — use largest
- - margins — next sibling overlaps previous by larger (absolute) of 2.
- A positive B negative — A + B ; if -ve ; next overlaps previous
- No collapse if `<br/> <hr/>` `overflow:` set
- `display: flow-root`, child's margins expand container's *border box* and never leaks out

#### Stacking Context
- each HTML element has its own and decides how its children are stacked in z-axis within it
- default `z-index : auto or 0`.  
- To change `z-index` of child, take it out of flow OR place it on a new layer with `opacity`, `will-change` , `transform` 
- `<select>` is always at top

## Reference

```css
.LENGTHS {
	width: 
		cm mm pt /*print*/ 
		em /* % of parent fsz (for font* props only)*/
		rem /* % of root's fsz */
		vh, vw, vmin  /* % of viewport's dim */
		min-content /* = widest child/word */
		max-content /* = enough to hold children/words */
		fit-content /* = clamp b/w min & max */
}
```

```css
.INHERITANCE {
	all: inherit;  /* all props */
	color : 
		inherit /* turn on inheritance */
		initial /* reset to spec default */
		revert /* reset to user/browser style */
		unset /* reset to natural value (init or inh) */
		revert-layer /* to previous layer */

		/*props skipped in shorthand & set to bad variables are unset*/
}
```

```css
.FUNCTIONS_CSS {
	content : 
		attr(title) /* value */
		path()  /*svg related*/
		calc(10% + 2px)
		min(5vw, 100px, 7%) /*& max */
		clamp(30px, 12%, 100px)  
		url('path/to'); /* cursor, filter, *-image, *-path  */	

	width: env(title-area-bar); /* user-agent vars 
		requires <meta name="viewport" content="viewport-fit=cover" />
	*/
}
```

```css
/*PSUEDO CLASSES & ELEMENTS */
:root
div:only-child
div:only-of-type
div:empty /* (whitespace allowed) */
div:nth-child(3n+2) /*every 3rd child starting from 2*/
div:fullscreen
div:modal
div:picture-in-picture
div:playing
div:paused
div:hover 
div:active 
div:focus-visible 
div:focus-within /* even if any child is focused */
div:lang(en)
div:dir(ltr)
a:visited 
a:link 
p:target  /* select p when <a> directs user to p */
p:target-within  /* even if directed to any child in it */

div::before,
div::after {
  content: url() / 'error'; /* img with alt */
  display: block;   /* to add w/h */
}

video::backdrop
div::first-letter
div::first-line /* only on block */
span::selection /* selected span */
li::marker {
  content: '%'; /* can be animated */
}
```

```css
/* COUNTERS */
div::after {
  counter-reset: popo; /* create/reset popo */
  counter-increment: popo; /* +1 */
  content: counter(popo); /* 1 */
  content: counters(popo, '.');   /* 1. */
}
```

```css
.POSITIONS {
  position: static; /* normal flow; no top-left support*/
  position: relative; /* normal flow ; re-position wrt itself */
  position: fixed; /* removed from flow; position fixed at viewport */
  position: absolute; /*like fixed but fixed inside non-static ancestor container*/
  position: sticky; /*static, until touches edge, then becomes absolute, requires top be set */
  will-change: transform; /* improves sticky/absolute repaint */
}
```

```css
.FLEX_LAYOUT {
	flex-direction: column-reverse; /* set main axis */
	flex-wrap: wrap;
	justify-content: start; 
	align-items: stretch; /* items in row */
	align-content: end; /* ROWS */
	place-content: end center;  /*AC JC*/
}

.CHILD_PROPS {
	flex-basis: 100px; /* init main-size; Growth/shrink will be next. */
	flex-grow: 1.2; /*growth factor, ratio of empty space to add  */
	flex-shrink: 0; /* shrink factor, ratio of -ve space subtracted */
	order: 2;
	flex: auto; /* 1 1 auto */
	flex: initial;  /* 0 1 auto default */
}
```

```css
.GRID_CONTAINER {
  grid-template-rows: 1fr auto fit-content(100px) clamp();  /*1fr 1fr = 50%:50% of free space */
  grid-template-columns: [first] 10px [second] 20px [last];
  grid-template-columns: repeat(auto-fill, minmax(100px, 1fr)); 
    /* make 100px cols w/o overflowing ; fill them with children  */
    /* if auto-fit, remove unused cols, then add free-space to each child equally*/
  
  place-items: center end; /* within CELL in block & inline */
  place-content: space-evenly center;  /* distribute row & col tracks */

  /* implicit tracks */
  grid-auto-columns: 60px; /* def: 100% */
  grid-auto-flow: dense; /* backtrack to fill empty holes */
}

.GRID_CHILD {
  grid-row: 1/-1; /*start, end*/
  grid-row: first / last;
  grid-column: 1 / span 3; /* from line-1, span 3 cols ; also = span 3 / 4 */

  place-self: end center; 
  justify-self: end; /* inline */
  align-self: first baseline; /* block ; use baseline from first/last line */

  order: -1; /* place at 1st cell (default is 0 for all) */
}

.USING_TEMPLATE_AREAS {
  grid-template-areas:
    '.      top top' 
    'aside main main' 
    'footer ' ; 
  .CHILD {
    grid-area: top; /*identifier*/
  }
}
```

```css
/* 5 generic fonts: serif, sans-serif, monospace, cursive, fantasy */

.TYPOGRAPHY { 
  font-family: system-ui, "Roboto", sans-serif, "Apple Color Emoji"; 
  font-size-adjust: 0.5; /* make font-swap better if fallback */
  font-variant: small-caps;
  text-align: right;
  font-stretch: semi-expanded;
  letter-spacing: .5rem; 
  word-spacing: .3rem; 
  line-height: .5rem; /* space b/w text lines */
  text-transform: capitalize; 
  text-indent: 2rem each-line;  /*2rem tab*/
  writing-mode: vertical-lr; /* vertical letters, LtoR newlines */
  text-orientation: upright;

  /* decorate  */
  text-decoration: line-through red wavy, overline blue dotted; 
  text-decoration-thickness: 3px;
  text-underline-position: under; /* underline appears below subscripts */
  text-underline-offset: .5rem; /* margin */
  text-emphasis: '*' blue;
  text-emphasis-position: under; 
} 

.SCROLLBAR {
  scrollbar-color: red cyan; /* thumb ,  track */
  scrollbar-width: thin; /* none (hide but still scrollable) */
  scrollbar-gutter: stable both-edges; /* no layout shift on overflow */
  scroll-snap-type: both mandatory; /*or x promixity ; requires 'smooth' scroll*/
  scroll-padding: 2rem; /*2rem gap b/w edge and snapped child */

  /*for children*/
  scroll-snap-align: center; /* snap to start/center/end egde */
  scroll-margin: 10px;  /*gap from edge */
  scroll-snap-stop: always; /* child can't bypass scroll-snap */
}

div::-webkit-scrollbar-*
div::-webkit-resizer /* resize arrows */

.SHADOW_FILTERS {
  box-shadow: 3px 3px red inset ; /* shadow at (0,0) ; box at (3,3) */
  box-shadow: 0 0 0 2em red, 0 0 0 4em yellow; /* nested borders */
  filter: blur(5px); /*apply filter to ele*/
  backdrop-filter: blur(5px); /* only to bg */
  filter: drop-shadow(3px 3px 2px cyan); /*shadow in image's shape*/
}
```

```css
.OVERFLOW_CONTENT {
  /* must be block; have set height or max-height or white-space nowrap. */
  /* use tabindex=0 and aria-label for a11y */
  overflow: auto; /* scrollbar only if needed ; JS can scroll hidden  */
  text-overflow: ellipsis;  /* ... */
  word-break: break-all;   /* break long words to nextline */
}
```

```css
.GRADIENTS {
  background-image:
    linear-gradient(blue, red 40%, orange) /* be blue at 0%, red at 40%, orange at 100% */
    linear-gradient(red 30px, green) /* hard red from 0-30px */
    linear-gradient(red , 30% , blue) /* use 30% as midpoint */
    repeating-linear-gradient(blue, red 20px) /* repeat till box is covered. */

    radial-gradient(ellipse at 0% 30%, red, yellow) /* default: circle at 50% 50% */
    repeating-radial-gradient(black 5px,white 5px 10px); /* hard repeats */
}
```

```css
.BACKGROUNDS {
  background-image: url(), url(); /* top to bottom, */
  background-size: 300px , 50px; /* for img1, img2 ; use position too */
  background-size: cover; /* clip/stretch img to fit */ 
  background-position: center 100px ; /* (x,y) box-relative */
  background-origin: border-box;
  background-clip: text; /* only behind text */
  background-blend-mode : overlay;
  background-repeat: space;  /* space-between */
  background: url('') center / cover no-repeat;
  background-attachment : scroll; 
  /*
    fixed -> to viewport ie., will never scroll.
    scroll -> to element, scroll only if page scrolls.
    local -> to element ; scroll with element's content
  */
}

```

```css
.LAYOUTING_STYLES {
  display: none;
  content-visibility: auto; /* render only when element is about to be scrolled to */
  float: inline-start; /* text-wrap around it */
  display: flow-root; /* on container to clearfloat */
  column-count: 2; /* list items in 2 cols */
  column-width: 200px; /* autocreate cols */
  column-gap: 2rem;
  column-rule: 5px solid red; /* divider */
}
```

```css
.ADVANCED {
  border-radius: 15px 30px 15px 30px;  /*set clockwise*/
  background-position: -75px 0; /* multiple imgs from a sprite, by changing X pos  */
  border-image: url(border.png) 30 round; /*https://www.w3schools.com/css/css3_border_images.asp*/
}
```