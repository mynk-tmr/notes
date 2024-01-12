
**HTML**: standard markup language of webpages ; *interpreted* & maintained by W3C and WHATWG

*How Browser works*
- Web browsers perform two main tasks: **parsing** and **rendering**.
- During **parsing**, it converts `bytes → characters → tokens → nodes`, and organise them in tree-like *data structure* DOM
- After making DOM, it **renders** DOM, ie. each node is displayed on screen

![[Pasted image 20231228224715.png]]

## Basics

Attributes define properties of an HTML element. They are _case-insensitive_

Types of elements
1. *non-replaced* — doesn’t replace content. Have open+close tags.
2. *replaced* — replaced by non-text objects. Can be void or with content. e.g. `<input>` , `<hr>`
3. *void* — self-closing elements.

Types of attributes
1. *Global*: can be applied to all elements
2. *i8n*:  `lang` and `dir`.
3. *Generic*: attach additional info to node. E.g. `data-*`
4. *Event* : allow `events` to trigger actions. e.g. `onclick="alert(event.type + this.src)"`

**Whitespace** 
- ignored when it appears on screen edge & outside elements.
- multiple whitespaces during rendering are collapsed into one
- parsing keeps whitespaces intact — `innerHTML` object

**Entitites —** to display special characters. Can be given in 3 formats — word, unicode (#) or hexcode (#x)

```html
rupee — &#8377;
letter S — &#83; or &#x53;
&amp; &lt; &gt; &quot; &apos; &nbsp; 
&ne; (≠) &asymp; (≈) &ndash; (-) &mdash; (--)
&copy; &reg; &trade; &deg; (°)
```

## TAGS

`<!doctype html>` specify mime as html5, null element
`<html lang='en'>` root element
`<head>` metadata + links to external resources
`<title>` tab, history, search-engine, OG cards
`<noscript>` content if script disabled
`<base href target>` set base for reference links & default target
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
- rec: insert content with `mark::before` for a11y

##### Code text

- `<pre>` : *block* text; preserves formatting ; must be nested inside `<figure>` and describe with `<figcaption>`
- `<code>` — generic inline code, usually wrapped in `<pre>`
- `<var>` — for variable names beside non-code text
- `<kbd>` — keyboard inputs
- `<samp>` — sample output of `<code>`
- `<output>` — result of a calc from `input`

##### Machine-readable info
```html
<data value="1000"> Ek Hazaar </data>
<time datetime="13:30"> sawa ek baje </time>

Other values
<time datetime="2013-10-19" />
<time datetime="10-19" /> 
<time datetime="2023-W15" />
<time datetime="8:00+5:30" />

durations
<time datetime="P2D" />
<time datetime="PT15H39M" />
```

##### Lists
- all **Block** (menu, ul, ol, li, `dl, dt, dd`)
- description lists for QnA, defn, glossary, metadata display
- you can give many dd for 1 dt.
- Attributes
	- `reversed`
	- `type` : for ol, `a` `A` `i` `I` `1` ; use `start` to specify diff start point

## Anchor tags

- `href="tel:"` `href="sms:"`
- `href="mailto:<addr>?subject=Shopping&body=Buy%20cooker"`
- `hreflang="pt-BR"` to specify language of linked URL
- `ping` a _space-separated_ list of URLs, used for tracking users. Sends POST requests to given list.

Create download link
```html
<a href="path/to/file" download="YakuzaLAD">Click me</a>
```

Security options
```html
<a rel="strict-origin"> tiny info if https is used</a>
<a rel="noopener noreferrer">No info</a>
```

Fragments
```html
<a href='mysite/#id'>Document fragment</a>
<a target='blank' rel='noopener' href='example.com#:~:text=tomato'> Text Fragment</a>

css matcher -> ::target-text
```

Button link
```html
<button onclick='location.href = "url"'>
```

Best Practices —
- Good Link : [Download Firefox]
- Good SEO : [Contact Logitech Solutions here]
- Good Descriptions : [AoT S9/E3 (1080p, 1.4GB)]

## Images

```html
<img src alt height width loading='lazy'>
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

*Best practices + SEO*
- properly name image files [dinosaur.jpg]
- use relative urls or CDNs
- always give width, height — prevents layout shifts
- avoid wrapping directly with <a/>, use div first
- maintain aspect ratio always

**Image hitmaps**
```html
<img src="java.png" usemap="#hitme" />
<map name="hitme">
	<area shape="circle" coords="20, 25, 5" href="page-2.html" />
	<area shape="rect" coords="10, 5, 20, 15" href="page-3.html" />
	<area shape="poly" coords="0, 10, 20, 15, 30, 30" href="page-3.html" />
	<area shape="default" href="page-4.html" /> <!-- area left-out -->
</map>
```

## Container elements

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
<dialog></dialog>
```
- form elements can close it with `method=”dialog” or formmethod=”dialog” [for submit]`
- `::backdrop` — style backdrop
- always use `autofocus` on dialog or element required to close dialog
- API : `showModal() show() close()`

## Embeds

##### Video - Audio
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

##### Inline frame `<iframe>`
```jsx
<iframe sandbox allowfullscreen loading='lazy'/>
//use like <video>
```
- embed media from 3rd party sites
- To reduce page load time, _set src by js_ after main content is done loading

> Google popular services you can embed.

Others -> `svg` `object` `math`

## Metadata tags

Types
- _pragma_: with http-equiv
- _named_: with name
- _unofficials_ : opengraph

Notes
 - `http-equiv/name` defines type, `content` specifies value
 - use only 1 of a kind ; unless legacy support

**`<meta charset='utf-8'>`**
- sets *char encoding*, utf-8 extends 7bit ascii to 256 characters
- html5 requires utf-8
- non-ASCII characters are replaced with %xx in URL, where x is **hex-digit** e.g Space = %20
- inherited by `js` and `css` files too

Refresh site
```jsx
<meta http-equiv='refresh' content='60; url'>
```

set CSP of webpage
```jsx
<meta http-equiv="content-security-policy" content="default-src https:" />
```

`viewport` — helps site responsiveness ; else site scales on smaller screens.
```html
<meta name="viewport" content="width=device-width, initial-scale=1, user-scalable=1" />
```

`author` — specify author. Used by CMS to provide visitors your info (if available).
```html
<meta name="author" content="Gus Fring"/>
```

`description` — SEO ranking, search results, in bookmarks
```html
<meta name="description" content="Offical Website of Github" />
```

`robots` — ask search engines not to index site
```html
<meta name="robots" content="noindex, nofollow" />
```

`theme-color`: for PWAs, browser UI
```html
<meta name="theme-color" content="#226DAA" media="<query>"/>
```

`property`: opengraph cards
```html
<meta property="og:title" content="StyleX" />
<meta property="og:image" content="path/to/img" />
<meta property="og:description" content="blah blah" />
```

## Link Tag

**`<link rel href>`**
- Used to create r/ship b/w html doc and external resources.
- if links meta-data resources, use in `<head>` ; if links performance-related, use in `<body>`
- other `rel` support -> `a area form`

#### Browser Hints

To improve page performance
```html
<link rel="prefetch" href="/public/app.08343a72.js" as="script">
<link crossorigin >
```

- **preload** – load content required for intial render. Must be used within 3s.
- **prefetch** - load content required to render next page
- **preconnect** - establish a [server connection](https://www.debugbear.com/blog/http-server-connections) without loading a specific resource yet
- **dns-prefetch** - only get IP of cross-origin source of resources


**Favicons** `link rel=”icon”`
- defaults to `favicon.ico` in root ; use same-color scheme for them
- if blocked by CSP, google
```html
<link rel="icon" type="image/png" href="con.png" sizes="16x16 32x32 48x48"/>

<!-- widthxheight
use sizes="any" for scalable icons -->
```

**Canonical links** `link rel='canonical' href=''`
- for search engine to know canon site out of many versions

#### Alternate sites

_translations_ with `hreflang`
```html
<link rel="alternate" href="path/to/fr/>" hreflang="fr-FR" />
<link rel="alternate" href="path/to/pt/>" hreflang="pt-BR" />
```

 _formats_ with `type="application/"
```html
<link rel="alternate" href="path/to/rss/q='tech'" type="application/rss-xml"> 
```

*alt stylesheets*
```html
<link rel="alternate stylesheet" title="Dark Mode" href="darkmode.css" />
```

---
### Script loading

- script loading & execution blocks parsing, so fix needed.
	- `async`: block & execute after complete download, and before window's `onload` event. Use for *critical* ones like analytics
	- `defer`: execute after page parsing, before window's `DOMcontentLoaded`. Less *critical* like videos
- `import('path').then(analytics => analytics.init())` 
- for 3rd party scripts, `preconnect` & `dns-prefetch` can be used
- lazy loading with `Intersection Observer`
- using *CDNs*
- cache script with *webworker*

```js
function loadJSAsync(url) {
	let script = document.createElement('script');
	script.src = url;  
	script.async = true;
	document.body.appendChild(script);
}
```

## Tables

```jsx
<table>
  <caption>Description of contents (A11y)</caption>
  <colgroup>
    <!--many <col /> for group-styling --> 
  </colgroup>
  <thead>
    <!-- 1 <tr> with many <th> -->
  </thead>
 <tfoot>
    <!-- 1 <tr> with many <td> -->
    <!-- always displayed at end -->    
  </tfoot>
  <tbody>
    <!-- many <tr> each with 1 <th> and many <td> -->
  </tbody>
</table>

<td colspan rowspan/> //to create big cells (give no.)
<th scope="row|rowgroup|column|colgroup" /> // define whether heading is for row(s)/column(s)

//To create explicit associations b/w `th(s) and td(s)`
<th id="price">  .... <td headers="price cost">
```

`tabindex` : tab-order. -1 (not tabbable), 0 (at last), 1 2 3..(ordering). Try to use only `0 & 1` and use markup to order.

__Good Table UI/UX__

Styling
  - alignment `left` for text, headings, qualitative data (phone no. etc.). 
  - `right` for qualitative no. and same decimal length
  - Combine related col/rows. Use different `font-weight & size` for combined data in cells e.g units, %change
  - `1px solid light gray` (good borders)
  - use `vertical-align : top` for large data
  - cell heights : `40px or 48px or 56px`
  - `scroll-smooth`
  - low contrast `color` & `bgcol` (zebra) in alternate rows / cols
  - `:hover` slight change 

Too many columns/rows
  - allow Resizing/Toggling col/rows
  - Sorting options
  - sticky header, footer, leftmost column
  - horizontal scroll

Table features
  - small icons/thumbnails
  - Expandable rows
  - Modals for cell (can have editing features)
  - Full screen mode

