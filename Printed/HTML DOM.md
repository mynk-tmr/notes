**HTML**: standard markup language of webpages
**Elements**
- *void* — self-closing elements.
- *non-replaced* — doesn’t replace content. Have open+close tags.
- *replaced* — replaced by non-text objects. Can be void or with content. e.g. `<input>` , `<hr>`

**Attributes** : define properties of an HTML element. Case-insensitive but **values aren't**
- *Global*: can be applied to all elements
- *i8n*:  `lang` and `dir`.
- *Generic*: attach additional info to node. E.g. `data-*`
- *Event* : allow `events` to trigger actions

**Whitespace** 
- ignored when it appears on screen edge & outside elements.
- parsing keeps whitespaces intact — `innerHTML` object. But rendering collapses them into 1

**Entitites —** to display special characters.
```html
rupee — &#8377;
letter S — &#83; or &#x53;
&amp; &lt; &gt; &quot; &apos; &nbsp; &copy
```

**Well know Tags**

`<!doctype html>` specify mimetype of document as html5, null element
`<html lang='en'>` root element
`<head>` metadata + links to external resources
`<title>` tab, history, search-engine, OG cards
`<noscript>` content if script disabled
`<base href= target= >` set base for reference links & default target
`<style media='print | tv' >` internal css

`<hr>` theme/topic shift in text
`<h2>` best heading for SEO
`<abbr title="">` abbreviations
`<dfn id=''>` term being defined by text. Used in `<p> , <dd>`
`<q / blockquote cite="url">` 
`<address>` contact details in footer. BLOCK element
`<i>` foreign or technical terms, taxonomy, official posts, thoughts, mood shifts
`<b>` draw attention, product names, keywords, lead sentences
`<u>` proper names, misspellings (wavy or dotted)
`<small>` side comments, copyright info, legal text

`<cite>` title of a cited _creative_ work (song, book, etc). Wrap immediately outside **anchor** element.
`<strong>` important word or content that doesn’t change meaning of surrounding text
`<em>` changes meaning. eg. sarcasm
`<del> <ins>` deleted, inserted info
`<s>` striked text (irrelevant)
`<mark>` to mark/highlight text ; search results; important to context
- inside quote elements to highlight things not originally emphasised by author
- insert content with `mark::before` for a11y

**Code tags**
`<pre>` : *block* text; preserves formatting ; use in `<figure>` and describe with `<figcaption>`
`<code>` — generic inline code, usually wrapped in `<pre>`
`<samp>` — sample output of `<code>`
`<var>` — for variable names beside non-code text
`<kbd>` — keyboard inputs
`<output>` — result of a calc from `input`

**Machine data
```html
<data value="1000"> Ek Hazaar </data>
<time datetime="13:30"> sawa ek baje </time>
```

**Lists**
- all **Block** (`menu, ul, ol, li, dl, dt, dd`)
- description lists for QnA, defn, glossary, metadata display
- you can give many dd for 1 dt.
- Attributes
	- `reversed`
	- `type` : for ol, `a` `A` `i` `I` `1` ; use `start` to specify diff start point

**Anchor tags**
- `href="tel:"` `href="sms:"`
- `href="mailto:<addr>?subject=Shopping&body=Buy%20cooker"`
- `ping` a _space-separated_ list of URLs, used for tracking users. Sends POST requests to given list.

```html
//create download link
<a href="path/to/file" download="YakuzaLAD">Click me</a>

//CSP
<a rel="strict-origin"> tiny info if https is used</a>
<a rel="noopener noreferrer">No info</a>

//Fragments
<a href='mysite/#id'>Document fragment</a>
<a target='blank' rel='noopener' href='example.com#:~:text=tomato'> Text Fragment</a>

css matcher -> ::target-text

//button link
<button onclick='location.href = "url"'>
```

**Images**

```html
<img src alt height width loading='lazy' decoding='async'>
```

Load diff images for diff screens
```html
<img srcset="apple.jpg 480w, banana.jpg 800w" 
	 sizes="(max-width: 600px) 480px, 800px" 
	 src="fallback.jpg" />
```

