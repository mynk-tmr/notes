
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

## Leetcode

##### Check if duplicate in array 
use `Set` , `has`, `add`

##### Remove duplicate in-place from sorted array/LL
```js
//array
uniq = 0
for(i: 0..n-1)
	if(a[i] !== a[uniq]) a[++uniq] = a[i]
a.splice(0,uniq+1)

//linked list -> traverse unique only, remove dups
node = head
while(node?.next)
	if(node.next.val == node.val) node.next = node.next.next
	else node = node.next
```

##### Find pivot index (leftsum = rightsum)
```js
lsum = 0, rsum=0;
nums.forEach(v => rsum+=v)
for(let i in nums) {
	rsum-= nums[i];
    if(lsum === rsum) return i;
    lsum+=nums[i]
}
```

##### Count no. of subarray sum to K
```js
//hash frequency of prefix sums 
map, sum=0, ct=0 
map.set(0, 1)
for(val : arr) 
	sum+=val;
    subfrq = map.get(sum - k); //how many subarrays upto i with sum=k exist?
    fullfrq = map.get(sum) ?? 0;
    if(sub) ct+=sub;
    map.set(sum, fullfrq + 1)
```

##### Longest subarray that sum to K
```js
//for no -ve elements, 2 pointers i,j
//calc prefixsum, check length, if sum exceeds K, move i forward until sum < k
sum = a[0]  i=0,j=0, mlen=0
while(++j < n)
	sum+=a[j];
	if(sum == k) mlen= MAX(mlen, j - i + 1)
	else if(sum > k) while(sum > k) sum-=a[i++]
if(mlen == 0) return a[-1] == k ? 1 : 0
return mlen

//if -ve exist, hash prefixsum indices
for(i: 0 to n-1)
	sum+=a[i]
	if(!map.has(sum)) map.set(sum, i) //always consider leftmost subarray
	if(sum == k) mlen = MAX(mlen, i+1)
	sub = map.get(sum - k) //longest subarray (sum=k) that ends at i
	if(sub) mlen = MAX(mlen, i - sub) // i - (sub + 1) +1
```

##### Array product except self
```js
ans = [], prod = 1
for(i : 0 to n-1)
	ans[i] = prod; //take prefix product, for 0th, it's 1
    prod *= nums[i] 
prod=1
for(i: n-1 to 0)
	ans[i] *= prod; //multiply suffprod
    prod *= nums[i]; //take suffix product
```

##### Two sum problem (find pairs)
```js
map = {} // {ele, index}
for(i in arr) {
	val= arr[i]
	map.set(val, i)
	need = sum - val;
	if(map.has(need)) return [map.get(need), i];
}
```

##### Two sum problem (only check in `O(1) space`)
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
	if(map.size > cap) {
      const lru_item = map.keys().next().value;
      map.delete(lru_item);
    }
}
```

##### Implement LFU Cache

```js
class LFUCache {
  map = new Map(); //stores itm = {value, frequency}
  hash = {}; //stores { i : Set of itms with freq=i}
  minfq = 0;

  constructor(cap) {
    this.cap = cap;
  }

 //each time item is already present, +1 its freq
 //Remove it from set at fqgroup[freq-1] and add to fqgroup[freq]

  #adjustFqGroup(key, itm) {
    hash[itm.freq++].delete(key);
    hash[itm.freq] ??= new Set();
    hash[itm.freq].add(key);
    if (hash[this.minfq].size === 0) ++this.minfq
  }

  get(key) {
    if (!map.has(key)) return -1
    const itm = map.get(key)
    this.#adjustFqGroup(key, itm)
    return itm.value
  }

  put(key, value) {
    if (map.has(key)) {
      const itm = map.get(key)
      this.#adjustFqGroup(key, itm)
      itm.value = value;
    }
    else { //invalidate
      if (map.size === this.cap) {
        const rmkey = hash[this.minfq].keys().next().value;
        hash[this.minfq].delete(rmkey);
        map.delete(rmkey)
      }
      const itm = { value, freq: 0 };
      map.set(key, itm);
      hash[0] ??= new Set();
      hash[0].add(key);
      this.minfq = 0;
    }
  }
}
```

##### Find lexical next permutation of array
`O(n) time & O(1) space`
```js
pivot, swapi 
if(n == 1) return ar;
//find first i,i+1 that are sorted
for(i: n-2..0)
	if (a[i+1] > a[i]) pivot = i; break;

//if no pivot, array is last permute, so reverse it
if(pivot is undef) return ar.reverse()

//swap ele after pivot that's closest bigger 
for(i: n-1 > pivot)
	if (a[i] > a[pivot]) swapi = i; break;
swap(swapi, pivot)

//reverse from pivot+1
while(++pivot < --n) {
    swap(n, pivot)
  }
```

##### Find ele who are larger than all their right ele(s)
```js
rmax = -Infinity
for(i : n-1..0)
	if(a[i] > rmax)
		ans.push(a[i])
		rmax = a[i]
return ans.reverse()
```

##### Find longest consecutive sequence `O(nlogN)`
- sort array, traverse, track largest ele encountered so far
- +1 len if new largest is in sequence, else reset it
```js
nums.sort()
last = nums[0], len = 1, mxlen = 1;
for (let val of nums)
	if (val > last)
	  len = (val - last == 1) ? len + 1 : 1;
	  mxlen = Math.max(len, mxlen)
	  last = val; //at end
```

##### Find longest consecutive sequence `O(N) with Set`
- if set has val-1, val cannot be start of a seq, 
- otherwise it is, so keep deleting ele sequentially from it
```js
len = 0, mxlen = 0; set = new Set(nums);
for(val of set.values())
	if(!set.has(val-1))
	  let it = val;
	  while(set.delete(it++)) len++;
	  mxlen = MAX(mxlen, len);
	  len = 0;
```

##### Check Valid Anagram s, t in O(n)
- store {char, freq} of s in map. Then, for each char of t
```js
ct = map[ch] ?? 0
if(ct == 0) return false;
--map[ch]
```

##### Groups anagrams in array of strings
```js
group = {}
for(str of strs)
    sorted = str.split('').sort().join('');
    group[sorted] ??= [];
    group[sorted].push(str) //the unsorted

return Array.from(Object.values(group))
```

##### Top K(=3) Frequent Elements 
```js
freqMap; freqGroup; result
for(val : nums)
	//set freqMap {val, freq}

for ([val, freq] of freqMap)
    freqGroup[freq] ??= new Set()
    freqGroup[freq].add(val)

for(i : n..1 && res.length < k) //all same or all diff
	if (freqGroup[i]) result.push(...freqGroup[i])
```

##### Find Nth largest element e.g. 3rd
```js
sld = [-Infinity, -Infinity, a[0]] //size 3
for(i : 1 to n-1)
	j = 0
	while(a[i] > sld[j] && j<3) {
		sld[j] = sld[j+1]
		++j;
	}
	sld[j-1] = a[i]	

return sld[0]
```

##### Validate Sudoku
```js
hasVal(set, val) {
  if (val == ".") return false;
  if (set.has(val)) return true;
  set.add(val);
}

get3x3(i, j, board) {
	let r = 3 * Math.floor(i / 3) + Math.floor(j / 3);
    let c = 3 * (i % 3) + (j % 3)
    return board[r][c]
}

//main function
for(i: 0 < 9)
	//define row, col, box as Set()
	for(j: 0 < 9)
		rowi = board[i][j]
		coli = board[j][i]
		boxi = get3x3(i, j, board)
		if(hasVal(row, rowi) || hasVal(col, coli) || hasVal(box, boxi))
			return false;

return true;
```

