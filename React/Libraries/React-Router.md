https://github.com/remix-run/react-router/tree/dev/examples

3 main jobs of React Router:
- Subscribing and manipulating the history stack
- Matching the URL to your routes
- Rendering a nested UI from route matches

## Route

Route couples URL segments to components, loaders and data mutations. A `route` is 1 object with keys to declare coupling info to React. A route array is `routes` or `<Routes>` container which best-matches a nested route from current location

```jsx
<Route
  path='' //segment to match (:id dynamic , id? optional , id/* splat ( id/..)

  index  //default outlet
  element={<App/>} OR Component={App}  //coupled element
  children={routes} //nested routes
  caseSensitive 
  id  //for react
  loader={fn} action={fn}
  errorElement={<Err/>} OR ErrorBoundary={Err}
  handle={} //for match object

  lazy={() => import("./props")} //import and spread non-route matcher props [ie. not index, path, children, caseSensitive]

  shouldRevalidate={fn} //opt out of data revalidation upon action/useRevalidator/change in url or params for rendered route

/>

//getting splats
let { org, "*": splat } = params;
```

## Loaders and Actions

Route actions are the "writes" to route loader "reads". They provide a way for apps to perform data mutations via mutation submissions

```js
const store = { text: `Secret`, showText : false }

function dataloader({params}) {
	return store;
}

async function action({request, params}) {
	let data = await request.formData();
	store.showText = Boolean(data.get('show')); 
	return {res : 'ok'}
}
//loader re-runs to validate after action returns
```
## Components

```jsx
<Link 
	to='..'  //url path (relative, absolute (/) , search params)
	relative='base' // to this
	reloadDocument  //server request
	preventScrollReset // on click, not back/forth
	replace  //history.replaceState
	state={{}}  //history.state  [accessed via useLocation()]

```

```jsx
<NavLink //special link with state awareness ; extra props
	caseSensitive
	aria-current  //on active
	className, style, children  /* can be a render prop like ({ isActive, isPending, isTransitioning }) => value */
>
```

An `<Outlet/>` in parent route component renders matched child route or nothing

```jsx
<ScrollRestoration> <Root /> </ScrollRestoration>
//restore scroll when navigated back to
//create entries of key-value [5&8/3i, (x,y)]. For custom key creation, use 
getKey={(location, matches) => location.pathname} //path based keys (properly works)
```

```jsx
< Form 
	action='..' //pathname , suffix ?index to submit to index route,
	method='GET' //POST PUT PATCH DELETE ; GET is used to URLSearchParams
	navigate={false} //don't navigate, but still revalidate
	fetcherKey='my-key' //for react
	>

//has all `Link` props too
```

## HOOKS

```jsx
const {width, height} = useParams(); //extract dynamic segments

const [params, setter] = useSearchParams(); // get & set ? params
params.get('category') ; setter(newparams)

useBeforeUnload(fn, dep_arr)
useLocation() //get window.location
useNavigate() //get history.go(), second arg takes config obj of keys -- replace, state, preventScrollReset, relative

const errors = useActionData() //return of action fn
const ctx = useOutletContext()  // <Outlet value={ctx} />
const rev = useRevalidator()  //rev.revalidate() 
```

Show blocker UI to confirm navigation 
```jsx
const block = useBlocker(fn); //takes in {curr_Loc , next_Loc} and return T/F 
{block.state === 'blocked' && <ConfirmUI />} //UI can call block.proceed() or block.reset() [for yes/no]
```

To make transitions, loading animations, optimistic UI
```jsx
const navigation = useNavigation();
navigation.state; //idle, submitting (non-get), loading
navigation.location; //to
navigation.formData; //from non-GET submission, for get use location.search
navigation.json; //if useSubmit json
navigation.text; // "" text
navigation.formEncType; // ""  
navigation.formAction; // pathname where form was
navigation.formMethod; // POST etc
```

To submit form via code
```jsx
const submit = useSubmit() // sumbit(value) can be FormData, URLSearchParams, [[key, value],] OR

submit(value, { method: "post", encType: "text/plain" })
//2nd arg is same as props of `<Form/>`
```
