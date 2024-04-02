Links
- [Coursera Algorithms](https://www.coursera.org/specializations/algorithms?action=enroll)
-  [Cheatsheet BigO space-time](https://www.bigocheatsheet.com/)

Algo-efficiency is measured in terms of its *input size (N)* to keep it machine-independent
- **Time complexity** : time taken to run, as a function of N
	- sum big-O of all individual operations
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

**Memory**
- cache -> primary -> secondary (speeds)
- program+state exist in secondary (RAM) ; had to be loaded to primary so CPU can access

**Approximate computing**: return results close to actual to improve efficiency e.g. search engines

Strategies for optimisation
- *greedy method* : choose best available solution
- *DP* : divide into overlapping subproblems and memoize the results to solve
- *branch and bound*
- *backtracking*: generate sub-solution, check who satisfy constraints, then proceed it with to generate further solutions

## Caches

Average time to access memory
```text
T = (m * Tm) + Th + E, where
m = miss ratio 
Tm = time to access main-memory upon miss
Th = latency ; time to retrieve from cache
E = secondary time (internals)
```

Cache performance depends on *latency* and *hit ratio*. Each replacement strategy is a compromise between them. Algorithms also maintain [cache coherence](https://en.wikipedia.org/wiki/Cache_coherence "Cache coherence") when several caches are used for the same data

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

## Arrays


| Feature           | Static Array                      | Dynamic Array                    |
| ----------------- | --------------------------------- | -------------------------------- |
| Size              | Fixed, determined at compile time | Variable, changes during runtime |
| Memory allocation | Stack                             | Heap                             |
| Access speed      | Fast                              | Slow                             |
| Flexibility       | Less                              | More                             |
## Hashtable

- Hashing is a technique to store / retrieve data elements using a key-value structure.
- keys are computed via *hash-function* and it generates a fixed-size `hash value (or code/digest).`
- Collisions occur when multiple keys map to same index
- *load factor* : `ele_count / table_size`. As it increases, collisions become more likely
- Runtime
	- average : `O(1) time, O(N) space`
	- worst: `O(n) when load balance is breached`
- Hash Tables suffer from bad cache performance. Relevant part of table needs to be reloaded into cache.
##### Index Mapping (or Trivial Hashing): 
- hashfn directly maps the key to an index in table.
##### Separate Chaining
- an entry in hash table can hold a *linked list* of key-value pairs. On collision, append to list. On retrieval, traverse list at given hash value
- Pros : no need to resize table frequently
- cons: overhead costs, inefficient if lists are long
##### Open Addressing
- each entry has a *bucket* which store key-value pairs 
- Schemes used in this
	- **Linear Probing:** on collison, use `i` closest to hashed index. 
		- tends to create *primary* clustering, ie., large occupied slots near hashed indices
	- **Quadratic Probing:** on collision, use `hash(val) + i^2 where i=1.. is attempt no.` to find an empty slot
		- creates *secondary* clusters , ie. away from hashed indicies. Less severe
- Pros : faster retrievals (especially for sparse tables).
- Cons: 
	- more need of *resizing* for optimisations
	- *clustering*
##### Double Hashing
- combines benefits of both open addressing and separate chaining. It uses two hash functions:
	- *primary* : computes initial index for ele
	- *secondary* : used for probing if a collision occurs. It generates a fixed-size offset to  add to primary hash value in a specific pattern (e.g., multiplying by a prime number).
- Pros : lower level of clustering and better than quadratic probing

##### Rehashing
- dynamically resizing table when load factor exceeds a certain threshold
- It involves making a new larger table and re-insert elements with **same** hashfn

## HashSet and HashMap

##### Hash Set
- stores a collection of unique elements, where element acts as **both** key & value
- Operations: `add, remove, contains, size =>(ele_count)`
- Use Cases: store unique ID, membership (dict), set operations like union
##### Hash Map
- store collection of elements as key-value pairs.
- supports operations - `put, get, remove, contains, size`
- Use Cases:
    - Associating data with unique ID (e.g. account name -> ID).
    - Caching data, building configuration files

JS map & set are doubly linked hashmap & hashset

|              | HashSet                 | HashMap                                       |
| ------------ | ----------------------- | --------------------------------------------- |
| Duplicates   | No                      | No dup keys                                   |
| Dummy values | Yes                     | No                                            |
| Speed        | slower                  | faster                                        |
| Null         | store single null value | Single null key and any number of null values |
| Data storage | as objects              | as key-value pair.                            |
### Implement hashmap

```js
class DLL {
  head = null; tail=null
  append(key, val) {
      const node = {key, val, prev: null, next: null}
      if(!head) head=tail=node;
      else {
          tail.next = node; node.prev = tail; tail = node;
      }
  }
  remove(node) {
      if(!node.next && !node.prev) // if there's only 1 node
          head=tail=null
	  else if(!node.next) { // if node is tail node
          tail = node.prev; tail.next = null;
    } else if(!node.prev) { // if node is head node
          head = node.next; head.prev = null;
      } else { 
          prev = node.prev; next = node.next;
          prev.next = next; next.prev = prev;
      }
  }
  find(key) {
	  node = tail;
	  while(node) {
		  if(node.key === key) return node;
		  else node = node.prev
	  }
  }
}

class HashMap {
	table = {}; size=0; 
	//ctr -> maxsize
  #hash(key) {
    let sum = 0;
    for (let i in key) sum += key.charCodeAt(i);
    return sum % this.maxsize; //% is important
  }
  put(key, value) {
	if(size == mxsize) return;
	let i = #hash(key)
    let list = table[i] ?? new DLL()
    list.remove(list.find(key))
	list.append(key, value)
    table[i] = list;
    size++;
  }
  get(key) {
    let list = table[#hash(key)];
    return list?.find(key).val;
  }
  remove(key) {
    node = list.find(key))
    if(!node) return false;
    list.remove(node)
    return --size;
  }
}
```

### Implement hashset (no DLL)
```js
class HashSet {
  //same constructor, fields
  //same #hash(key)
  add(v) {
	if(size === maxsize) return;
	i = #hash(v)
	if(table[i] === undefined) size++;
    table[i] = v;
  }
  contains(v) {
    return table[#hash(v)] !== undefined
  }
  remove(v) {
    i = #hash(v);
    if(table[i] === undefined) return false;
    delete table[i]
    return --size;
  }
```