Same size, but different resolution, 480 = 320 x 1.5 dpi
```html
<img srcset="apple-320w.jpg, apple-480w.jpg 1.5x" 
	 src="fallback.jpg"/>
```

Embed alternative versions of image
```html
<picture>
  <source srcset=".." type="image/avif" media="(max-width: 600px)"  />
	<!--any no. of sources -->
  <img src="fallback.jpeg" />
</picture>
```

**Container elements**
`<header>`: for introductory info. Can be in `<body> or <section>`
`<footer>`: for site info, contact details, *quick links*
`<main>`: all content unique to web page. Only 1 allowed. Used as child of `<body>`
`<section>`: Must begin with _heading_. Group together a single piece of functionality/theme.
`<article>` : a self-sufficient block of info. E.g. blog post

`<aside>`: content not directly related to _primary flow_ of main, section or article. Can provide *glossary*, *relevant* *links*, *Ads*.
`<nav>`: site navigation links. If >1 nav, label each with *aria-labelledby* Use *id* on controls.
`<search>`: for form controls related to search/filter operations
`<template>`: holds HTML not rendered immediately on loading, but later by scripts. Access content with `$('template').content`

**Accordian**; uses _toggle_ event (’open’ ‘close’)
```jsx
<details>  
	<summary>Click to expand</summary>  
	<table>Content ..</table>
</details>

// style only open state
details[open] > table
```

**Figure**
```html
<figure >
	<!--tables, images, code/pre , audio/video  -->
	<figcaption>I am fig</figcaption>
</figure>
```

**Dialog**
```jsx
<dialog> </dialog>
```
- `::backdrop` — style backdrop
- API : `showModal() show() close()`
- close dialog on submit : `<form method='dialog_id>` OR `<input type='submit' formmethod='id'/>

**Audio / Video Embeds**
```jsx
<video src="path" controls autoplay loop muted poster="thumbnail.jpeg"
preload="auto | none | metadata">
	<p>Fallback msg</p>
</video>

//using with source (shift src)
<video>
	<source src=".." type="video/mp4" />
		<!--any no. of sources -->
</video>

<audio controls/> //like video but no width, height, poster

//common mimes
mp4, mp3, webm, ogg
```

**Subtitles** (`.vtt` file)
```jsx
//style with video::cue {}
<video>
	<track kind='subtitles | caption' src='path/to' srclang='en' 
	label='English' default/>
	<!--any no. of tracks ; only 1 default -->
</video>
```

**Inline frames**
```jsx
<iframe sandbox allowfullscreen loading='lazy'/>
//use like <video>
```
- embed media from 3rd party sites
- To reduce page load time, **set src via JS** after main content is done loading

**SVG embeds**
makes page less cacheable & can delay loading of other elements
```xml
<svg xmlns="..." viewbox="0 0 100 100">
  <rect class="one" fill="blue"/>
  <text> Demo SVG </text>
  Alt text for svg
</svg>
```

**Metadata element
* represents metadata that cannot be represented by other elements
* each have a type attribute and `content` that specifies values for element
* Types
	- **pragma** that use `http-equiv`
	- **named** that use `name`
	- **charset** that use `charset`
	- **community** : OG cards `property`

**`<meta charset='utf-8'>`**
- sets char encoding. Inherited by `js` and `css` files too. HTML5 requires UTF-8
- utf-8 extends 7bit ascii to 256 characters. Non-ASCII characters are replaced with **%xx** in URL, where x is **hex-digit** e.g Space = %20

**Well known metadata**
```html
<meta http-equiv='refresh' content='60; url'> //refreshes site
<meta http-equiv="content-security-policy" content="default-src https:" /> // set CSP of webpage

<meta name="author" content="Gus Fring"/> //specify author. Used by CMS to provide visitors your info (if available).

<meta name="description" content="Offical Website of Github" /> //SEO ranking, search results, in bookmarks

<meta name="robots" content="noindex, nofollow" /> //ask search engines not to index site

