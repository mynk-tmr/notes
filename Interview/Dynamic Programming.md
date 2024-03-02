## Concepts



**3. Optimal substructure:**

- Dynamic programming relies on the principle of **optimal substructure**. This principle states that **an optimal solution to a problem can be constructed from optimal solutions to its subproblems**. In simpler terms, if we have the best solutions to all the smaller subproblems, we can efficiently combine them to get the best solution for the entire problem.

**4. Memoization:**

- To avoid redundant calculations, dynamic programming uses **memoization**. This technique involves **storing the solutions to the subproblems** in a table or data structure. When solving a larger subproblem, we first check if the solution to its corresponding smaller subproblem already exists in the table. If it does, we simply use the stored solution instead of recomputing it.

**5. Bellman equation:**

- The **Bellman equation** is a central concept in dynamic programming. It's a **recursive formula** that expresses the value of an optimal solution to a subproblem in terms of the optimal solutions to its smaller subproblems. This equation allows us to efficiently calculate the solutions from the bottom up, filling in the memoization table mentioned earlier.
- 