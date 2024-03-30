
| Feature           | Static Array                      | Dynamic Array                    |
| ----------------- | --------------------------------- | -------------------------------- |
| Size              | Fixed, determined at compile time | Variable, changes during runtime |
| Memory allocation | Stack                             | Heap                             |
| Access speed      | Fast                              | Slow                             |
| Flexibility       | Less flexible                     | More flexible                    |
## Hashtable

- Hashing is a technique to store / retrieve data elements using a key-value structure.
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
	- **Quadratic Probing:** if slot is occupied, use `hash(val) + i^2 where i=1.. is attempt no.` to find an empty slot
##### Double Hashing
- combines the benefits of both open addressing and separate chaining. It uses two hash functions:
	- *primary* : computes initial index for ele
	- *secondary* : used for probing if a collision occurs. It generates a fixed-size offset to be add to primary hash value in a specific pattern (e.g., multiplying by a prime number).
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

|              | HashSet                 | HashMap                                       |
| ------------ | ----------------------- | --------------------------------------------- |
| Duplicates   | No                      | No dup keys                                   |
| Dummy values | Yes                     | No                                            |
| Speed        | slower                  | faster                                        |
| Null         | store single null value | Single null key and any number of null values |
| Data storage | as objects              | as key-value pair.                            |
### Implement hashmap

```js
class HashMap {
  constructor(maxsize) {
    this.table = {};
    this.maxsize = maxsize;
    this.size = 0;
  }
  #hash(key) {
    let sum = 0;
    for (let i in key) sum += key.charCodeAt(i);
    return sum % this.maxsize; //% is important
  }
  put(key, value) {
	if(this.size === this.maxsize) return;
    let i = this.#hash(key);
    let bucket = this.table[i]; 
    if (!bucket) bucket = [[key, value]];
    else {
      let dup_pair = bucket.find(pair => pair[0] === key);
      if (!dup_pair) bucket.push([key, value]);
      else dup_pair[1] = value;
    }
    this.table[i] = bucket; //take back new bucket
    this.size++;
  }
  get(key) {
    let bucket = this.table[this.#hash(key)];
    return bucket?.find(pair => pair[0] === key)[1];
  }
  remove(key) {
    let bucket = this.table[this.#hash(key)];
    if (!bucket) return false;
    let i = bucket.findIndex(pair => pair[0] === key);
    bucket.splice(i, 1);
    return --this.size;
  }
}
```

### Implement hashset
```js
class HashSet {
  //same constructor
  //same #hash(key)
  add(value) {
	if(this.size === this.maxsize) return;
    this.table[this.#hash(key)] = value;
    this.size++;
  }
  contains(value) {
    const res = this.table[this.#hash(key)];
    return Boolean(res)
  }
  remove(value) {
    const i = this.#hash(key);
    if(this.table[i] === undefined) return false;
    delete this.table[i]
    return --this.size;
  }
```


## Leetcode

Find duplicate in array -> use `Set` , `has`, `add`

Two sum problem (find pairs)
```js
map = {} // {ele, index}
for(i in arr) {
	val= arr[i]
	map.set(val, i)
	need = sum - val;
	if(map.has(need)) return [map.get(need), i];
}
```

Two sum problem (only check in `O(1) space`)
```js
//sort arr
i= 0, j = n-1
while( i < j) {
	if(ar[i] + ar[j] == sum) return true
	++i; --j
}
```

##### Implement LRU Cache
- map store keys in order, so on get/put delete key and re-insert. Then, LRU item will bubble to first index of map. To get it, use iterator's `{value : item}`
- without built in ? create `map` with *(doubly linked list hashmap*)
	- on get, put -> delete & prepend item
	- on cap breach, remove tail

```js
class LRUCache {
  //ctr --> map, capacity
  get(key) {
	  if (!map.has(key)) return -1
	  //delete key; set key,val; return val
	}
  put(key, v) {
	//delete key ; set k,v ; 
	if(this.cap < this.map.size) {
      const lru_item = map.keys().next().value;
      map.delete(lru_item);
    }
}
```

##### Implement LFU Cache