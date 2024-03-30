
| Feature           | Static Array                      | Dynamic Array                    |
| ----------------- | --------------------------------- | -------------------------------- |
| Size              | Fixed, determined at compile time | Variable, changes during runtime |
| Memory allocation | Stack                             | Heap                             |
| Access speed      | Fast                              | Slow                             |
| Flexibility       | Less flexible                     | More flexible                    |
Hashing is a technique for efficiently storing and retrieving data elements using a key-value structure. Here's the breakdown:

- **Key:** A unique identifier associated with a data element.
- **Hash Function:** A function that takes a key as input and generates a fixed-size value called a hash value (or hash code, hash digest).
- **Hash Table:** A data structure that stores key-value pairs. Each element in the hash table is indexed by its hash value.

Here's how hashing works:

1. **Hash Function Application:** When you want to insert a new element, its key is fed into the hash function.
2. **Hash Value Calculation:** The hash function computes a hash value for the key.
3. **Hash Table Indexing:** The hash value is used as an index into the hash table. Ideally, the element should be stored at this index position.

However, collisions can occur if multiple keys map to the same hash value (like having two people with the same birthday). Collision handling techniques are crucial to manage these situations.

**Index Mapping (or Trivial Hashing)**

This is a very simple approach where the hash function directly maps the key to an index in the hash table. For example, if the key is an integer, the hash function might simply return the key itself. However, this method is only suitable for cases where keys are naturally distributed across the hash table size and collisions are unlikely.

**Separate Chaining for Collision Handling**

In separate chaining, each slot in the hash table can hold a linked list of key-value pairs that map to the same hash value. If a collision occurs during insertion, the new element is appended to the linked list at the corresponding index. Retrieval involves searching the linked list at the calculated index for the desired key.

**Advantages:**

- Efficient for handling many collisions.
- No need to resize the hash table frequently.

**Disadvantages:**

- Introduces overhead due to linked lists.
- Retrieval might become slower in the presence of long linked lists.

**Open Addressing for Collision Handling**

In open addressing, all elements are stored directly within the hash table itself. If a collision occurs, a probing technique is used to find an alternative slot (bucket) in the hash table to store the element. Common probing techniques include:

- **Linear Probing:** Start at the calculated index and probe sequentially until an empty slot is found.
- **Quadratic Probing:** Probe by calculating a sequence of offsets based on the hash value and the collision number.

**Advantages:**

- No linked list overhead.
- Potentially faster retrievals (especially for sparse hash tables).

**Disadvantages:**

- Clustering can occur, where collisions create "pockets" of occupied slots, leading to poor performance.
- Hash table resizing might be needed more often to maintain efficiency.

**Double Hashing**

This technique combines the benefits of both open addressing and separate chaining. It uses two hash functions:

- **Primary Hash Function:** Calculates the initial index for the element.
- **Secondary Hash Function:** Used for probing if a collision occurs. The secondary hash function generates a fixed-size offset to be added to the primary hash value in a specific pattern (e.g., multiplying by a prime number).

**Advantages:**

- Reduces clustering compared to linear probing.
- More efficient probing compared to quadratic probing.

**Disadvantages:**

- Requires two hash functions, increasing complexity.
- Might not completely eliminate clustering.

**Load Factor and Rehashing**

The load factor is the ratio of the number of elements in the hash table to the size of the hash table. As the load factor increases, collisions become more likely, impacting performance.

**Rehashing** is the process of dynamically resizing the hash table when the load factor exceeds a certain threshold. This helps reduce collisions and maintain efficient retrieval operations. Rehashing involves:

1. Creating a new, larger hash table.
2. Re-inserting all existing key-value pairs into the new hash table using the same hash function.