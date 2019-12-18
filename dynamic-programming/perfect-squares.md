# Perfect Squares(https://leetcode.com/problems/perfect-squares/)
Given a positive integer n, find the least number of perfect square numbers (for example, 1, 4, 9, 16, ...) which sum to n.

Example 1:

Input: n = 12
Output: 3
Explanation: 12 = 4 + 4 + 4.
Example 2:

Input: n = 13
Output: 2
Explanation: 13 = 4 + 9.


#### Solution(O(n^2), 224ms)
`perfectCount[n]` stands for the least number of perfect square numbers when given n.

Notice:
Loop condition of the outer loop is `i * i <= n`.
Initial value of the inner loop is `i * i`.

```javascript
var numSquares = function(n) {
  if (n <= 1) return n;
  const perfectCount = [0];

  for (let i = 1; i*i<=n; i++) {
    for (let j = i * i; j <= n; j++) {
      if (i === 1) {
        perfectCount[j] = j;
        continue;
      }
      const max = perfectCount[j];
      const square = i * i;
      const remainder = j%square;
      const quotient = Math.floor(j/square);
      const current = quotient + perfectCount[remainder];
      perfectCount[j] = Math.min(max, current);
    }
  }
  return perfectCount[n];
}
```
