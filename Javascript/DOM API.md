```js
div.scrollIntoView (  
	behavior: 'smooth',
    block: 'nearest',
    inline: 'center'
)
```


## IntersectionObserver

*asyncly* observes if element is *intersecting* with its container or viewed

```jsx
callback(entries, observor) { 
// do something when !entry.isIntersecting
}
obs.disconnect() //unbobserve all
```