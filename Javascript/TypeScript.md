## Decorators

To customize classes and their members in a reusable way. Can be used on properties, getters, setters. Classes themselves can be decorated for things like subclassing and registration.

```ts
function log(fn: any, context: ClassMethodDecoratorContext) {
    const fnName = String(context.name);
    return function(this: any, ...args: any[]) {
        console.log("Entered "+fnName)
        return fn.call(this, args)
    }
}

//usage, runs before any other field initialisation, greet --> replaced by return of @log
@log
greet() {  /* code */ }
```

Binding methods ..
```ts
function bound(fn: any, context: ClassMethodDecoratorContext) {
    const n = context.name;
    context.addInitializer(function () { //builtin ts method
        this[n] = this[n].bind(this);
    });
}
```

 2 decorators can be stacked. Decorators can _return_ decorator functions.
```less
@bound @log greet() { } //log runs first
```


```less
// ✅ allowed
@register export default class Foo {
    // ...
}
export default @register class Bar {
    // ...
}
```

---
##### `const`
Tells compiler to infer most narrow type
```ts
[k, v] as const // k : Mykey , v : GoodValueType

//TS 5.0, const modifier
declare function fnGood<const T extends readonly string[]>(args: T): void;
```

`const` modifier doesn’t _reject_ mutable values, and doesn’t require immutable constraints.

##### `enums`
```ts
enum E { A, B} //E.A = 0 E.B = 1
type X = E.A | E.B

//to change default numbering
enum E {A=5, B} //5,6

//fn usage
function isPrimaryColor(c: Color): c is PrimaryColor { ...}

//in TS 5, enums are considered union types
type E = A | B
```

##### `declare namespace`
To describe types or values accessed by dotted notation.
```ts
declare namespace Toffee {
	function greet(s: string) : string;
	let id : number ;
	interface Packet {
		//code
	}
}

//using with . notation
const mygreet : Toffee.greet = ;
const x : Toffee.Packet.color
```

## Utility Types

To facilitate common type transformations.
```ts
//to mock async wait
type A = Awaited<Promise<string>>

//to make a type with ALL properties of type Todo
const todo: Partial<Todo> = {text : 'hello' } //optional
Required<Todo> //required
Readonly<Todo> //good for Object.freeze()

//to define object type of specific Keys and their values
Record<Keys, Type>;
type CatName = "miffy" | "boris" | "mordred"; 
type CatInfo = { age: number;    breed: string;  }  
const cats: Record<CatName, CatInfo> = {    
	miffy: { age: 10, breed: "Persian" }
}

//to make a type by picking keys from Type
Pick<Type, Keys>;
type Todo3 = Pick<Todo, "title" | "completed"> //picked them

//to omit
Omit<Type, Keys>;
type Todo2 = Omit<Todo, "done" | "checked"> //omitted them

//to exclude members from union
Exclude<union, members>; //any union item assignable to members is deleted
type T2 = Exclude<string | number | (() => void), Function> //T2 = string | number

//to do opposite of exclude
Extract<union, members>;
type T2 = Extract<string | (() => void), Function> //T2 = () => void

//Constructs a type by excluding `null` and `undefined`
Nonnullable<T>;

//Constructs a type from a function type
Parameters<FnType>; //from params, if multiple -> returns tuple
ReturnType<FnType>; //from return
InstanceType<FnType>; //type of object returned by constructor Fn
ThisParameter<FnType>; //type of this (or unknown)
OmitThisParameter<fnType>;
```

For strings
```ts
UPPERCASE<strtype> ; //also lower, capitalise,
```

template literals
```ts
type Vertical = 'top' | 'bottom' | 'center'
type Horizontal = 'left' | 'right' | 'center'
type Position = Exclude<`${Vertical}-${Horizontal}`, 'center-center'>
```
## ReactTs

```tsx
//typing events
e : React.MouseEvent<HTMLButtonElement>

//css type
React.CSSProperties

//discriminated types
type UpdateAction = { type: 'up' | 'down' ; payload : number}
type ResetAction = {type: 'reset' }
type ReducerAction = UpdateAction | ResetAction

//non-null assertion to skip optional chaining
useRef<HTMLDivElement>(null!) 

//with class components
class Button extends Component<ButtonProps, ButtonState> {...}

//restricting props with never
type MyProp = { children? : never}

//type safety, ...rest support for wrapper components
//we can use Omit to get better type
type ButtonProp = {variant : 'red' | 'blue' } & React.ComponentProps<'button'>

//extracting props of a type
props : React.ComponentProps<typeof Suspense>

//generic props
const List = <T extends {},>(props : ListProps<T>) => {...};
```

Isomorphic component
```ts
const Text = <E extends React.ElementType = 'div'>(props : TextProps<E>) => {}

type TextProps<E extends React.ElementType> = TextOwnProps<E> & Omit<React.ComponentProps<E>, keyof TextOwnProps<E>>

type TextOwnProps<E extends React.ElementType> = { as? : E, /* rest */}
```

