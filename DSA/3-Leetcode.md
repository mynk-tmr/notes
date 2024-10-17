### Set - Map

| topic                                            | how to do                                                                                                |
| ------------------------------------------------ | -------------------------------------------------------------------------------------------------------- |
| check duplicates                                 | Set                                                                                                      |
| store freq of items in array                     | Map                                                                                                      |
| two sum problem                                  | {val: index}                                                                                             |
| LRU cache                                        | `shift(k,v)` / map                                                                                       |
| LFU cache                                        | `cache(k,v)` and `FreqMap { [freq] : Keys_Set }`                                                         |
| Map - Set (custom)                               | {k, v}  /// {stringify(k) : v}                                                                           |
| valid anagrams                                   | same length & same freqMap                                                                               |
| group anagrams                                   | `{ [sorted_str] : [anagrams] `                                                                           |
| top k frequent elements                          | FreqMap => Array => sort in DESC b/on freq =>slice(0,k)                                                  |
| longest consecutive sequence of numbers in array | Set                                                                                                      |
| valid sudoku                                     | **Set** for each row/col/box. If (r)(c) already in owner sets, false. Add (r)(c) to owner sets if absent |

### Prefix - Sum / Min / etc

| problem                      | how to do |
| ---------------------------- | --------- |
| product of array except self |           |
| pivot index (lsum is rsum)   |           |

### Slow Fast pointers

| problem                        | how to do |
| ------------------------------ | --------- |
| check cycle in linked list     |           |
| return node where cycle starts |           |

### Sliding Window/Two pointers

When to use?
- If a wider scope of the sliding window is valid, the narrower scope is valid too.
- If a narrower scope is invalid, the wider scope is invalid too.

e.g. subarray sum to 2 -> `[1,1]` is valid, but `[1]` is invalid. So, this problem isn't solved by TPs





## Binary Search

#### Problems
- Guess Number -> define `search(low=0, high=num)`. 
- Find first bad product -> define `search(low=1,high=n)`

##### Koko Eating Bananas
```js
low = 1, high = Math.max(...piles) //speeds
hoursTaken = (speed) => piles.reduce((acc, pile) =>
    acc + Math.ceil(pile / speed), 0)

ans = high;
while (low <= high)
    //get mid
    if (hoursTaken(mid) > h) low = mid + 1
    else 
	    ans = mid; high = mid - 1
return ans;
```

##### Minimum item of sorted rotated array
```js
l = 0, r = n - 1;
while (l < r)
	m = Math.floor((l + r) / 2)
    if (nums[m] > nums[r]) l = m + 1;
    else r = m

return nums[l]
```

##### Search in sorted rotated array
Find which half is sorted, if target lies that in sorted, proceed with this half, else other half
```js
//usual binary l,r, while, ===
    if (nums[l] <= nums[m])  //left half is sorted
      if (nums[l] === target) return l;
      if (nums[l] < target && target < nums[m]) r = m - 1;
      else l = m + 1
      
    else 
      if (nums[r] === target) return r;
      if (nums[m] < target && target < nums[r]) l = m + 1;
      else r = m - 1
```

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

##
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
rmax=a[-1], maxdiff=0; //0 since problem stmt
	for(i : n-2..0)
		maxdiff = Math.max(maxdiff, rmax - a[i]);
	    rmax = Math.max(rmax, a[i])
return diff
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

## Moore's Voting

Find elements that are majority e.g. appear >n/2 or >n/3 & so on...
For `n/i` problem, solution contains atmost `i-1` ele

**Find >n/2 ele**
```js
isOK(arr, candidate)
  f=0;
  for(it : arr) 
    if(it === candidate) f++
  return f > arr.length / 2; //based on stmt

a, cta=0;
for(val : nums)
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

##### Remove duplicate from Sorted in O(1)
```js
uniq = 0
for(i: 1..n-1)
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
Does a permute of pattern in string exist?
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
Min window where chars of pattern (t) exist
```js
tFreq = Array(128).fill(0) //all english chars
for (ch of t) tFreq[ch.charCodeAt()]++

lf = 0, rt = 0, minwd = "", rem = t.length //how much pattern left to match
while (rt < s.length) {
    idx = s[rt].charCodeAt();
    tFreq[idx]--; //trying to cross out pattern
    if (tFreq[idx] == 0) rem--; 
    rt++;

    while (rem === 0) { //all crossed
      if (rt - lf < (minwd.length || Infinity))
        minwd = s.substring(lf, rt);
        
      idx = s[lf].charCodeAt();
      tFreq[idx]++; //left is leaving, so reverse earlier -- 
      if (tFreq[idx] > 0) rem++;
      lf++;
    }
  }
  return minwd;
}
```

