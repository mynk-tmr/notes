---

---

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
- `isComposing` : **true** if event is fired within a composition session

---
#### Input-Related Events

Event targets :- `<input> <textarea> <select>` and any element with `contenteditable` or `designmode` ON.

`focusin` `focusout` element or any of its descandants gain or lose focus.
`focus` `blur` Do not bubble. All focus events are **non-cancelable**
`beforeinput` fired before value is changed and is **cancelable.** Only for text-related.
`input` fired after every change & **isn’t cancelable.** Applies to all controls.
`change` fired after blur/`Enter` /select & if value has changed.
`cut, copy, paste` should always be **cancelled**.

Event Properties
- _input_ and _beforeinput_
    - `data` : single inserted char ; null for anything else.
    - `dataTransfer` : object representing inserted data. Has **get, set & clear** methods.
    - `inputType` : insertText, deleteContentBackward, insertFromPaste, formatBold
    - `isComposing`
- _clipboard-related_
    - `event.clipboardData` gives access to clipboard.
        - `getData('text/plain')` `setData('text/plain', str)`
        - `dataTransfer`

----
#### Load events

**document.body**
- `DOMContentLoaded` – DOM built but resources are yet to be loaded. `load` happens when resources loaded
- `resize` —  window is resized.
- `beforeunload` — before page close ; shows confirm dialog. In handler, prevent default and set event.returnValue = '"';

Any element with resources e.g. img
`load` — element’s resource was successfully loaded.
`error` — element’s resource failed to load, or can't be used

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
div.exitPointerLock();
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

```js
//zoom using 'wheel' event
scale += event.deltaY * -0.01; //wheel down zooms
scale = Math.min(Math.max(0.125, scale), 4); //limit zoom
div.style.transform = `scale(${scale})`;

//scroll events 
'scroll', 'scrollend'
```