<meta name="theme-color" content="#226DAA" media="<query>"/> //PWAs ; browser UI
```

**`property`**: opengraph cards
```html
<meta property="og:title" content="StyleX" />
<meta property="og:image" content="path/to/img" />
<meta property="og:description" content="blah blah" />
```

**`viewport`** — helps site responsiveness. Sets virtual viewport explicitly and disable default shrink-to-fit behavior on smaller screens
```html
<meta name="viewport" content="width=device-width, initial-scale=1" />

- width,height : set viewport W/H
- initial-scale : zoom level on load (0.1 to 10)
- minimum-scale or maximum-scale : max/min range 
- user-scalable : 1 or 0
```

**Virtual viewport** : area occupied by `<body>` of a webpage. It is split into
- *layout* viewport : contains all elements which may or not appear on screen
- *visual* viewport : part of page currently shown (excludes user-widgets, url bars ie., things we can't scale)

**Link element**
- Used to create r/ship b/w html doc and external resources.
- if it links meta-data resources, use in `<head>` ; if links performance-related, use in `<body>`
- Top usecases
	- to provide **browser hints** to improve page performance
	- to create **favicons**
	- to specify **canonical links** for SEO (most relevant page from a group of **duplicate** pages)

**Browser Hints**
```html
<link rel="prefetch" href="/public/app.08343a72.js" as="script" crossorigin>
```
- **preload** – load content required for intial render. Must be used within 3s.
- **prefetch** - load content required to render next page
- **preconnect** - establish a server connection without loading a specific resource yet
- **dns-prefetch** - only get IP of cross-origin source of resources

**Favicons**
```html
<link rel="icon" type="image/png" href="con.png"/>
```

**Canonical links**
```html
// translated sites
<link hreflang="fr-FR" rel="alternate" href="path" />

//rss-feed
<link type="application/rss-xml" rel="alternate" href="path"> 

//alternate stylesheet
<link rel="alternate stylesheet" title="Dark Mode" href="darkmode.css" />
```

**HTML tables** : for showing tabular data
```html
<table>
	<caption> accessibility </caption>
	<thead> <tr> many <th></th>  </tr> </thead> 
	<tbody> <tr> many TD OR (1 TH, then TDs)  </tr> </tbody> 
	<tfoot> <tr> many TDs </tr> </tfoot> 
</table>

ATTRIBUTES
colspan='2' rowspan='3' --> to create bigger cells
```

**Styling tables**
```css
table, td, th {
  border-collapse: collapse; /* collapse border */
  border-spacing: 5px;
  padding: 1rem; /* cell padding */
}

/* even-odd row styling */
tr:nth-child(even) {} tr:nth-child(odd) {} 
```

**Scalable Vector Graphics**
* SVG is a XML based markup language for vector images. Raster images are pixel based.
* Pros:
	  - can scale without quality loss
	  - text in svg remains accessible (SEO)
	  - each image component can be styled differently via CSS/JS
  - **SVG Viewbox** : viewable area coords (<= svg's width * height)
  - SVG elements : `rect` `circle` `line` `polygon` `text` `tspan` `polylines`
  - SVG length/coordinate attributes: `x` `y` `width` `height` `cx` `cy` `r` `x1` `x2` `y1` `y2` `points` 
  - attribute values are always in **px** (integer)

**CSS / JS and SVGs**
* SVG's attributes => CSS properties ; use css `--variables` to manipulate them via JS
* Text : `color` `font-*` `text-decoration` `letter-spacing` `word-spacing` `overflow` 
* Display: : `stroke` (border-color) `fill` (bgcol) `stroke-width` `display` `opacity` `visibility` `mask` 
* Animation: `transition` `animation` ... all position attributes

**Form element**
```html
<form action="/path" method="post" enctype="multipart/form-data">

