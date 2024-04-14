##### useGSAP 
```tsx
//register GSAP plugin in App.tsx
gsap.registerPlugin(useGSAP);
```

```tsx
useGSAP( () => {
 // selector text or ref.current
 gsap.to(".box", { rotation: "+=360", duration: 3 });
  }, config ) //defaults to '[]'

//config 
{ dependencies: [endX], scope: containerRef, revertOnUpdate: true}
[endX] //or just dep_array
```
- can be used in SRCs, uses `useIsomorphicLayoutEffect` 
- `revertOnUpdate: true` reverts GSAP related objects on dep_array change & unmount. Default : only on unmount

##### Cleanups
animation AFTER useGSAP() executes (mount) isn't added to `gsap.context` so it won't get cleaned up. To cleanup, 2 ways
```tsx
//way 1
const { contextSafe } = useGSAP({scope: divRef})
const onClick = contextSafe(() => {
 gsap.to(".good", {rotation: 180}); //adds fn to gsap.context
});

// way 2 (for attaching listeners)
useGSAP((context, contextSafe) => {
  const handler = contextSafe( /* fn */ );
  ref.current.addEventListener("click", handler);
  return () => { // <-- cleanup
    ref.current.removeEventListener("click", handler);
  }
})
```

##### TimeLines
To avoid re-creating timelines on render, useRef
```tsx
useGSAP(() => {
    ref.current = gsap.timeline().to(/*code*/).to(/*code*/);
  }, { scope: container });

//we can safely manipulate timeline
curr = ref.current
const toggleTimeline = contextSafe(() => {
    curr.reversed(!curr.reversed())
});
```

##### QuickTo
For constantly firing events
```jsx
const g =  useGSAP(() => {
    xTo.current = gsap.quickTo(".flair", 'x', {duration: 0.8, ease: "power3"})
    yTo.current = gsap.quickTo(".flair", 'y', {duration: 0.8, ease: "power3"})
  },{ scope: app } );

const moveShape = g.contextSafe((e) => {
    xTo.current(e.clientX);
    yTo.current(e.clientY);
});

  return (
    <div className="app" ref={app} onMouseMove={(e) => moveShape(e)} >
      //code
    </div>
  );
}
```

## Controls

A single animation is called a 'tween'. Four types -> `to, from, fromTo, set`

```js
let tween = gsap.to("#logo", {duration: 1, x: 100});
tween.pause();
tween.resume();
tween.reverse();
tween.seek(0.5); //jump to 50%
tween.progress(0.25) //jump to 25% of so far
tween.timeScale(0.5) //halves speed, really good for complex animations
tween.kill() //stop and make it garbaged

//enter-leave animations
$on('#box', "mouseenter", () => tl.timeScale(1).play());
$on('#box', "mouseleave", () => tl.timeScale(3).reverse());
```

## Animatables
##### CSS properties
- `x`
- `scale rotation skew opacity`
- `autoAlpha (for opacity & visibility)`
- `backgroundColor`
! filter and boxShadow are CPU-intensive
##### Special Properties
- `duration ease stagger`
- `repeatDelay delay`
- `repeat yoyo`
- `onComplete onUpdate`
##### SVG 
width, height, fill, stroke, cx, opacity, viewBox via `attr` object
##### Objects / Canvas
```js
const position = { x: 0, y: 0 };
function draw() {
  ctx.clearRect(0, 0, 300, 300); //ctx = $('#canvax').getContext('2d')
  ctx.fillRect(position.x, position.y, 100, 100);
}
//animate rect on canvas
gsap.to(position, { 
  x: 200, y: 200, duration: 4, onUpdate: draw 
});
```
##### Values
```js
x: 200, //px
x: "+=200" // relative values
x: '40vw'
x: () => window.innerWidth / 2, 
rotation: 360 // deg
rotation: "1.25rad" 
```

## Easing
`power1 power2 power3 power4 expoScale
`back bounce circ elastic expo rough`
`sine steps slow`
`CustomEase`

Can use `in out inOut` for most e.g. `bounce.out`
```js
ease: "power1.in" // start slow and end faster, like a heavy object falling
ease: "power1.out" // start fast and end slower, BEST for UI transtion

ease: "power1.inOut" // start slow and end slow, like a car accelerating and decelerating
```

## Staggers
If a tween has multiple targets, you can add delay between start of each animation
```js
gsap.to(".box", { //querySelectorAll
  duration: 1, rotation: 360, opacity: 1, delay: 0.5, stagger: 0.2,
  ease: "sine.out", 
  force3D: true
});

$all(".box").forEach(box => { 
	box.onclick =  () => gsap.to(".box", {
      duration: 0.5, opacity: 0, y: -100, stagger: 0.1,
      ease: "back.in"
    })
});
```

stagger can be number, object, fn
##### Grid staggers
```js
gsap.to(".box", {
  //props
  stagger: {
    grid: [3,6], //[row, col] or 'auto' (responsive)
    from: "center", //"start", "edges", "random", "end", 4 (5th ele)
    ease: "power3.inOut",
    amount: 1, //if 100 ele, start time between each is 1/100 s
    each: 0.01, //OR explicit amount of each
    axis: 'x' //default both
    // Repeat / Yoyo / Callbacks to fire them for each ele
  }
});
```
If elements aren't arranged in a uniform grid, use [distributeByPosition()](https://codepen.io/GreenSock/pen/gyWrPO?editors=0010)
`grid: "auto"` doesn't adapt if layout change while animating

```js
gsap.to(".box", {
  y: 100,
  stagger: function (index, node, nodeList) {
    return index * 0.1; //delay of node from start of animation
  },
});
```

## Timelines
To chain animations
```js
const tl = gsap.timeline({defaults : defaultConfigforAll })
tl.to(".green", config1).to(".purple", config2).to(".orange", config3)

//position param
tl.to(sel, config, position)
5 //5s after timeline begins
"<" //at start of previous
"<25%" ">-75%" //25% into prev's start
"+=4" "+=50%" //after previous
"-=3" "-=37%" //overlap previous from end

// += / -= is relative to inserted anime's duration
// < / > is relative to prev's
```

##### Labels
```js
tl.to("#green", co1)
  //add blueGreenSpin label
  .add("blueGreenSpin", "+=1")
  .to("#blue", co2, "blueGreenSpin")
  .to("#orange", co3, "blueGreenSpin+=0.5") //0.5s after label
```

## Callbacks

All tweens and timelines have these callbacks:
- **onComplete**: 
- **onStart**: 
- **onUpdate**: every frame while the animation is active
- **onRepeat**: each time the animation repeats.
- **onReverseComplete**: reached its beginning again when reversed.

```js
gsap.timeline({onComplete: tlComplete})
```

