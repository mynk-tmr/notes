**Terms**
- **Problem** - a function of inputs and mapping them to outputs.
- **Algorithm** - 
	- a step-by-step set of operations to solve a specific problem or a set of problems.
	- properties : It must be correct and has finite concrete steps
- **Program** - a specific sequence of instructions in a prog. lang., and it may contain the implementation of many algorithms.
- Meaningful and processed data is the **information**.
- **cost** of a solution is the amount of resources it needs.

**Data type** 
* defines a certain **domain** of values and **operations** allowed on those values e.g `int` 
    - Takes only integer values
    - Operations: +, -, /
- **Primitive data types** : int char
- **Non-primitive**: user-defined e.g. stack, heap, etc
- **Abstract data type** (non-primitives) describes a set of objects sharing the same properties (domain) and behaviors (operations)

**Data Structure**
- A organization of the data so that it can be used efficiently. It is used to implement an ADT.
- ADT tells us *what* is to be done and data structures tell use *how* to do it.
- Types:
	- **linear** : data elements are **sequentially** connected. Can traverse all elements in one run e.g. Array, Linked List, Stack, Queue
	- **non-linear** (tree, graph): **heirarchical** connected. e.g. Trees, Graphs 
	- **static** (compile time memory allocation) eg. array
		- Advantage: fast access
		- Disadvantage: slow insertion and deletion
	- **dynamic** (run-time memory allocation), linked list
		- Advantage: faster insertion and deletion
		- Disadvantage: slow access

**Algo-efficiency** is measured in terms of its input size (N) to keep it machine-independent
- **Time complexity** : time taken to run, as a function of N. Add big-O of all individual operations
- **Space complexity** : 
	- memory required (for input size + auxillary space), as as a function of N
	- most data structures are `O(N)`, primitives and pass by ref. are `O(1)`
	- to calc, add all *input + local bindings + big-O of nested calls* => then drop constants, submissives

**Asymptotic notations** : mathematical tools to represent time-space complexity. Eg. `Big O (worst case), Omega (best), Theta (average)`