//action : URL to submit
//method: HTTP verb
//enctype: how to encode form-data for server
```

**Enctypes**
* `application/x-www-form-urlencoded`	: Default **utf-8 encoding** (spaces => "+" , and special characters => ASCII HEX values)
* `multipart/form-data` : if user will upload a file
* `text/plain`: sends data without any encoding at all

**Fieldset** : to group related form elements
**legend**: used with fieldset as group heading

**Text-area**
```html
<textarea rows='8' cols='10' resize='none'> Placeholder</textarea>
```

**Select**
```html
<select multiple> 
  <optgroup label='subheading'>
    <option value='1'>Banana</option>
```

**Radio / Checkbox**
```html
<input type="checkbox | radio" name="color" value="yellow" checked />

//if value is skipped, use "on" value
//encoded: color=yellow [for radio]  AND 
```

**Input fields**
```html
<input type='url| text | password | email | hidden | search | tel'> 
<input type='image' src='pp.jpg' alt='click'> // sends coords in img
<input type='file' accept='audio/*, .json' multiple>
<input type='number' min='3' max='33' step='2 | any'> //any = float
<input type='date | time | datetime-local | month | year'/>

<input type='range' min max step style="appearance: slider-vertical" list='dataId'>
//list for datalist tag (uses option's value)

//attributes
<input spellcheck='true' maxlength='30' minlength pattern >
```

**Submit/Reset**
```html
<input type='submit' formaction='Post_to_url'   /> //to override form's action
<input type='reset'/>

```

**Datalist tag**
```html
<input type='search' list='dataId'> //search dropdown
<datalist id='dataId'>
  <option label='low'>⭐</option> 
  <option label='mid'>⭐⭐</option>
  <option label='high'>⭐⭐⭐</option>
</datalist>
```

**Inline Progress**
```html
<progress value="32" max="100">32% (fallback) </progress>
```

**Other attributes**
`autocomplete` `autofocus` `required`
`disabled` : disable the control (NOT validated or submitted)
`readonly`: validated and submitted
`form='form_id'` : associate element with a form

**Form element pseudoselectors**
```css
:focus :blur :enabled :disabled :read-only :read-write :required :optional :blank

:valid :invalid :auto-fill :user-invalid /* ignore browser invalid */

:in-range :out-of-range :checked :default

input::placeholder
input[type='file']::-webkit-file-upload-button
```

**Constraint Validation API** 
* helps in validating form fields using JS. Done on element or form level
* supports `button, fieldset, input, output, select, textarea & form`
* to disable default validation, use `novalidate` attribute on `<form>` (needed for JS-based)
* `form.submit()` submits without validation ; use `submitBtn.click()` instead for validation
* Inline validation: 
	* done by listening to events `input` `change` `focus` `blur`
	* Or `invalid` event (bubble:*false*) viz. triggered on every invalid field. 
* Methods for form fields
	* `checkValidity()` : return true/false ; `reportValidity()` : checks and shows message on UI
	* `setCustomValidity(msg)` : custom message on UI 
* Field DOM properties
	- `validationMessage` : error msg
	- `validity` : an object with **bool** properties e.g. `ele.validity.rangeOverflow`. These are - 
	-  patternMismatch , stepMismatch, typeMismatch (e.g. ❌ url)
	- tooLong, tooShort, valueMissing, badInput (unreadable)
	- rangeOverflow, rangeUnderflow
	- valid
	- customError (`true` if custom msg was set)

```jsx
myform.addEventListener('submit', (e)=>{
	e.preventDefault(); //don't submit
	//rest of code
	form.submit(); // if all checks were valid
});
```

**Input-Related Events**
* Event targets :- `<input> <textarea> <select>` and any element with `contenteditable` 
* Events
	- `focusin` `focusout` element or any of its descandants gain or lose focus.
	- `focus` `blur` Do not bubble. All focus events are **non-cancelable**
	- `beforeinput` fired before value is changed and is **cancelable.** Only for text-related.
	- `input` fired after every change & **isn’t cancelable.** Applies to all controls.
	- `change` fired after blur/ Enter /select & if value has changed.
	- `cut, copy, paste` should always be **cancelled**.
* **input and beforeinput**
    - `e.data` : single inserted char ; null for anything else.
    - `e.dataTransfer` : object representing inserted data. Has **get, set & clear** methods.
    - `e.inputType` : insertText, deleteContentBackward, insertFromPaste, formatBold
- **clipboard-related**
    - `event.clipboardData` gives access to clipboard. (let eC)
	- `eC.getData('text/plain')` `setData('text/plain', str)`
	- `eC.dataTransfer`


## DOM

DOM represents content on HTML document as a **hierarchical tree** of node objects.
- **DOM-API** exposes the functionality to change webpage’s content
- It is cross-platform and language-independent.
- It is *live data-structure* and updates when changed by JS

DOM basic objects
- `window` : global object
- `document` : property of window. Serves as **root** node.
    - its properties are `documentElement` (html) , `head` `body`

#### DOM Datatypes
```js
`NODELIST (iterable of nodes)`
children.length, children[4], children.forEach //live

