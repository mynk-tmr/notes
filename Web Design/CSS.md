
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

Absolute units : always same in any context
Relative units : relative to something else
##### Logical Properties
- lets us control layout based on **writing** mode.
- `-inline-` : dimension parallel to text flow 
- `-block-` :  perpendicular to text flow 
- `block-start, block-end` 
- `inline-start, inline-end` 

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
 - grid lines (start and end) that delimete a grid track.
 - grid gutter/alley (gap b/w rows & columns)
 - grid cell & grid area (collection of adjacent grid cells)
 - explicit v/s implicit grid
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
- Boxes can be — **block** or **inline** ; and have an `outer & inner display.`
	- `display: block flex`
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
  - *Inner display*
    - dictates how elements inside box are laid out e.g. `flex`, `grid`, `normal`
  - *Outer display*
    - dictates how box is laid out e.g.`block`, `inline`, `inline-block` (block with no-linebreak)

#### Margin Collapsing
Only applies in `normal flow` and to `vertical` margins that are `touching`
- 2 positive margins — use largest
- 2 negative margins — next sibling overlaps previous by larger (absolute) of 2.
- A positive B negative — A + B ; if -ve ; next overlaps previous
- No collapse if <br/> <hr/> `overflow:` set
- `display: flow-root`, child's margins expand container's *border box* and never leaks out

