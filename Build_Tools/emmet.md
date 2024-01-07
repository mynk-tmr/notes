# [Emmet Cheatsheet](https://docs.emmet.io/cheat-sheet/)

## Syntax

`>` : child           | `#id`
`+` : sibling         | `.class.class2`
`^` : climb up        | `[attr="value"]` (for num, "" not needed )
`()` : group          | `p>{text}+a{tx2}+{tx3}`
`li*3` : multiply     |


## HTML 

Implicit tags : `ul>.cls`  (ul>li.cls)

Item numbering : 
`$` replaced by no.  |  `$@-` reverse no.  
`$$$` 001 002...     |  `$@5`  start from 5

`c` : comment
`select+` `pic+` : add + to autofill required children


## CSS

`-prop` - suggest vendors
`bg+` - shorthand autofill
`bxsh` : box-shadow autofill
`tac` - text-align:center;
`pos:s` - position: static;
`bdtc:t` - border-top-color: transparent;
`prop:no` - prop: none;
`@f+` - @font-face { src...}
`anim-` - shorthand animation

_Giving values_
`m20` : px
`m20p` : 20%
`m20p-40` : 20% 40px
`m0-a` : 0 auto;
`fz2e` : font-size: 2em;
`fz.8r`: 0.8rem;
`cr` : color: rgb(0, 0, 0) [_tabs_ to jump edit ]
`animdur:30ms`
`p2r+fz16` : multiple styles