3 principles of redux
- a single *store* (object) holds app state
- dispatch *actions* to update state, when something happens in app. Created by factories
- define pure *reducers* for updating state based on actions

Implementing redux store
- `getState()` to access state
- `dispatch(actionfn)` for sending action objects
- `subscribe(listener)` to register and its return to unsuscribe

##### Old way
```tsx
const store = createStore(combineReducers(R1, R2))
const R1 = function(state=initMoney, action) {..}
const R2 = function(state=initCakes, action) {..}

const unsub = store.subscribe(() => {..})

//to extend redux's functionality, we use Middlewares
createStore(R, applyMiddleware(logger, asyncThunk,...))
```

