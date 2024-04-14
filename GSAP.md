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


