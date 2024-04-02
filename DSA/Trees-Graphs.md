##### TOP/BOTTOM view of Binary tree
BFS, Hashmap, Sorting
`time : O(nlogn) and space : O(n)`
```js
q = [[root , 0]] //0 => x-coord [root at origin] 
while(!q.empty)
	node, x = deque
	hash[x] ??= node //for BOTTOM view, remove ??
	if(node.left) enque [node, x-1]
	if(node.right) enque [node, x+1]

//sort based on keys, then return values
return Object.keys(hash).sort().map(k => hash[k])
```

##### RIGHT/LEFT view of Binary tree
Right -> dfs Root, Right, Left
Left -> dfs Preorder
`time: O(n) and space: O(height)`
```js
ans = [];
dfs(0)
return ans;

dfs(level, node=root)
  if(!node) return;
  if(level === ans.length) ans.push(node.val)
  dfs(level+1, node.right)
  dfs(level+1, node.left)
```

##### Same, Symmetrical, Subtree of given tree

```js
//isSame(P, Q)
if(!p || !q) return p===q
return p.val === q.val && isSame(p.left, q.left) && isSame(p.right, q.right)

//isSymmetric(P)
return fn(P.left, P.right)
fn(p, q)
    if(!p || !q) return p===q
    return p.val === q.val && fn(p.left, q.right) && fn(p.right, q.left)

//isSubtree(big, small)

```
