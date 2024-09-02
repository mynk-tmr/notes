
```css
/* css units */
integer , number (2.3) , length (2.3vh), % (of parent)
angle (90deg or 0.25turn)
color (hex, rgb, hsl)
image -- url('path'), *gradient , image-set('one.ico' 1x, 'two.ico' 2x)
position -- top, left, 100px etc. (center is default)
identifier -- like red
string -- `hello`
```

Absolute units : always same in any context, since they have fixed base
Relative units : relative to something else, so changes based on context

## Cascade and Specificity

- Elements are laid out in order they appear in markup
- *Block elements*
    - force line-breaks before and after
    - Take up full width of their container.
    - Respect width & height
    - MBP will push other elements
- *Inline elements*
    - appear on same line, unless content overflows or is too big.
    - their size = content. They can wrap around other elements.
    - Don’t respect WH
    - MBP will apply but only **Left and right** will **push** other elements.
##### Out of flow
- when ele is not `display: block or inline` , is `fixed, absolute, float, multi-col, table-cell`
- Creates a new **Block Formatting Context (BFC)** (inner layout) separate from rest of page. 
- `<html>` is out of flow and creates BFC for document
- `display : flow-root` gives container its new BFC

**Logical properties** lets us control layout based on **writing** mode.
- `-inline-` : dimension parallel to text flow 
- `-block-` :  perpendicular to text flow 
- positions : `start` `end` `inset` , `inset-block`, `inset-inline`

**Margin collapsing**: Only applies in `normal flow` and to `block` margins that are `touching`
- + margins — use largest
- - margins — next sibling overlaps previous by larger (absolute) of 2.
- A positive B negative — A + B ; if -ve ; next overlaps previous
- No collapse if `<br/> <hr/>` `overflow:` set
- `display: flow-root`, child's margins expand container's *border box* and never leaks out


**Animations**
- Improvement over transitions - can be looped, don't require trigger to run, and can combine complex state changes.
- `@keyframes` provides transition states and browser does interpolation.

```css
@keyframes example {  
	from {background-color: red;}  
	to {background-color: yellow;}
	}

div {  
	animation: example 5s linear 2s infinite alternate;
	/* duration, timing-function, delay, iter-count, direction */
}

div {
	animation-direction: 
		normal /* play forward */
		reverse
		alternate
		alternate-reverse;

	animation-timing-function : ease-in; /* specifies the speed curve */
	animation-fill-mode: forwards; 
		/* specifies a style when animation is not playing (before it starts, after it ends, or both).	
	`forwards` - retain style set by last keyframe */
}
```

```js
em // % of parent's font
rem // % of root's font
vh, vw, vmin  // % of viewport dimensions
min-content // widest child/word
max-content // enough to hold children/words 
fit-content // clamp b/w min & max
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
		url('path/to');
}
```

```css
div:only-child div:only-of-type
div:empty /* (whitespace allowed) */
div:nth-child(3n+2) /*every 3rd child starting from 2*/

div:fullscreen div:modal div:picture-in-picture div:playing div:paused video::backdrop 

div:focus-visible  div:focus-within /* even if any child is focused */
div:lang(en) div:dir(ltr)

a:visited a:link  p:target  /* select p when <a> directs user to p */
p:target-within  /* even if directed to any child in it */

div::before, div::after {
  content: url() / 'error'; /* img with alt */
}

div::first-letter div::first-line span::selection 

li::marker { content: '%'; }
```

```css
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