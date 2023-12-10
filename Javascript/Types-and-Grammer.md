
### Control Flow 

**Conditional stmts**
- `case: ` tests for *strict* equality ; case can be enum values / string object
- `switch` is faster -> due to jump table creation during compilation
- `if-else` is more flexible than switch

**Loops**
- entry controlled (check first, then exec) -> `for` `while`
- exit controlled (execute first, then check) -> `do while`
- `for` evaluates step at end of current iteration
- In ONLY while & do while loops, *postfix* runs *1 extra* than prefix.
- `for in` iterates over *enumerable* properties (enum flag `true`) 
- `for of` iterates over *values* of **iterable** objects (built-in or user-specified)
	- both pass by **value** to key/val
- `for await(.. of ..)`
	- iterate over async oR sync objects
	- can trigger custom iteration workflows for each property
- `onix : for(key in obj)`
	- onix is *label*, an identifier for *codeblock* that you can refer to elsewhere
- `break` terminates current (or *labelled*) loop
- `continue` *jumps* control-flow to next iteration. In for, it updates STEP. Given *label* must be LOOP
- you can only jump to *parent* label

**Ternary/conditional operator**
- acts on 3 operands and works like an *inline-if-else*.
- returns value of expr chosen to be evaluated.
- can't work with `break` `continue` `return` (can't control flow)

### Error Handling

**Stack trace** : list of function calls that lead to an error -> latest to earliest

`try {..}`
- creates a codeblock where any error transfers control to *first catch block* in call stack. 
- To specify user-defined exception, we have `throw val` (val can be any value/function)
- Error object is captured and visible only inside `catch` block. 

`finally {..}` : always execute *immediately* after `try` and `catch` blocks. *Overrides* rethrows/returns in `try-catch` with its own return (if given)

#### Error objects

All errors are *serializable* object, so can be cloned with structuredClone() or copied between [Workers](https://developer.mozilla.org/en-US/docs/Web/API/Worker) using postMessage()

**Types**
- `ReferenceError` — accessing undeclared or uninitialized variables
- `SyntaxError` — incorrect syntax
- `TypeError` — invalid operands/arguments; invalid use or change to a value
- `StackOverflowError` — when number of execution contexts exceed size of host’s stack

**Properties**
- `name` , `message` , `stack` 


**Equality checks**
- `==` : loosely equal (type convert operands)
- `= = =` : strictly equal (no conversin)
- `Object.is(val,val)`: strict, treats `-0` and `+0` differently, and 2 `NaN` as equal

