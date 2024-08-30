```ts
//type annotations
let a : string, b : boolean;
let nums : number[] = [1,2,4]
let dim : number[][][]

//literal types
let a : number | "NIL";

//nullish are considered subtypes of all types
let a : number = null //correct

//type inference
let a = 1; //infered by ts compiler

//functions
let getFavoriteNumber = async () : Promise<number> => 28; 

//Optional & Default params
function fn(x? : string, y : number = 10) {}

//type alias --> name for any type
//interface --> like type alias but extensible
type User = { id: number; }
interface User { id : number;}

//type assertions --> to convert less specific to more specific type
let x = y as string;
ele!.focus() //non-null assertion [ele is not NULL]
```

`never` --> a variable never gets a value or a function will never return

`any v/s unknown` --> unknown type can't call any method. Use type assertion to bypass

`readonly` --> makes a property as _read-only_ in the class, type or interface.
```ts
type User = {
	readonly id : string;
}
```

`&` -> intersection types (formed by combining types)
```ts
type Cat = { name : string}
type Domestic = {owner : string}
let kitty : Cat & Domestic
```

`|` -> union types (type which can be one of any operand types)
```ts
let jack : Cat | Domestic 
```

`tuple` -> typed array with a pre-defined length and types for each index.
```ts
let p : [string, number]
```

`interface` -> defines shapes of objects, classes and functions. They are extensible
```ts
interface Addfn<T> {
	(x : T, y: T) : T
}
```

`const` --> tells compiler to infer most narrow type
```ts
[k, v] as const // k : Mykey , v : GoodValueType
```

`declare` -> specifies a type to an already existing variable
```ts
declare function foo(x : number) : string ;
```

*Enums* : allows for creating a value which could be one of many named constants. 
```ts
enum E { A, B} //E.A = 0 E.B = 1
type X = E.A
//to change default numbering
enum E {A=5, B} //5,6
```

*Type narrowing*: refining union types with conditional checks 
```ts
let x : boolean | string
if(typeof x == "boolean") x = !x
instanceof //another check
```

*Declaration merging* : compiler merges two separate declarations with same name into a single definition. (interface, namespace)

*Generics* : create reusable types that can work with many types. They treat types as slots
```ts
interface Addfn<T, U, X> {
	(x : T, y: U) : X
}
```
 
*Namespace* : to encapsulate functions, classes and objects that share common relationships.
```ts
declare namespace D3 {
	export interface drawRect(x: number, y: number) : void;
	//export is needed to access it outside
}

//usage
class Rectangle implements D3.drawRect {
...
}MongoDB
```

**Decorators** : to add features to methods & functions
```ts
//add logger
function log<T, P, Q>(fn: T, context: ClassMethodDecoratorContext) {
    return function(this: Q, ...args: P) {
        console.log("Entered "+ context.name)
        return fn.call(this, args)
    }
}

//Runs before any other field initialisation
//greet --> replaced by return of @log
@log
greet() {  /* code */ }

@bound @log greet() { } //log runs first
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

```ts
//ISOMORPHIC COMPONENT

const Text = <E extends React.ElementType = 'div'>(props : TextProps<E>) => {}

type TextProps<E extends React.ElementType> = TextOwnProps<E> & Omit<React.ComponentProps<E>, keyof TextOwnProps<E>>

type TextOwnProps<E extends React.ElementType> = { as? : E, /* rest */}
```

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