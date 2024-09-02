CSS is used to define styles for web pages. To apply style, 3 ways are there - **inline, internal css and external css**

**Declaration**: made of `selector` and a `rule-set`

CSS selector is used to apply styles to 1 or group of elements

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
* a technique used to combine multiple images into a single image file, reducing the number of HTTP requests required to load the page
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
* `and`: combines a media feature with a media type or other media features.
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
/* establish the order up-front */
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


**CSS Cascade** : algorithm for resolving conflict between styles. The rules flow down cascade, filtered out at each step, until 1 rule-set is left
* **direct** (not inherited)
* **changes**: @media transition animation
* **origin** : !browser !user !dev dev user browser
* **inline-style**
* **layers**: !first !last !unlayered unlayered last first
* **specificity**:  a 3-digit value given to each selector. The one with highest value wins.
	*  ID
	* Class, Attribute, PseudoClass
	* Element, PseudoELement
* **order**: later wins

**`* and >, ~ etc.`***  have 0 specificity


111. ### What is the difference between fluid and fixed layouts in CSS?
    
    [](https://github.com/Saran-pariyar/100_Days_Of_Frontend_Interview_Questions#what-is-the-difference-between-fluid-and-fixed-layouts-in-css)
    
    A fluid layout in CSS adjusts its width and height based on the size of the screen, while a fixed layout has a set width and height. Fluid layouts use percentages to set their dimensions, while fixed layouts use pixels.
    
112. ### How do you make images responsive in CSS?
    
    [](https://github.com/Saran-pariyar/100_Days_Of_Frontend_Interview_Questions#how-do-you-make-images-responsive-in-css)
    
    To make images responsive in CSS, you can use the max-width: 100% property, which will make the image scale down proportionally to fit the width of its container while maintaining its aspect ratio.
    
114. ### What is the difference between responsive and adaptive design in CSS?
    
    [](https://github.com/Saran-pariyar/100_Days_Of_Frontend_Interview_Questions#what-is-the-difference-between-responsive-and-adaptive-design-in-css)
    
    Responsive design in CSS adapts to different screen sizes and devices by using flexible grids, fluid images, and media queries to adjust the layout and content of the website. Adaptive design in CSS, on the other hand, uses predefined layout sizes and breakpoints to adjust the layout and content based on the screen size and device being used.
    
115. ### How do you optimize responsive images for faster loading in CSS?
    
    [](https://github.com/Saran-pariyar/100_Days_Of_Frontend_Interview_Questions#how-do-you-optimize-responsive-images-for-faster-loading-in-css)
    
    To optimize responsive images for faster loading in CSS, you can use smaller file formats like JPEG and PNG, reduce the image size and resolution, and use lazy loading to only load images when they are needed.
    

# Day 24

[](https://github.com/Saran-pariyar/100_Days_Of_Frontend_Interview_Questions#day-24)

116. ### How does calc() work in css?
    
    [](https://github.com/Saran-pariyar/100_Days_Of_Frontend_Interview_Questions#how-does-calc-work-in-css)
    
    The CSS3 calc() function allows us to perform mathematical operations on property values. Example: `div{width: calc(100px + 50px)}`
    
117. ### What is the overflow property in CSS used for?
    
    [](https://github.com/Saran-pariyar/100_Days_Of_Frontend_Interview_Questions#what-is-the-overflow-property-in-css-used-for)
    
    The overflow property specifies what should happen if content overflows an element’s box. It's possible values are: auto, none, scroll, visible.
    
118. ### What is the difference between `visibility:hidden` and `display:none`?
    
    [](https://github.com/Saran-pariyar/100_Days_Of_Frontend_Interview_Questions#what-is-the-difference-between-visibilityhidden-and-displaynone)
    
    `visibility:hidden` means the tag is not visible, but the space is allocated for it on the page. `display:none` means the tag will not appear at all and there will be no space allocated for it between the other tags.
    
119. ### Are quotes mandatory in URL’s?
    
    [](https://github.com/Saran-pariyar/100_Days_Of_Frontend_Interview_Questions#are-quotes-mandatory-in-urls)
    
    Quotes are optional in URL’s, and it can be single or double.
    
120. ### Explain what are web-safe fonts and fallback fonts.
    
    [](https://github.com/Saran-pariyar/100_Days_Of_Frontend_Interview_Questions#explain-what-are-web-safe-fonts-and-fallback-fonts)
    
    Web-safe fonts are fonts that are commonly installed on most devices and web browsers. Fallback fonts are alternative fonts specified in case the primary font is not available on the user's device.
    

# Day 25

[](https://github.com/Saran-pariyar/100_Days_Of_Frontend_Interview_Questions#day-25)

121. ### What is the purpose of CSS content property?
    
    [](https://github.com/Saran-pariyar/100_Days_Of_Frontend_Interview_Questions#what-is-the-purpose-of-css-content-property)
    
    The CSS content property is used to insert content before or after an HTML element using the ::before and ::after pseudo-elements.
    
122. ### How can we create custom cursor in CSS?
    
    [](https://github.com/Saran-pariyar/100_Days_Of_Frontend_Interview_Questions#how-can-we-create-custom-cursor-in-css)
    
    To create a custom cursor in CSS, you can use the "cursor" property and set it to "url" with the path to the image file that you want to use as the cursor.
    
123. ### What is the "line-height" property in CSS?
    
    [](https://github.com/Saran-pariyar/100_Days_Of_Frontend_Interview_Questions#what-is-the-line-height-property-in-css)
    
    The "line-height" property in CSS is used to control the spacing between lines of text within an element.
    
124. ### What is specificity in CSS?
    
    [](https://github.com/Saran-pariyar/100_Days_Of_Frontend_Interview_Questions#what-is-specificity-in-css)
    
    Specificity in CSS is a way of determining which CSS rule applies to an element. It is based on the number of selectors and their types in a CSS rule. Specificity is calculated using a formula: inline styles have the highest specificity, followed by IDs, classes, and then elements.
    
125. ### Which property is used to control the scrolling of an image in the background?
    
    [](https://github.com/Saran-pariyar/100_Days_Of_Frontend_Interview_Questions#which-property-is-used-to-control-the-scrolling-of-an-image-in-the-background)
    
    The `background-attachment` property is used to control the scrolling of an image in the background.
    

# Day 26

[](https://github.com/Saran-pariyar/100_Days_Of_Frontend_Interview_Questions#day-26)

126. ### Which CSS property is used to capitalize text or convert text to uppercase or lowercase letters?
    
    [](https://github.com/Saran-pariyar/100_Days_Of_Frontend_Interview_Questions#which-css-property-is-used-to-capitalize-text-or-convert-text-to-uppercase-or-lowercase-letters)
    
    The text-transform property is used to capitalize text or convert text to uppercase or lowercase letters.
    
127. ### What is word-wrap property in CSS3?
    
    [](https://github.com/Saran-pariyar/100_Days_Of_Frontend_Interview_Questions#what-is-word-wrap-property-in-css3)
    
    The word-wrap property allows long words to be able to be broken in order to prevent overflow and wrap onto the next line.
    
128. ### Describe ‘rule set’ in CSS?
    
    [](https://github.com/Saran-pariyar/100_Days_Of_Frontend_Interview_Questions#describe-rule-set-in-css)
    
    It is an instruction that tells browser on how to render a specific element on the HTML page. It consists of a selector with a declaration block that follows.
    
129. ### How can you create a CSS-only dropdown menu?
    
    [](https://github.com/Saran-pariyar/100_Days_Of_Frontend_Interview_Questions#how-can-you-create-a-css-only-dropdown-menu)
    
    A CSS-only dropdown menu can be created by using the "hover" pseudo-class and the "display" property. When the user hovers over a parent element, the "display" property of the child element can be set to "block" or "inline-block" to reveal the dropdown menu.
    
130. ### What are the potential drawbacks of using CSS frameworks such as Bootstrap?
    
    [](https://github.com/Saran-pariyar/100_Days_Of_Frontend_Interview_Questions#what-are-the-potential-drawbacks-of-using-css-frameworks-such-as-bootstrap)
    
    Using CSS frameworks like Bootstrap can lead to bloated code, difficulties in customizing the design, and unoriginal or generic looks
    

# Day 27

[](https://github.com/Saran-pariyar/100_Days_Of_Frontend_Interview_Questions#day-27)

131. ### What is Tailwind CSS?
    
    [](https://github.com/Saran-pariyar/100_Days_Of_Frontend_Interview_Questions#what-is-tailwind-css)
    
    TailwindCSS is a utility-first CSS framework that provides pre-defined CSS classes that can be used to rapidly build custom user interfaces.
    
132. ### How do you customize TailwindCSS to match a specific design system or brand guidelines?
    
    [](https://github.com/Saran-pariyar/100_Days_Of_Frontend_Interview_Questions#how-do-you-customize-tailwindcss-to-match-a-specific-design-system-or-brand-guidelines)
    
    TailwindCSS provides a configuration file that can be customized to match a specific design system or brand guidelines. This file includes variables for colors, fonts, spacing, and more, which can be adjusted to match the project's needs.
    
133. ### Can you explain the difference between utility classes and component classes in TailwindCSS?
    
    [](https://github.com/Saran-pariyar/100_Days_Of_Frontend_Interview_Questions#can-you-explain-the-difference-between-utility-classes-and-component-classes-in-tailwindcss)
    
    Utility classes in TailwindCSS are small, single-purpose classes that provide a specific styling utility, such as padding, margin, or text alignment. Component classes, on the other hand, are larger classes that provide a collection of styles for a specific component, such as a button or card.
    
134. ### How do you optimize the file size of TailwindCSS in a production environment?
    
    [](https://github.com/Saran-pariyar/100_Days_Of_Frontend_Interview_Questions#how-do-you-optimize-the-file-size-of-tailwindcss-in-a-production-environment)
    
    TailwindCSS provides a purge option that removes any unused classes from the final CSS file, reducing its size. This option should be enabled in a production environment to minimize the CSS file size.
    
135. ### What are some common performance issues with TailwindCSS, and how do you optimize performance in your projects?
    
    [](https://github.com/Saran-pariyar/100_Days_Of_Frontend_Interview_Questions#what-are-some-common-performance-issues-with-tailwindcss-and-how-do-you-optimize-performance-in-your-projects)
    
    Common performance issues with TailwindCSS include the size of the CSS file and the number of classes being generated. To optimize performance, it is important to enable the purge option in a production environment, use a caching mechanism to speed up builds, and avoid generating unnecessary classes.
    

# Day 28

[](https://github.com/Saran-pariyar/100_Days_Of_Frontend_Interview_Questions#day-28)

136. ### What is CSS preprocessor?
    
    [](https://github.com/Saran-pariyar/100_Days_Of_Frontend_Interview_Questions#what-is-css-preprocessor)
    
    A CSS preprocessor is a scripting language that extends the capabilities of CSS which makes it easier and more efficient to write CSS code.
    
137. ### What is the difference between a CSS preprocessor and a post-processor?

    
    A CSS preprocessor generates CSS code from source code written in a higher-level scripting language, whereas a post-processor takes existing CSS code and applies transformations or optimizations to it. In other words, a preprocessor is used during development, while a post-processor is used after development to optimize performance.

[](https://github.com/Saran-pariyar/100_Days_Of_Frontend_Interview_Questions#day-30)

146. ### What are some common mistakes that developers make when writing CSS, and how do you avoid them?
    
    [](https://github.com/Saran-pariyar/100_Days_Of_Frontend_Interview_Questions#what-are-some-common-mistakes-that-developers-make-when-writing-css-and-how-do-you-avoid-them)
    
    Common mistakes in CSS include over-reliance on frameworks, lack of organization, and using non-semantic HTML
    
147. ### How do you balance the need for visual aesthetics with the need for website or application functionality?
    
    [](https://github.com/Saran-pariyar/100_Days_Of_Frontend_Interview_Questions#how-do-you-balance-the-need-for-visual-aesthetics-with-the-need-for-website-or-application-functionality)
    
    I balance the need for visual aesthetics with the need for functionality by designing ``with the user in mind, testing designs with real users, and incorporating feedback and data into the design process.
    
148. ### How do you ensure that your CSS is optimized for search engine optimization (SEO)?
    
    [](https://github.com/Saran-pariyar/100_Days_Of_Frontend_Interview_Questions#how-do-you-ensure-that-your-css-is-optimized-for-search-engine-optimization-seo)
    
    We can ensure CSS is optimized for SEO by minimizing code bloat to improve load time, use relevant class names, avoiding inline styles, etc.

We can ensure that our CSS is scalable and maintainable for large project by:
    
    1. Using proper naming convection for ID and classes.
    2. Using preprocessor like sass, less, etc.
    3. Using performance enhancing techniques like lazy-loading, etc.``