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
* Each node represents a character or a part of a string. Root node = empty string. Each edge from a node signifies a specific character. 
* The path from the root to a node represents the prefix of a string stored in the Trie
* Type of node : `{isLeaf: boolean ; child : Node[26] }`
* Time Complexity: O(number of words * maxLengthOfWord)
* Auxiliary Space: O(number of words * maxLengthOfWord)
* Suffix Tree stores all the suffixes of a given string. Tries generally require more space.

**Graphs** 
* used to track relationships or paths. Used in social networks, recommendation engines and pathfinding algorithms. 

**Hashtable**
- Hashing uses a hash-function to map **keys** to storage locations in a table (hascode/digest)
- Why use? When **constant** lookup, ins/del is needed. Needs **linear** space
- Use cases: Memoisation, Caching
- Collisions occur when multiple keys map to same **index**.  As *load factor* `ele_count / table_size` increases, collisions become more likely
- Hash Tables suffer from bad cache performance. Relevant part of table needs to be reloaded into cache.

**Rehashing** : make a new larger table and re-insert elements with **same** hashfn. Done to lower collision rate and load factor

**Index Mapping (or Trivial Hashing):**  hashfn directly maps the key to an index in table. e.g. Map

**Separate Chaining**
- an entry in hash table holds a **linked list** of key-value pairs. On collision, append to list. On retrieval, traverse list at given hash value
- Rehashing is less needed but Overhead costs, inefficient if lists are long

**Open Addressing**
- each entry has a **bucket** which store key-value pairs 
- Pros : faster retrievals (especially for sparse tables).
- Cons: more need of *resizing* ; and *clustering*
- Schemes used in this
	- **Linear Probing:** on collison, use `i` closest to hashed index. Tends to create **primary** clustering, ie., large buckets near hashed indices
	- **Quadratic Probing:** on collision, use `hash(val) + i^2 where i=1.. is attempt no.` to find an empty slot. Creates **secondary** clusters , ie. away from hashed indicies

**Double Hashing**
- combines benefits of both open addressing and separate chaining. It uses two hash functions:
	- *primary* : computes initial index for ele
	- *secondary* : used for probing if a collision occurs. It generates a fixed-size offset to add to primary hash value in a specific pattern (e.g., multiplying by a prime number).
- Pros : lower level of clustering

**Hash Set**
- stores a collection of unique elements, where element acts as **both** key & value
- Use Cases: store unique ID, membership (dict), set operations like union

**Hash Map**
- store collection of elements as key-value pairs. 
- Use Cases:
    - Associating data with unique ID (e.g. account name -> ID).
    - Caching data, building configuration files

**Heap memory**
- large area of memory that a program can allocate and deallocate during its execution. 
- stored in RAM and is global, meaning it can be accessed and modified from anywhere in program
- Heap manager is a library code that manages the heap for the program. The allocation function reserves a block of memory of the requested size in the heap and returns a reference to it. 
- Heap memory can fragment when there are a lot of allocations and deallocations. 

**Stack memory v/s Heap memory** : both are allocated during program execution, but
- Heap stores objects that last longer than stack memory
- When an object is created, it's stored in heap, while stack contains a reference to it. 
- Heap is used by all parts of an application, while stack is only used by one thread of execution. 
- Heap memory has a slower allocation of variables

**Cache** 
* CPU cache is a small, fast memory between **main memory (RAM)** and CPU. Program+state exist in RAM. 
* It stores recently accessed data and instructions, so the CPU can access them quickly without  fetching them from the slower main memory. 
* Contiguous memory storage allows for better caching and fewer cache misses. When an array element is accessed, the cache can prefetch and store nearby elements
* Cache performance depends on **latency** and **hit ratio**. Each replacement strategy is a compromise between them.
* To implement an LRU cache we use : **a hashmap and a doubly linked list**. DLL helps in maintaining the eviction order and a hashmap helps with O(1) lookup