`HTMLCollection - a live iterable of HTMLElements`
//returned by -Element- methods

`NamedNodeMap - a live iterable of `Attr` objects mapping element's attributes`
div.attributes.id.value //or specified 

`DOMTokenList - space separated live classes on element`
div.classList //get
```

##### Node Codes / Constants
`1 (Node.ELEMENT_NODE)`
`3 (Node.TEXT_NODE)`
`8 (Node.COMMENT_NODE)`
`9 (Node.DOCUMENT_NODE)`
`11 (Node.DOCUMENT_FRAGMENT_NODE)`

`DocumentFragment` interface is a lightweight version of `Document`. It is used to compose nodes and inserted as a whole to improve performance
```jsx
let fragment = new DocumentFragment();
fragment.append(eleList) //append to fragment
div.appendChild(fragment) //append fragment to DOM
```

##### Node v/s Element
Node is a generic name of any **object** in DOM tree.
An element is a node with a specific node type `Node.ELEMENT_NODE`. It has several subtypes like `HTMLInputElement`

##### Node Relationships
Each node has **6 relationships** (default : null)
`parentNode` `firstChild` `lastChild` `previousSibling` `nextSibling` `childNodes`
(with `*Element*` versions )
- Selecting nodes — use the most specific method, match is **case-sensitive.**
- Altering attributes — always use **get** and **set** methods
- Inserting *multiple nodes* return **undefined** or throw error if insertion fails.  But for 1 node a *reference* to node is returned

##### Altering attributes
- standard html attributes -> element properties. Some may type convert
- Boolean attributes can’t be set to false, they have to be `removeAttribute()`

```jsx
getAttribute('id'); //better than div.id
setAttribute('id', 333); //name is lowercased, string coerced
removeAttribute('id'); //No error on absent 
hasAttribute('id')

//styling 
$ div.style //inline styles object
$ div.currentStyle // computed styles object
$ getComputedStyle(div,':hover') 


div.className //returns a string of space-separated classes
div.classList //returns a domtokenlist
div.classList[2] //get 3rd class

//classlist
values() //return iterable list of classes
add('hero', 'mob');                                      
remove('hero', 'mob'); 
toggle('active');
replace('dark', 'light'); // replace dark with light
contains('hero') // false
```

## Event-Handling

Events are fired to notify **"interesting changes"** that may affect code execution. They may be user-generated (like clicks) or system generated (like low battery). 

Each event is represented by an **object** that implements **Event interface.** They can be accessed only via listeners

An **event target** is any object that can subscribe to an event. It can trigger callbacks (called eventlisteners) upon event.

Events happen in **cascade** : a compound event happens after elemental events
Event flow has 3 phases 
- Capturing phase ⬇️ — event flows down from **root** element to event **target** via branch
- Target phase 🎯 — event reaches its target element
- Bubbling phase ⬆️ — event flows up from target to root through same DOM branch

##### Event Delegation
- a technique which uses **bubbling** to handle event at a Node higher than actual node target
- Greatly reduces no. of event handlers needed, thus improving memory utilisation and performance
- wherever possible, attach listeners on `document`

##### Handler Optimisations
used for rate-limiting the execution of callback to improve performance
- Given a rapid series of events,
    - **Debouncing** ensures that callback is executed **only once** by calling it only when time difference between 2 events > max_delay
    - **Throttling** ensures that callback is executed at **regular intervals** by calling it only when time elapsed since previous callback > max_delay
- debounce — *search*
- throttling — *infinite scrolling*

3 ways to register **event-listeners :**
- HTML event attributes — `onclick='handler()'`
- property — `btn.onclick = handler`
- `.add`
    - can add any number of handlers for a **single** event.
    - works on any event target, even XMLHttpRequest

```jsx
addEventListener(type, listener, flags)
//named listeners aren't re-registered. Anonymous do

