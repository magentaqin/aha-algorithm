# 4Sum II
Given four lists A, B, C, D of integer values, compute how many tuples (i, j, k, l) there are such that A[i] + B[j] + C[k] + D[l] is zero.

To make problem a bit easier, all A, B, C, D have same length of N where 0 ≤ N ≤ 500. All integers are in the range of -228 to 228 - 1 and the result is guaranteed to be at most 231 - 1.

Example
```
Input:
A = [ 1, 2]
B = [-2,-1]
C = [-1, 2]
D = [ 0, 2]

Output:
2

Explanation:
The two tuples are:
1. (0, 0, 0, 1) -> A[0] + B[0] + C[0] + D[1] = 1 + (-2) + (-1) + 2 = 0
2. (1, 1, 0, 0) -> A[1] + B[1] + C[0] + D[0] = 2 + (-1) + (-1) + 0 = 0
```

#### Answer One[O(n^2). Run time: 300ms]
The basic idea is sum A[i] + B[j], and sum C[k] + D[l]. Find if they are absolute value of each other.
User object `left` to store sum value and the count of the the same sum value.
```javascript
var fourSumCount = function(A, B, C, D) {
  // key is abSum, and value is the times key appears.
  const left = {};
  for (let a of A) {
      for (let b of B) {
        const abSum = a+b;
        // if the same sum value exists, times should +1.
        if (left[abSum]) {
          left[abSum] = left[abSum] + 1;
        } else {
          left[abSum] = 1;
        }
      }
  }
  let count = 0;
  for (let c of C) {
    for (let d of D) {
      const cdSum = c + d;
      const negativeSum = -cdSum;
      // check whether the absolute value of c + d exists. if exists, get the times and add it to count.
      if (left[negativeSum]) {
        count += left[negativeSum]
      }
    }
  }
  return count;
};
```


#### Answer Two[156ms]
