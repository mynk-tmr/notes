**Memory**
- cache -> primary -> secondary (speeds)
- program+state exist in secondary (RAM) ; is loaded to primary so CPU can access

**Heap memory**
- large area of memory that a program can allocate and deallocate during its execution. 
- stored in RAM and is global, meaning it can be accessed and modified from anywhere in the program
- Heap manager is a library code that manages the heap for the program. The programmer makes requests to the heap manager. The allocation function reserves a block of memory of the requested size in the heap and returns a reference to it. 
- Heap memory can fragment when there are a lot of allocations and deallocations. 

**Stack memory v/s Heap memory**
* Both are allocated during program execution, but
- Heap stores objects that last longer than stack memory
- When an object is created, it's stored in heap space, while stack memory contains a reference to it. 
- Heap memory is used by all parts of an application, while stack memory is only used by one thread of execution. 
- Heap memory has a slower allocation of variables in comparison to variables on the stack. 


**Caching**
Cache performance depends on *latency* and *hit ratio*. Each replacement strategy is a compromise between them.

**Belady's Algorithm** : discard info that's willn't needed for the longest time. Not practical but most optimal.

**Queue-based invalidation** : LIFO, FIFO

**Recency-based invalidation :** 
- LRU, Time-aware LRU (has expiry)
	-  LRU allow streaming data to fill the cache
- MRU (works where older items, more likely to be accessed), 
- Segmented LRU
	- divided into 2 segments: *probationary* and *protected*. Each is ordered from most-to-least recently-used
	- on *hit*, add item at start of *protected* line. On *miss*, add at start of *probationary*
	- protected is finite, so extra items are moved to *probatinory*

**Frequency-based invalidation :** 
- LFU
- LFRU -> good for networks
	- cache is divided into two partitions: *privileged* (LRU)  and *unprivileged* (LFU)
	- If content is popular, it is pushed into privileged. If unpopular, it is moved to *unpriviliged* by replacing entries therein


**Hashtable**
- Hashing is a technique to store / retrieve data elements using a key-value structure.
- indices are computed via *hash-function* and it generates a fixed-size `hash value (or code/digest).`
- Collisions occur when multiple keys map to same index
- *load factor* : `ele_count / table_size`. As it increases, collisions become more likely
- Efficiency
	- average : `O(1) time, O(N) space`
	- worst: `O(n)` , when load balance is breached`
- Hash Tables suffer from bad cache performance. Relevant part of table needs to be reloaded into cache.

**Rehashing**
- dynamically resizing table when load factor exceeds a certain threshold
- It involves making a new larger table and re-insert elements with **same** hashfn

**Index Mapping (or Trivial Hashing):**  hashfn directly maps the key to an index in table.

**Separate Chaining**
- an entry in hash table holds a **linked list** of key-value pairs. On collision, append to list. On retrieval, traverse list at given hash value
- Pros : no need to resize table frequently
- cons: overhead costs, inefficient if lists are long

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

JS map & set are **doubly linked** hashmap & hashset

|              | HashSet                 | HashMap                                       |
| ------------ | ----------------------- | --------------------------------------------- |
| Duplicates   | No                      | No dup keys                                   |
| Dummy values | Yes                     | No                                            |
| Speed        | slower                  | faster                                        |
| Null         | store single null value | Single null key and any number of null values |
| Data storage | as objects              | as key-value pair.                            |