// default flags
{ capture : false, 
	passive : false, 
	once : false,    
	signal : null,  
}
```

Adding more listeners **inside a listener** doesn’t trigger them during **current phase** of flow

**Listener** can add **effects** to event 
- `preventDefault()` — prevent the browser’s default handling of event ; works only if property `cancelable=true` ; sets `defaultPrevented` to true.
- `stopPropagation()` — stops the event-flow once it has processed all its listeners on **currentTarget** ; works only if `bubbles=true` (default)
- `stopImmediatePropagation()` stops the event-flow. No other listeners will be called

**Passive Listeners**
cannot cancel the default handling, so the browser can start it immediately, without waiting for the listener to finish. Will work only when **all listeners** on an event path are passive.

**synthetic or custom events**
```jsx
const event = new CustomEvent("build", { detail: 'hi', bubbles: true});

//send event to element invoking listeners in appropriate order.
btn.onclick = input.dispatchEvent(event);
```

Unlike **"native" async** events, it invokes event handlers _synchronously_ and returns when all of them have executed. `false` if atleast 1 handler used `preventDefault()`, else true.

## Reference

```js
window.top //topmost window
window.parent

document.activeElement //focused
document.referrer //uri of referrer page
document.readyState //loading, loaded, completed
document.styleSheets //iterable stylesheet collection
document.title 
document.forms //links, images, etc
document.importNode(node, deep=true) //clone node+children from another doc
document.adoptNode(node) //steal node+subtree
document.createElement('p') //TextNode, Comment
document.getElementsByName('div') //LIVE nodelist
document.getSelection().toString() //selected text
```

### HTMLElement interface (live)

```js
tagName //uppercase
innerHTML textContent //markup inside vs. all textnodes in ele+children
innerText //respect css hidden

//FIND elements
$q //first matched child
$qall //static nodelist
div.closest('.book') //itself or ancestor closest
div.matches('.book') //true if this ele matched

//ASSERT
div.contains(ele) //descendant
div.hasChildNodes()
div.hasfocus() 
div.isEqualNode(ele) //equal,
div.isSameNode(ele)

//will relocate if node is in DOM
div.before(...nodes) // insert these
div.prepend(...nodes) //before first child
div.append(...nodes) //after last child
div.after(...nodes)
div.insertBefore(new, nextSibling) //append for none
div.replaceChildren(...nodes) 
div.replaceChild(new, old) 
div.removeChild(child)
div.remove() //remove itself

//helpers
div.blur() div.focus() div.click()
div.cloneNode(deep=true) //clone with listeners (+children)
div.normalize() //fix jumbled text nodes
```

```js
div.insertAdjacentElement(pos, ele) 
div.insertAdjacentHTML(pos, markup) 
'beforebegin' 'afterbegin' // before element / first child
'beforeend' 'afterend' // after last child / element
```

---
##### Event Object properties
$ `target` — element that triggered event
$ `type` of event that was fired e.g. `‘click'`
$`currentTarget` — element on which event is currently flowing
$`detail` more info about the event
$`eventPhase` — 1 for capturing phase, 2 for target, 3 for bubbling
$`isTrusted`  — user-generated `true` and for synthetic `false`
$`timeStamp` — time(ms) when event was created

---
#### Drag events

`event.dataTransfer` stores data being dragged.
```jsx
dT.setData('text/plain', str) //text/uri-list 
dT.getData('text/plain'); //str
dT.setDragImage('hi.png', 50, 50) //cursor at (50,50) inside drag image
dT.dropEffect = 'link'; //cursor [move, copy, none] ; if none event got cancelled
[...dT.types].includes("text/html") //false
```

