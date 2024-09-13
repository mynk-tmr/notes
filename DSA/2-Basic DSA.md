## Implement hashmap

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
      delete node;
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

## Implement hashset (no DLL)

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

## Sorting Arrays

##### INSERTION SORT
```js
for(i : 1 to n) //assume left subarr is already sorted
	pick = a[i], j=i-1
	while(a[j] > a[i] && j >=0)
		a[j+1] = a[j] //shift
		--j;
	a[j + 1] = pick
```

##### QUICK SORT
```js
sort(arr, low, high)
	if(low >= high) return
	pIdx = partition(arr, low, high)
	sort(arr, low, pIdx-1)
	sort(arr, pIdx+1, high)

partition(arr, low, high)
	i=low+1, j=high, pivot=a[low]
	while(i < j)
		while(a[i] <= pivot && i <= high - 1) i++; //find first ele > pivot
		while(a[j] >= pivot && j >= low+1) j--; // find first ele < pivot
		if(i < j) swap(a[i], a[j])
	swap(a[low], a[j])
	return j
```

##### MERGE SORT
```js
sort(arr)
	if(n < 2) return arr
	mid = floor(n/2)
	left = sort(arr.slice(0, mid))
	right = sort(arr.slice(mid))
	return merge(left,right)

merge(l, r)
	sorted = [], i=0, j=0
	while( i < l.length && j < r.length)
		sorted.push(a[i] > a[j]? a[i++] : a[j--])
	return [...sorted, ...l, ...r]
```

##### CYCLIC SORT
- for distinct no. from `x to N`, an ele must be `at(x+ele)`
- `[3,2,1,5,4] ---> x=1, N=5`

```js
x = 1
for(i : n-2)
	while(a[i] !== x + i)
		swap(a[i], a[x + i])
```


**Iterative Binary search (prefer this)**
```js
low = 0, high = n - 1;
while (low <= high)
    mid = low + Math.floor((high - low) / 2) //to avoid overflow
    if (nums[mid] === target) return mid;
    else if (nums[mid] > target) high = mid - 1
    else low = mid + 1
return -1
```

**Recursive Binary Search**
```js
return seek()
seek(low = 0, high = n - 1)
    if (high < low) return -1
    mid = low + Math.floor((high - low) / 2)
    if (nums[mid] === target) return mid;
    return nums[mid] > target ? seek(low, mid - 1) : seek(mid + 1, high)
```

**2-D binary search**
```js
m = matrix.length, n = matrix[0].length
l = 0, r = m * n - 1
while (l <= r)
    mid = (l + r + 1) >> 1
    val = matrix[Math.floor(mid / n)][mid % n]
    if (val === target) return true;
    else if (target < val) r = mid - 1
    else l = mid + 1

return false;
```

**Implement a Trie for lowercase english strings**
```js
class Node {
	children = []; //store next letter; eg. 'c' in children[2]
	isLeaf = false; //not a end of word currently
}

class Trie {
	root = new Node()
	insert(str) {
		curr = this.root;
		for(ch of str) {
			idx = ch.charCodeAt() - 'a'.charCodeAt();
			curr.children[idx] ??= new Node()
			curr = curr.children[idx] 
		}
		curr.isLeaf = true;
	}
	search(str) {
		curr = this.root
		for(ch of str) {
			idx = ch.charCodeAt() - 'a'.charCodeAt();
			if(curr.children[idx] === undefined) return false
			curr = curr.children[idx]
		}
		return curr.isLeaf
	}
}

// how to run
let arr = ['and', 'ant', 'trap', 'trans'], trie =  new Trie()
for(word of arr) trie.insert(word)
```