##### Sliding Window Maximum
- Each time window of k size slides, find max in window
- uses *monotonic deque* -> where ele are in decreasing order
	- each time ele is enqued, we deque all smaller first

```js
q = [], res = [];
nums.forEach((val, rt) => {
	while (q.length && q.at(-1) < val)
	  q.pop();

	q.push(val);
	if (rt >= k - 1) //arranged k ele, now time to calc maxes
	  res.push(q[0])
	  lf = rt + 1 - k; //lf -- 0
	  if (q[0] === nums[lf]) q.shift()
})
```

## Stack
##### Valid Paranthesis
```js
stack = [];
pairs = { '(' : ')', '{' : '}', '[' : ']'}
for (ch of s)
	if (ch in pairs)
      stack.push(pairs[ch])
    else if(ch !== stack.pop())
      return false;

return stack.length === 0;
```
##### Implement minstack
Get min ele in stack in `O(1)`. On push, store current MIN (min ele so far)
```js
class MinStack {
  push(val) {
    push({ val, min: Math.min(val, this.getMin()) })
  }
  getMin() {
    return stack.length === 0? Infinity : stack.at(-1).min
  }
}
```

##### Next Greater Element (all distinct)
Monotonic stack {top -> bottom , decreasing}. Elements left in stack have no NGE
```js
map = {}
for (val of arr)
    while (stack.length && stack.at(-1) <= val)
      map[stack.pop()] = val;
    stack.push(val)
```

##### Next greater Element distance
```js
stack = [], map = Array(n).fill(0) //0 coz problem stmt
for(i : n-1...0)
	while (stack.length && a[stack.at(-1)] <= a[i])
      stack.pop()
    if (stack.length)
      map[i] = stack.at(-1) - i;
    stack.push(i)
```

##### Car Fleet
Each slower car will swallow faster cars behind it's position to become a fleet. 
```js
//create cars array => car = {time, pos}
cars.sort((A, B) => A.pos - B.pos)
stack = []
for (car of cars) 
	while (stack.length && stack.at(-1).time <= car.time)
	  stack.pop()
	stack.push(car)
return stack.length
```

##### Largest area in histogram of bar width=1
- stack bars as long as height is improving. When not, remove bar. A rectangle can be formed with 
	- height = height of smaller removed bar 
	- width= index of current bar - index of removed bar 
- put earliest possible position of current bar in stack
```js
stack = [] , max = 0;
heights.push(0) //to evaluate last height
heights.forEach((ht, i) => {
	let earliest = i;
	while (stack.length && stack.at(-1)[1] > ht) {
	  let [pos, height] = stack.pop()
	  max = Math.max(max, (i - pos) * height);
	  earliest = pos;
	}
	stack.push([earliest, ht])
})
```

##### Evaluate postfix expression
```js
stack = []
calc = ['+', '-', '*', '/']
for (token of tokens) {
    if (!calc.includes(token)) stack.push(token)
    else
      let B = stack.pop(), A = stack.pop()
      let ans = eval('(' + A + ')' + token + '(' + B + ')');
      ans = Number.parseInt(ans)
      stack.push(ans)
    
return stack.at(-1);
```

## Queue
##### Generate Valid Parantheses for given N
```js
q = [[1, 0, '(']] //count of left & right paras
while (q.length)
	[lf, rt, s] = q.pop()
	if (s.length === 2 * n) ans.push(s)
	if (lf < n) q.push([lf + 1, rt, s + '('])
	if (rt < lf) q.push([lf, rt + 1, s + ')'])
```