---
#### Pointer Events

Better since, they are _hardware-agnostic_ and supports mouse, stylus, touch, etc.
Event flow : `down` `enter` `over` `move` `out` `leave` `up`  ; can `cancel` anywhere
Others : `got/lost pointercapture` 
Mouse-only : `click` `dblclick` ; `contextmenu` right-click.
Notes
- preventDefault  `down`
- for `move` to work, prevent `ondragstart` and add `touch-action: none` on element
- `enter/leave` do not bubble
- `move` fires continously 
##### Event properties
- `pointerId` given to **each** finger, stylus, etc during a flow. Starts from 1.
- `pointerType` (mouse, pen, touch, etc.).
- `isPrimary` true if primary for given **type**
- `button` — 0 to 5 {L,M,R, X1, X2, eraser}
- `relatedTarget` — element where pointer was previously
- Modifier keys (**true** if pressed) : `altKey`, `ctrlKey`, `shiftKey` , `metaKey` (ctrl of Mac)
- `clientX` `clientY` — (x,y) coords of pointer at event [viewport relative]
- `screenX` `screenY` — for combined screens (many monitors)
- `pageX` `pageY` — document relative, takes scrolled amount into account
- `offsetX` `offsetY` — target relative

---
#### Keyboard Events

Shouldn’t be used for inputs. **`Fn key`**— no keyboard event
Types
- `keydown` – auto-repeats if the key is pressed for long
- `keyup` – release [once only]
Properties
- `code` (location) : `"KeyA"`, `"Digit0"` , `"Enter"`
- `key` (interpretation) : `"A"`, `"a"` or same as `code` for non-character keys
- Modifier keys `altKey`, `ctrlKey`, `shiftKey` , `metaKey`
- `location` : which **version** of key pressed. [0,1,2,3] → [single key, left one, right one, numpad]
- `repeat` : **true** if long key press

----
#### Load events

**document.body**
- `DOMContentLoaded` – DOM built but resources are yet to be loaded. `load` happens when resources loaded
- `resize` —  window is resized.
- `beforeunload` — before page close ; shows confirm dialog. In handler, prevent default and set event.returnValue = '"';
Any **element** with resources e.g. img `load` — success `error` 

---
**Full screen and PIP mode**

```jsx
document.fullscreenEnabled //true
document.fullscreenElement; // get element currently in fs mode
div.requestFullscreen(); //put div is fs-mode
div.exitFullscreen();

// Similarly for PIP
pictureInPicture

//events are sent to element and document that owns element
'fullscreenchange'  /* ele enter/left FS */
'fullscreenerror'  /*browser cannot switch to FS */
```

**Pointer Lock**
```jsx
div.requestPointerLock(); // lock pointer, set div as target for pointer events
document.exitPointerLock(); //on doc
document.pointerlockElement //read only getter
'pointerlockchange' // event associated
```

---
#### Dimensions

```js
window.
outerHeight. outerWidth //window
.innerHeight .innerWidth //viewport 

div. //read only
offsetHeight .offsetWidth // border box + scrollbar
.clientHeight  .clientWidth // viewable padding box
.scrollHeight  .scrollWidth // full padding box
.offsetTop .offsetLeft  // (x,y) w.r.t parent

div.getBoundingClientRect()  // position w.r.t viewport
// contains top, bottom, left, right
```

---
#### Scroll manipulation

```jsx
window
.scrollX .scrollY  //amount scrolled
.scrollTo(0, 10) .scrollBy(0, 10)  // left,top [No -ve allowed]

//1 page scrolling
window.scrollTo({top: 100, left: 100, behavior: "smooth"})

//get and set scroll value
div.scrollTop, div.scrollLeft // min 0 , max = scrollHeight - clientHeight

//scroll div into container’s visible area
div.scrollIntoView ({
	behavior: 'smooth',
    block: 'nearest', //align to y-edge (start, center, end)
    inline: 'center' //align to x-edge ("")
})
```
