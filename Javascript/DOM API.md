```js
div.scrollIntoView (  
	behavior: 'smooth',
    block: 'nearest',
    inline: 'center'
)
```


## IntersectionObserver

observes if element is *intersecting* with its container or viewport
- notification is fired *twice* (enter/leave) 
- callback is executed only when threshold is crossed while intersecting
- it is **asynchronous**

```jsx
callback(entries, observor) { 
// do something when !entry.isIntersecting**
}

//setup observor
obs = new IntersectionObserver(callback, options);
obs.observe(e)

kot = obs.takeRecords()
obs.unobserve(ele)  //unobserve 1 element
obs.disconnect() //unbobserve all
```

`options` — an object with options. Default — viewport, 0, 0

- `root: div` – scroll container element
- `rootMargin: '100px'` – % or px to adjust container’s virtual size. Element “intersects” this margin. +ve : expand, -ve: shrinks
- `threshold: 0.4` – 0 to 1. Specify what % of area must be **atleast** visible during intersection. If it reduces, callback is executed.

Each entry has read-only properties

- `root` , `rootMargin` and `**thresholds`** which is array of all thresholds for given entry. (1 obs = array contains 1 threshold)
- `**isIntersecting**` : describe whether target is going into (**********true)********** or going out of ****(false)**** intersecting state.
- `target` : element which fired notification
- `time` : time (ms) at which intersection was recored, starting from DOM creation
- `intersectionRatio` : ratio of target’s intersected area to its total area