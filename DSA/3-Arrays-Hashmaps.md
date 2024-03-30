
| Feature           | Static Array                      | Dynamic Array                    |
| ----------------- | --------------------------------- | -------------------------------- |
| Size              | Fixed, determined at compile time | Variable, changes during runtime |
| Memory allocation | Stack                             | Heap                             |
| Access speed      | Fast                              | Slow                             |
| Flexibility       | Less flexible                     | More flexible                    |
## Hashtable

- Hashing is a technique for storing and retrieving data elements using a key-value structure.
- keys are computed via *hash-function* and it generates a fixed-size value called a `hash value (or code/digest).`
- Collisions occur when multiple keys map to same index
- *load factor* : `ele_count / table_size`. As it increases, collisions become more likely
- Runtime
	- average : `O(1) time, O(N) space`
	- worst: `O(n) when load balance is breached`
- Note : **Hash Tables suffer from bad cache performance**. For large collection - the access time might take longer, since relevant part of table needs to be reloaded from into cache.

**Index Mapping (or Trivial Hashing)** : hashfn directly maps the key to an index in table.
##### Separate Chaining
- an entry in hash table can hold a *linked list* of key-value pairs. On collision, append to list. On retrieval, traverse list at given hash value
- Pros : no need to resize table frequently
- cons: overhead, inefficient if lists are long
##### Open Addressing
- each entry can have a *bucket* with slots for colliding elements. 
- Pros : faster retrievals (especially for sparse tables).
- Cons: 
	- *clustering* :  create many slots in bucket as load factor increases 
	- more need of *resizing* for optimisations
- Schemes used in this,
	- **Linear Probing:** linear search in bucket for retrieval
	- **Quadratic Probing:** 

##### Double HashingTypes
- combines the benefits of both open addressing and separate chaining. It uses two hash functions:
	- *primary* : computes initial index for ele
	- *secondary* : used for probing if a collision occurs. It generates a fixed-size offset to be add to primary hash value in a specific pattern (e.g., multiplying by a prime number).
- Pros : lower level of clustering and better than quadratic probing

##### Rehashing
- dynamically resizing table when load factor exceeds a certain threshold
- It involves
	- making a new larger table
	- re-insert elements with **same** hashfn

