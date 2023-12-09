
## Concepts

Algo-efficiency is measured in terms of its *input size (N)* to keep it machine-independent
- **Time complexity** : time taken to run, as a function of N
- **Space complexity** : memory used, as as a function of N

Asymptotic notations : mathematical *tools* to represent time-space complexity. Eg. `Big O (worst case), Omega (best), Theta (average)`

**Big-O** 
- worst case TSC as a F(N) ; the *upper bound* of an algorithm
- ignores constants & non-dominant numbers
- most common notations 
	- O(1) - *Constant* (N doesn't matter)
	- O(log N) - *Logarithmic* (+1 step when N doubles). Divide & conquer style.
	- O(N) - *Linear* (steps  N)
	- O(N log N) - *Nlog N*. A step that cuts/reduces data set will run N times.
	- O(n²) - *Quadratic* (2 loops ; both i++)
	- O(n³) - *Cubic* (3 loops; all i++)
	- O(2ⁿ) - *Exponential* (double steps for each N+1). Recursions
	- O(N!) - *Factorial* (checking permutations & combinations). e.g. travelling salesman

When 2 algos have same complexity, choose divider e.g. O(N) & O(N/2) (both *linear* but 2nd better)