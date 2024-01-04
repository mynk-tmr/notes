```js
div.scrollIntoView (  
	behavior: 'smooth',
    block: 'nearest',
    inline: 'center'
)
```


## IntersectionObserver

*asyncly* observes if element is *intersecting* with its container or viewport
- notification is fired *twice* (enter/leave) 
- callback is executed only when threshold is crossed while intersecting

```jsx
callback(entries, observor) { 
// do something when !entry.isIntersecting
}
obs.disconnect() //unbobserve all
```

Each entry has read-only properties

- `root` , `rootMargin` and `**thresholds`** which is array of all thresholds for given entry. (1 obs = array contains 1 threshold)
- `**isIntersecting**` : describe whether target is going into (**********true)********** or going out of ****(false)**** intersecting state.
- `target` : element which fired notification
- `time` : time (ms) at which intersection was recored, starting from DOM creation
- `intersectionRatio` : ratio of target’s intersected area to its total area