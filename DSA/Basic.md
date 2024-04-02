## Recursion

##### Check if a number can be reached from 1 by adding 5 or multiplying with 3 
```ts
fn solve(path=1) {
	//evaluate possiblities
	if(path < ans) return solve(path + 5) || solve(path * 3)
	else return path === ans
}
```

##### Flatten an object
```ts
js = {}
fn solve(obj, name='') {
	for(key in obj) {
		val = obj[key], nukey = name + key;
		if(typeof val === 'object') solve(val, nukey + "_")
		else js[nukey] = val;
	}
	return js;
}
```

##### Generate Permutations of Array in lexical order
`time : O(n * n!) & space: O(1)`
```js
ans = []; solve(0); return ans;
solve(curri)
	 if(curri === n)
        return ans.push([...nums]) //copy by value
    for(i: curri to n-1)
	    swap(curri, i) //swap i..n-1 & recurr on modified nums
        solve(curri + 1)
        swap(curri, i) //when backtrack, revert swapped to orginal
```

## Prefix Sum

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

##### Longest subarray that sum to K ( has -ve values)
```js
//hash prefixsum indices
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

## Carry Item

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

##### Best time to buy, sell stock
Find max difference between 2 numbers
```js
rmax=a[-1], diff=0; //0 since problem stmt
	for(i : n-2..0)
		profit = rmax - a[i]
		diff = Math.max(diff, a);
	    rmax = Math.max(rmax, a[i])
return diff
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

## Hashing

check duplicates -> use `Set`
need to store frequency -> use `Map`
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

return Object.values(group)
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

## Moore's Voting

Find elements that are majority e.g. appear >n/2 or >n/3 & so on...
For `n/i` problem, solution contains atmost `i-1` ele

**Find >n/2 ele**
```js
isOK(arr, candidate)
  f=0;
  for(val of arr) 
    if(val === candidate) f++
  return f > arr.length / 2; //based on stmt

a, cta=0;
for(val of nums)
	if(val === a) cta++;
	else if(cta === 0)
		cta = 1; a = val;
	else cta-- ;

return isOK(nums, a)? a : null
```


**Find >n/3 ele**
```js
res[2], cta = 0, ctb=0;
for(val of nums)
	if(val === res[0]) cta++;
	else if(val === res[1]) ctb++;
	else if(cta === 0)
		cta = 1; res[0] = val;
	else if(ctb === 0)
		ctb = 1; res[1] = val;
	else 
		cta-- ; ctb--;
return res.filter(v => isOK(nums, v))
```

## Two pointers

Assert Palindrome
##### Remove duplicate from Sorted in O(1)
```js
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

### N Sum problems

Requires SORTING array first and using res[ ] to store pairs

**2-sum `O(NlogN + N)`** 
```js
left=0, right = n-1
while(left < right) 
	s = a[left] + a[right];
	if(s < T) left++
	else if(s > T) right--
	else 
		res.push([a[left], a[right]])
		while (left < right && a[left] == a[++left]);
	    while (left < right && a[right] == a[--right]);
```

**3-sum `O(NlogN + N^2)`**
```js
for(i : 0..n-2)
	if (i > 0 && a[i] === a[i - 1]) continue;
    left = i + 1; right = n - 1;
    while (left < right)
	    s = a[i] + a[left] + a[right];
	    //same as above, push a[i] too
```

**4-sum `O(NlogN + N^3)`**
```js
for(i : 0..n-3)
	if (i > 0 && a[i] === a[i - 1]) continue;
	for(j: i+1..n-2)
		if (j > i+1 && a[j] === a[j - 1]) continue;
	    left = j + 1; right = n - 1;
	    while (left < right)
		    //same as above
```

## Sliding Window

**Patterns**
- Find `n` size subset that <conditon/>  => constant window
- Find `max/min` subset that <condition/> => dynamic window
- Count subsets where <condition/> => ans(supercondition) - ans(subcondition)
- shortest valid window => find largest valid, then shrink to get more valids
##### Longest subarray sum=K (no -ve ele)
```js
sum = a[0]  l=0,r=0, mlen=0
while(++r < n)
	sum+=a[r];
	if(sum == k) mlen= MAX(mlen, r - l + 1)
	else if(sum > k) while(sum > k) sum-=a[l++]
	
if(mlen == 0) return a[-1] == k ? 1 : 0
return mlen
```

##### Longest Substring Without Repeating Characters
```js
if (n < 2) return n
set={a[0]}
while(++rt < n)
	while (set.has(s[rt])) 
      set.delete(s[lf]) //reduce window from left till dup char
      lf++
    set.add(s[rt])
    max = Math.max(max, rt - lf + 1)
```

##### Longest repeating character replacement
```js
freqMap={}, maxfreq, maxlen
//window is valid when non-maxfreq chars in window are atmost k

while(r < n)
	char = s[r], len = r - l + 1;
    freqMap[char] ??= 0;
    maxfreq = MAX(maxfreq, ++freqMap[char])
    while (len - maxfreq > k) { 
      --freqMap[s[l]];
      --len;
      ++l;
    }
    maxlen = Math.max(maxlen, len);
    r++;
```

##### Max points from K Cards
Max sum where l or r should stick to array edge
```js
l = 0, r = k - 1, sum = sum(l..r)
max = sum
while (r !== -1)
    sum -= cardPoints.at(r--);
    sum += cardPoints.at(--l);
    max = Math.max(max, sum);
```

##### Permutation substring in string
```js
len1, len2 //lengths of pattern, string
if(len1 > len2) return false;
count = Array(26).fill(0)
getIdx = (str, i) => str.charCodeAt(i) - 97;

// string will populate count, pattern will cancel chars
// Don't switch their roles
for(i : 0 < len1)
	count[ getIdx(s1, i) ]--
	count[ getIdx(s2, i) ]++
if(count.every( i => i === 0)) return true;
  
//sliding window for leftover string
for(r: len1 < len2)
	idxLf = getIdx(s2, r - len1); //left will leave window
	idxRt = getIdx(s2, r) //right will enter window
	count[idxLf]--; count[idxRt]++
	if(count.every( i => i === 0)) return true;

return false
```

##### Minimum window substring 
```js
var minWindow = function(s, t) {
    // Create an array to store the frequency of characters using their ASCII codes
    const tFreq = new Array(128).fill(0);
    
    // Calculate the frequency of characters in 't'
    for (let char of t) {
        tFreq[char.charCodeAt()]++;
    }
    
    // Initialize pointers and variables
    let left = 0;
    let right = 0;
    let minWindow = "";
    let minWindowLength = Infinity;
    let count = t.length;
    
    // Expand the window to the right
    while (right < s.length) {
        // If the current character in 's' is present in 't', decrease the count
        if (tFreq[s[right].charCodeAt()] > 0) {
            count--;
        }
        
        // Decrease the frequency of the current character in 'tFreq'
        tFreq[s[right].charCodeAt()]--;
        right++;
        
        // Contract the window from the left
        while (count === 0) {
            // If the current window is smaller than the minimum window found so far, update the minimum window
            if (right - left < minWindowLength) {
                minWindowLength = right - left;
                minWindow = s.substring(left, right);
            }
            
            // Increase the frequency of the character at 'left' in 'tFreq'
            tFreq[s[left].charCodeAt()]++;
            
            // If the frequency of the character at 'left' becomes positive, increase the count
            if (tFreq[s[left].charCodeAt()] > 0) {
                count++;
            }
            
            left++;
        }
    }
    
    return minWindow;
};

```



