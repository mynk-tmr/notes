## Matrices

##### Fill zeros in matrix in 0(1) space
```js
r = matrix.length
c = matrix[0].length;
col0 = 1;

//because 0,0 overlap, use col0 for 1st col's[0] and [0][0] for 1st row's [0]
//mark 1st row & 1st col based on presence of 0
for(i : 0..r)
	for(j : 0..c) 
		if (matrix[i][j] === 0) {
        matrix[i][0] = 0;
        if (j == 0) col0 = 0;
        else matrix[0][j] = 0;
        
//fill 0 based on first row & col
for(i : 1..r)
	for(j : 1..c) 
	    matrix[i][j] = matrix[i][0] && matrix[0][j] && matrix[i][j];

if (matrix[0][0] === 0) matrix[0].fill(0)
if (col0 === 0) 
	for(i : 0..r) matrix[i][0] = 0;
```

##### Rotate matrix/image by 90deg
```js
//swap
swap = (i, j) => [matrix[i][j], matrix[j][i]] = [matrix[j][i], matrix[i][j]]

//transpose the matrix
matrix.forEach((row, i) => {
	if (row == matrix.at(-1)) return;
	//i+1, so we don't swap them to orginal pos
	for (let j = i + 1; j < row.length; j++) { 
	  if (i != j) swap(i,j)
	}
})
//reverse each row
matrix.forEach(row => row.reverse())
```

##### Spiral Matrix Traversal 1
```js
ans = []
const obj = {
	col0: 0,
	row0: 0,
	col1: mat[0].length - 1,
	row1: mat.length - 1,
	hasCol() {
	  return this.col0 <= this.col1;
	},
	hasRow() {
	  return this.row0 <= this.row1;
	}
}
  
while (obj.hasCol() && obj.hasRow())
	for (i : col0...col1)
	  ans.push(mat[row0][i])
	obj.row0++;
	
	for (i : row0...row1)
	  ans.push(mat[i][col1])
	obj.col1--;
	
	if (obj.hasRow()) {
	  for (i : col1...col0)
		ans.push(mat[row1][i])
	  obj.row1--;
	}
	if (obj.hasCol()) {
	  for (i : row1...row0)
		ans.push(mat[i][col0])
	  obj.col0++;
	}
```

##### Pascal's Triangle
- where each ele = nCr `n! / r!(n-r)!. 
```js
//find ele, given n & r ; fn(n-1, r-1)
prev = 1
for(i : 0 < r)
	res *= n-i ; res /= i + 1
return res

//get nth row
ans.push(1)
for(i..1 < n) // ele = prev * (row - col) / col
	prev* = n - i; prev/= i; ans.push(prev)
return ans;

//print entire triangle of n rows in O(n^2)
ans = []
for(i : 0 to n-1)
	row = []
	for(j : 0 to i) //i-th row has i+1 ele
		if (j == 0 || j == i) row.push(1)
		else
	        preRow = ans[i - 1];
	        row.push(preRow[j - 1] + preRow[j])
	ans.push(row)
```
