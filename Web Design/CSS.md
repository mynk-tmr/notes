
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
- If !imp : `agent > user > author` AND ... `inline` > `layer 1-N` > `unlayered`
- If NO !imp : `author > user > agent` AND ... `inline` > `unlayered` > `layer N-1`
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
- Boxes can be **block** or **inline**  (formatting context) and have **outer , inner display**  
	- `display: block flex`
	-  inner : how elements inside box are laid out e.g. `flex`, `grid`, `normal`
	- outer : how box is laid out e.g.`block`, `inline`, `inline-block` (block with no-linebreak)
- Standard Model — width and height pertain to **content** box.
- Alternate Model — they pertain to `border box`. Set by `box-sizing: border-box`
- Sub-boxes in box -- `Content` < `Padding` < `Border` < `Margin`
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
- `inset` : shorthand of `top, right, bottom, left`  OR `inset-block` `inset-inline`

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

## Rendering

https://www.udacity.com/course/browser-rendering-optimization--ud860

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

Dev tools : 
- `Performance tab` > `Record` while animation running
- Run commands like `Enable FPS`, `Paint flash`  , `Show layers`
- `Performance tab > Enable advanced paint ....` . Then interact with page and check `Paint Profiler` section in that tab

Layer creations happen for
- container whose child has layer ; new stacking contexts
- `3D` stuff
- `<video>` with accelerated video decoding
- `<canvas>` with 3D (WebGL)
- accelerated CSS `filters`

## Transitions & Animations

**CSS transitions** provide a way to control animation when changing CSS properties.
Events : 
- `transitionrun` -delay- `transitionstart`, `transitionend /cancel`  (non-cancelable)
- `transitionend` event is fired in both directions (to & back)
- Properties `propertyName`, `elapsedTime` (from start) and `pseudoElement` e.g. '::before'
- Using animations with `auto` may lead to unpredictable results

[discrete animation type](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_animated_properties#discrete) : properties will flip between two values 50% into run

Tips
- use `setTimeout(12ms)` to trigger transition of appended or display:none elements
- use `@prefers-reduce` OR server side `Sec-CH-Prefers-Reduced-Motion`
##### Animations
- Improvement over transitions - can be looped, don't require trigger to run, and can combine complex state changes.
- `@keyframes` provides transition states and browser does interpolation.
- Events : just like `transition`