**Big-O**:  the upper bound of an algorithm in worst-case situation.
- `O(1)` - *Constant* (N doesn't matter)
- `O(log N)` - *Logarithmic* (step+1 when N doubles). Divide & conquer style.
- `O(N)` - *Linear* (when N+1, steps + C)
- `O(N log N)` - *Nlog N*. A step that cuts/reduces data set will run N times.
- ---------Bad-------------------------------------
- `O(n²)` - *Quadratic* (2 loops ; both i=i+C)
- `O(n³)` - *Cubic* (3 loops; all i=i+C)
- `O(2ⁿ)` - *Exponential* (for each N+1, step double).
- `O(N!)` - *Factorial* (checking permutations & combinations). e.g. travelling salesman

**Lists and Array**
* both store ordered collection of values. 
* List can have **multiple** types and don't support **math** operations
* Array only **same** type and may support math operations.
* use cases: TODO, carts, social media feed
* Problem: **slow insertion, deletion, reordering**
* Arrays are well-suited if 
	* size of the collection is known or doesn't change frequently.
	* random access to elements is required
* **Static** arrays : fixed size arrays; stored in **stack** ; are faster
* **Dynamic** arrays: no-fixed size; stored in **heap** 

**Linear Search v/s Binary Search**
* linear : sequential search `O(n)`
* Binary: finding an element's position in a sorted array using divide & conquer `O(log n)`

**Heap data structure:** 
- It is a **complete binary tree** (nodes are formed from left to right)
- In **max-heap**, all nodes are greater than their children. 
- In **max-heap**, all nodes are smaller than their children. 
- Uses : memory management, **priority queues** (task scheduling) 

**Linked Lists**
* elements are stored in ordered sequence of nodes. Each node contains a pointer to the next node.  `{data, next }`
* All operatons are `O (n)`
* Why better?
	* No need to shift data to insert/delete items
	* doesn't require contiguous memory locations
	* Dynamic sizing (shrink/grow at run-time)
* head pointer "defines" the linked list (it is not a node). It gives access to 1st element.
* Cons : more memory ; NO random access ; No binary search
* **Circular linked lists**
	* last node contains a pointer to the first node. No node points to NULL
	* Uses: **looping** is needed, from last go to 1st, circular queues
* **Doubly linked lists**
	* nodes contains a pointer to previous node too. (Bidrection LL)
	* Pros over SLL: 
		* constant-time additions and removals at both head/tail 
		* traverse in both directions -> faster INS/DEL
	* Uses: undo/redo ; forward/backward navigation (playlist) 


**Stacks**: LIFO
* Elements are added to and removed from the top (most recently added items).
* Use cases: undo/redo operations
* Stack may be created using an **array or singly linked list**. For SLL, prepend and HEAD = top
* Properties: `top len maxLen`
* Operations : `push(i) pop() peek() clear() isEmpty()`

**Queues**: FIFO
* Elements can only be added to `rear` and removed from `front`
* Properties: `front=0` `rear=0` `len` `maxlen`
* Operations: `enque(i)` `deque()` `peek()` `isEmpty()` `clear()`
* Uses: print jobs, user-input processing, chat apps 
* **Linear** Queues : 
	* Implemented using either an array or linked list.
	* Cons: once `rear` has reached array's end, it's not possible to add elements
* **Circular** Queues
	*  last element is connected to first element forming a circle. So `rear` can move forward, if queue has empty space
	* `rear=-1` and has `isFull()` 
* **Double-Ended / Dequeue**
	* Implemented using a circular array or a circular **doubly** linked list.
	* Elements can be added/removed from either end `LEFT`, `RIGHT`
	* *Input-restricted* : can enqueue at only 1 end, deque from both
	* *Output-restricted*: can deque from only 1 end.
* **Priority Queue**
	* each element is assigned a priority to determine the order in which elements should be processed
	* Two elements with the same priority are processed on (**FCFS**) basis.

**Trees**
* Trees organize data in a **hierarchicy** of nodes connected via **edges**.
* show natural hierarchies or relationships. e.g. parent-child rship of HTML elements 
* uses : database indexing, decision trees, file systems
* Terms
	* **Root**: node without a parent
	- **Siblings**: nodes share the same parent
	- **Internal node**: node with at least one child
	- **External node** (leaf): node without children
	- **Ancestor**: all nodes that can traverse to given node
	- **Descendants**: all nodes that given node can reach
	- **Depth/ Height** of a node: number of edges from root to node / node to deepest leaf.
	- **Height of tree**: height of root
	- **Degree of a node**: no. of children. Leaf = 0
	- **Degree of a tree**: maximum degree of a node in the tree.
	- **Subtree**: tree consisting of a node and its descendants
	- **Empty (Null)-tree**: a tree without any node
	- **Root-tree**: a tree with only one node

**Binary Trees**
* a tree where each node has a `left` and `right` pointer for its children. Any node can't have >2 children
* **Complete binary tree** - every level except possibly the last is completely filled. All nodes must appear as far left as possible.
* Implementation
	* Linked list way : `node = {data, left, right}`. A `root` pointer in tree
	* Array implementation
		* `tree[0] = root`
		* for node at `i`, parent at `floor(i/2) - 1`, left child at `2i + 1` and right child at `2i + 2`
* Traversing 
	* **Preorder** : visit root, traverse left sub-tree, then right sub-tree
	* **Postorder**: (L, R, X)
	* **Inorder**: (L, X, R)
* Uses
	* evaluation of expressions: all operands are at leaves

**Binary Search Trees / Ordered Binary tree**
* variant of binary trees in which the nodes are arranged in an order viz. `left < root <= right`

**Tries**
* stores all the **prefixes** of a set of words in a tree-like structure
* Each node represents a character or a part of a string. Root node = empty string. Each edge from a node signifies a specific character. The path from the root to a node represents the prefix of a string stored in the Trie
* Type of node : `{isLeaf: boolean ; child : Node[26] }`
* Time Complexity: O(number of words * maxLengthOfWord)
* Auxiliary Space: O(number of words * maxLengthOfWord)
* Suffix Tree stores all the suffixes of a given string. Tries generally require more space.


Hash tables allow for efficient data 
lookup, insertion, and deletion.   
They use a hash function to map keys to 
their corresponding storage locations. 
It enables constant-time 
access to the stored values. 
Hash tables are widely used in various 
applications, such as search engines,   
caching systems, and programming 
language interpreters or compilers. 
In search engines, hash tables can be used   
to store and quickly retrieve 
indexed data based on keywords. 
This provides fast and relevant search results. 
Caching systems may use hash tables 
to store and manage cached data. 
It allows for rapid access to frequently requested 
resources and improves overall system performance. 
Another example is the implementation of symbol   
tables in programming language 
interpreters or compilers. 
Hash tables can be used to efficiently 
manage and look up variables, functions,   
and other symbols defined in the source code. 


Graphs are all about tracking 
relationships or finding paths. 
This makes them invaluable in social networks,   
recommendation engines, 
and pathfinding algorithms. 
In a social network, a graph can be used 
to represent the connections between users. 
It enables features like friend 
suggestions or analyzing network trends. 

Cache 
CPU cache is a small, fast memory between 
the main memory (RAM) and the CPU. 
It stores recently accessed data and instructions,   
so the CPU can access them quickly without 
fetching them from the slower main memory. 
Different data structures have varying levels   
of cache friendliness based on how 
their elements are stored in memory. 
Contiguous memory storage, like that in arrays,   
allows for better cache locality and fewer 
cache misses. 
When an array element is accessed, the cache 
can prefetch and store nearby elements,   
anticipating that they might be accessed soon. 
On the other hand, data structures 
with non-contiguous memory storage,   
like linked lists, can experience more 
cache misses and reduced performance. 

To implement an LRU cache we use two data structures: **a hashmap and a doubly linked list**. A doubly linked list helps in maintaining the eviction order and a hashmap helps with O(1) lookup of cached keys.