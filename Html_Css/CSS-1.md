## Basic Syntax

CSS — a language used to describe a webpage's presentation on screen, paper, or in other media. 
CSS is a rule-based language.

CSS _rule_ = 
  _selector_ {
    _css declarations block_
  }


## Units (MDN vocabulory)

`integer` , `number` (+decimals), `dimension/length` (num+unit), `%` (of parent) 
`angle` : 45deg, .25 turn (turn 90deg)
`color` : hex, rgb, hsl. 
`image` : file / url() / *-gradient() / image-set('a.ico' 1x, 'b.ico' 2x). 
`gradient` : img with no intrinsic size


`position` : 2 coords - X and Y. Hardcoded values or _top left bottom right center_. Default : center. Can be combined *top 100px center right*

`identifier` : unquoted values that CSS understands eg. red

`string` : quoted values

Absolute units : always same in any context
Relative units : relative to something else


## Lengths

Print : `cm, mm, in, pt, px`

`em`: when typography, uses `parent's font-size` ; in other case uses `self's fsz` 

`rem`: `root's font size`. Doesn’t compound like em

`vw /vh`: 1vw= 1% of vw === `lvw, lvh`
`svw, svh`: unaffected by browser UI 
`vmin & vmax`: 1% of smaller or larger vdim.

`min-content` : “E pluribus unum” --> “pluribus”.
`max-content` : full sentence


Upto 200dpi `1.0` pixel-to-density ratio, 200-300dpi `1.5`, then `floor(dpi/150)`


## Controlling inheritance via special values

`color : inherit` : turn on inheritance
`initial`: revert to spec default ; 
`revert` : user OR user-agent's default styling.
`unset` : reset to natural value (ie., inherit or initial) <-- values skipped in *shorthand* css
`revert-layer` : reset to in previous cascade layer

`all: <revert>` : _property_ to reset all values of element

We override default styles to get consistent cross-browser styling
**Meyer’s Reset** : most popular
**Normalize.css** : `npm i normalize.css`


## Logical Properties and values

Provide the ability to control layout based on writing mode.
`-inline-` : dimension parallel to text flow 
`-block-` :  perpendicular to text flow 

`block-start, block-end` : top, bottom
`inline-start, inline-end` : left, right


## Cascade and Specificity

`Cascade` : algorithm that decides which rule to apply among several candidates.

Perform these steps, stop if 1 rule is decided.

  - filter relevant rules (@media, matching selectors)
  - check importance (_transitions > !important >  animations_)
  - pick 1 origin-layer
  - pick highest specificity one 
  - pick the later defined one


**!important part** : agent > user > author ; THEN pick layer, `inline` > `layer 1` > `layer 2` .... > `un-named layer` 

**origin-layer part** : author > user > agent ; THEN pick layer, `inline` > `un-named` > `layer N` .... > `layer 1` 

**specificity part**
Values : (0,0,0,0). Add 1 for each selector
  1. direct (1), inherited (0)
  2. id 
  3. attribute, class, pseudo-class
  4. type, psuedo-element


### Functions

Css functions return a value inline based on args. Important ones
`attr(title)` : return value of title
`path(<svg-string>)` : draw path (supports _offset-path_, _clip-path_, _d_)

`exp(2)` `pow(1.1,2)` 
`calc()`
`min() max()` : return min/max of given values
`clamp(min, ideal, max)` : stay ideal sized until bounds are reached (**ideal must be relative**)

You can do maths (+,- etc. without calc in them).

`url(str)` : Supports background-image, border-image, content, cursor, filter, list-style-image, mask-image, offset-path, clip-path, src

`counter(var)` 

`env(titlebar-area-*)` : return value of a user-agent variable. Useful for PWAs to avoid overlapping with UI elements. Requires <meta name="viewport" content="viewport-fit=cover" />

Trignometry functions : `cos(90deg)` and `acos(1)` to rotate without scaling


## Custom properties (--)

When you declare custom property, they are visible only to selected element and all its descendants. That's why `:root` is chosen. 

```js
// get variable from inline style
div.style.getPropertyValue("--my-var");

// get variable from wherever
getComputedStyle(div).getPropertyValue("--my-var");

// set as inline style 
div.style.setProperty('--bg', 'blue', /* 'important' */);
```

`var(--custom, default1, default2...)`
  - return value of custom property or default(s)
  - for invalid returns, values are `unset`, even if a previous rule has set the property value.
  - you can access properties declared later, in other files, 


## Grid and Flexbox

_Flexbox_ concepts :
  - flex container and flex items
  - main axis and cross axis
  - growth, shrink, basis

_Grid_ concepts :
 - grid container and grid items
 - grid tracks : rows or columns defined using `grid-template` 
 - grid lines (start and end) that delimete a grid track.
 - grid gutter/alley (gap b/w rows & columns)
 - grid cell (area defined by 1 row & 1 column track)
 - grid area (collection of adjacent grid cells)
 - grid template : visual structure of grid items
 - explicit v/s implicit grid
   - explicit : when we define grid tracks with `grid-template`
   - implcit : when css define new grid tracks for placing extra content. Their size is set by `grid-auto-*`


## CSS Selectors

_Common_
- Element  => `#id, .class, tag, universal (*)`
- Group => `s1, s2` ; if any selector is invalid, entire list fails, Fixed by `:is(s1,s2)`
- Combinators (apply to 2nd arg) => `s1 s2, s1>s2, s1 + s2,  s1 ~ s2`   [directly after / anywhere after]
- Relative (1st arg implied) =>  `+ img`  <--- img next to any element
- Psuedo-class (:) => select in specific state
- Psuedo-element (::) => select specific parts of element


_Attribute selectors_  
- a[href] --> any a with href
- a[href='yes']  --> exact this val
- [... i] --> case-insensitive
- ^=  --> begins with
- $= --> ends with
- *=  --> includes (doesn't support list)
- ~=  --> includes in list
- |='en'  --> en or en-*


_Functions_
- `p:is(.red, #ouch)` --> match p if any 1 is valid 
- `p:has(.red, #ouch)` --> match p if it has a *descendant* with .red or # ouch
- `p:where(s1,s2)` --> like is() ; but 0 specificity ; others compute to most specific arg
- `p:not(s1,s2)` --> not all of them
- `p::part(foo)`  --> match p shadow DOM with part="bye foo bar"
- `p::slotted(span)` --> p with a span in slot shadow dom


## Normal Flow & Box Model

CSS treats html elements as boxes and lay them out in normal flow by default. 
Boxes can be — block or inline ; and have an outer & inner display.

- Elements are laid out in the order they appear in the HTML code.
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

*Parts of Box*
  - Content box : area where content is displayed
  - Padding box : wraps content box & padding
  - Border box : wraps padding box & border
  - Margin box : wraps border box & margin


Standard Model — width and height pertains to content box.
Alternate Model — width and height pertains to `border box`. Set by `box-sizing: border-box`


## Margin Collapsing

- 2 positive margins — use largest
- 2 negative margins — next sibling overlaps previous by larger (absolute) of 2.
- A positive B negative — A + B ; if -ve ; next overlaps previous
- only applies in `flow layout` mode
- only vertical (or block) margins collapse
- collapse only when touching ie., no collapse if <br>, <hr> `overflow:` set
- wrapping element will NOT prevent it ; unless element's margin box remains within container's border.
  - if crosses border, container & child margins collapse
  - if parent `display: flow-root`, parent margin push = `given` and child's margin pushes _parent's borders_
- `float` , `absolute` never collapse
