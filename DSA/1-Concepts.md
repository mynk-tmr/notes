Links
- [Coursera Algorithms](https://www.coursera.org/specializations/algorithms?action=enroll)
-  [Cheatsheet BigO space-time](https://www.bigocheatsheet.com/)

Algo-efficiency is measured in terms of its *input size (N)* to keep it machine-independent
- **Time complexity** : time taken to run, as a function of N
- **Space complexity** : 
	- memory required (for input size + auxillary space), as as a function of N
	-  most data structures are `O(N)`, primitives and pass by ref. are `O(1)`
	- to calc, add all *input + local bindings + big-O of nested calls* => then drop constants, submissives

**Asymptotic notations** : mathematical *tools* to represent time-space complexity. Eg. `Big O (worst case), Omega (best), Theta (average)`
#### Big-O 
- the *upper bound* of an algorithm
- most common notations 
	- `O(1)` - *Constant* (N doesn't matter)
	- `O(log N)` - *Logarithmic* (step+1 when N doubles). Divide & conquer style.
	- `O(N)` - *Linear* (when N+1, steps + C)
	- `O(N log N)` - *Nlog N*. A step that cuts/reduces data set will run N times.
	- ---------Bad-------------------------------------
	- `O(n²)` - *Quadratic* (2 loops ; both i=i+C)
	- `O(n³)` - *Cubic* (3 loops; all i=i+C)
	- `O(2ⁿ)` - *Exponential* (for each N+1, step double).
	- `O(N!)` - *Factorial* (checking permutations & combinations). e.g. travelling salesman
- How to calc ?
	- sum big-O of all individual operations

```jsx
for(key in obj1) dothis();  //O(n)
for(key2 in obj2) dothat(); //O(n) -> O(2n) -> O(n)
```

**Memory**
- cache -> primary -> secondary (speeds)
- program+state exist in secondary (RAM) ; had to be loaded to primary so CPU can access

**Approximate computing**: return results close to actual to improve efficiency e.g. search engines

Strategies for optimisation
- *greedy method* : choose best available solution
- *DP* : divide into overlapping subproblems and memoize the results to solve
- *branch and bound*
- *backtracking*: generate sub-solution, check who satisfy constraints, then proceed it with to generate further solutions