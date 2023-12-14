Links
- [Coursera Algorithms](https://www.coursera.org/specializations/algorithms?action=enroll)
- 

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

**Lazy evaluation** or call-by-need : delays the evaluation of an expression until its value is needed 

**Approximate computing**: return results close to actual to improve efficiency e.g. search engines

![[Pasted image 20231209203605.png]]

![[Pasted image 20231209203436.png]]


## Recursion

2 steps in recursion -> `base case` ; `recursion case`
It can be `O(n)` like in factorials.

```jsx
//check if no. can be reached from 1 with +5 or *3 only
function findSolution(num) {
  var path2;
  return find(`1`);
  function find(path) {
     path2 = eval(path)
     return path2 == num? true : 
        path2 > num ? false:
        find(`${path} + 5`) || find(`${path} * 3`);
  }
}
```

```jsx
// normalise object properties
function cleanProps(input) {
	let newObj = {};
	void function normal(obj = input, name = '') {
	for (key in obj)
	if (typeof obj[key] == 'object') normal(obj[key], name + key + '_');
	else newObj[name + key] = obj[key];
	}();
	return newObj;
}
```


## Algorithm Techniques

![[Pasted image 20231210191521.png]]

![[Pasted image 20231210191812.png]]

## Data Structures

Recursively-defined data structure can be defined using itself like *linked list*. Such structures support easy recursion.
`list = {value: val, next: list2}`

[BFS v/s DFS](https://stackoverflow.com/questions/3332947/what-are-the-practical-factors-to-consider-when-choosing-between-depth-first-sea)

![[Pasted image 20231215004401.png]]


### Greedy Algorithm

Selecting the best option available at the moment, without considering future.

We can greedify, if any one holds
1. best local step at any moment -> global optimal solution
2. optimal overall solution is sum of optimal solution to its subproblems 

Cons
- never reverses the earlier decision. 
 
Premise 
1. let N = empty solution set
2. add best available item to N
3. if N is feasible, keep item ; else reject item forever
4. repeat 2-3 until a solution is reached.

## Example - Greedy Approach

Problem: You have to make a change of an amount using the smallest possible number of coins.
Amount: $18

Available coins are
  $5 coin
  $2 coin
  $1 coin
There is no limit to the number of each coin you can use.

**Solution:**

1. Create an empty `solution-set = { }`. Available coins are `{5, 2, 1}`.
2. We are supposed to find the `sum = 18`. Let's start with `sum = 0`.
3. Always select the coin with the largest value (i.e. 5) until the `sum > 18`. (When we select the largest value at each step, we hope to reach the destination faster. This concept is called **greedy choice property**.)
4. In the first iteration, `solution-set = {5}` and `sum = 5`.
5. In the second iteration, `solution-set = {5, 5}` and `sum = 10`.
6. In the third iteration, `solution-set = {5, 5, 5}` and `sum = 15`.
7. In the fourth iteration, `solution-set = {5, 5, 5, 2}` and `sum = 17`. (We cannot select 5 here because if we do so, `sum = 20` which is greater than 18. So, we select the 2nd largest item which is 2.)
8. Similarly, in the fifth iteration, select 1. Now `sum = 18` and `solution-set = {5, 5, 5, 2, 1}`.

---

## Different Types of Greedy Algorithm

- [Selection Sort](https://www.programiz.com/dsa/selection-sort)
- [Knapsack Problem](https://en.wikipedia.org/wiki/Knapsack_problem)
- [Minimum Spanning Tree](https://www.programiz.com/dsa/spanning-tree-and-minimum-spanning-tree)
- [Single-Source Shortest Path Problem](https://en.wikipedia.org/wiki/Shortest_path_problem)
- Job Scheduling Problem
- [Prim's Minimal Spanning Tree Algorithm](https://www.programiz.com/dsa/prim-algorithm)
- [Kruskal's Minimal Spanning Tree Algorithm](https://www.programiz.com/dsa/kruskal-algorithm)
- [Dijkstra's Minimal Spanning Tree Algorithm](https://www.programiz.com/dsa/dijkstra-algorithm)
- [Huffman Coding](https://www.programiz.com/dsa/huffman-coding)
- [Ford-Fulkerson Algorithm](https://www.programiz.com/dsa/ford-fulkerson-algorithm)