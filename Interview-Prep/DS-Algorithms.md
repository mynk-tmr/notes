
## Concepts

Algo-efficiency is measured in terms of its *input size (N)* to keep it machine-independent
- **Time complexity** : time taken to run, as a function of N
- **Space complexity** : memory required (for input size + auxillary space), as as a function of N

Asymptotic notations : mathematical *tools* to represent time-space complexity. Eg. `Big O (worst case), Omega (best), Theta (average)`

**Big-O** 
- the *upper bound* of an algorithm 
- ignores `constants` & `non-dominant` numbers
- most common notations 
	- `O(1)` - *Constant* (N doesn't matter)
	- `O(log N)` - *Logarithmic* (step+1 when N doubles). Divide & conquer style.
	- `O(N)` - *Linear* (when N+1, steps + C)
	- `O(N log N)` - *Nlog N*. A step that cuts/reduces data set will run N times.
	- ---------Bad-------------------------------------
	- `O(n²)` - *Quadratic* (2 loops ; both i=i+C)
	- `O(n³)` - *Cubic* (3 loops; all i=i+C)
	- `O(2ⁿ)` - *Exponential* (double steps for each N+1). Recursions
	- `O(N!)` - *Factorial* (checking permutations & combinations). e.g. travelling salesman

When 2 algos have same complexity, choose divider e.g. O(N) & O(N/2) (both *linear* but 2nd better)

**Calculate Big-O**
- break function into individual operations.
- calc Big-O of each operation
- add all

```jsx
for(key in obj1) dothis();  //O(n)
for(key2 in obj2) dothat(); //O(n) -> O(2n) -> O(n)
```


**Memory**
- cache -> primary -> secondary (speeds)
- program+state exist in secondary (RAM) ; had to be loaded to primary so CPU can access

**Space complexity**
- notations like time complexity 
- most data structures are `O(N)`, primitives and pass by ref. are `O(1)`
- to calc, add all *input + local bindings + big-O of nested calls* => then drop constants, submissives
- recursive calls => `O(n²)`


[Cheatsheet BigO space-time](https://www.bigocheatsheet.com/)

**Memoizing** : caching results of expensive function calls in *pure functions*. A function can only be memoized if it is referentially transparent -> call can be substituted with its return.

**Lazy evaluation** or call-by-need,[1] is an evaluation strategy which delays the evaluation of an expression until its value is needed 

**Approximate computing**: return results close to actual to improve efficiency e.g. search engines

![[Pasted image 20231209203605.png]]

![[Pasted image 20231209203436.png]]

