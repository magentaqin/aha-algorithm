# [Unique Paths](https://leetcode.com/problems/unique-paths/)
A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

How many possible unique paths are there?

Example 1:

Input: m = 3, n = 2
Output: 3
Explanation:
From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
1. Right -> Right -> Down
2. Right -> Down -> Right
3. Down -> Right -> Right
Example 2:

Input: m = 7, n = 3
Output: 28

#### Solution One(Runtime 72ms, O(n^2))
The algorithm ignores the order of params. It means whether you pass `m = 3, n=7` or `m=7, n=3`, you will get the same result.

`dp[`${m}.${n}`]` means number of paths when you provide a `m x n` or `n x m` field.

```javascript
var uniquePaths = function(m, n) {
  const max = Math.max(m, n);
  const min = Math.min(m, n)
  const dp = {};
  dp["1.1"] = 1;
  for (let i = 2; i <= max; i++) {
    for (let j = 1; j <= i; j++) {
      const key = `${i}.${j}`
      if (j === 1) {
        dp[key] = 1;
        continue;
      }

      if (i === 2 && j === 2) {
        dp[key] = 2;
        continue;
      }

      let dynamic = dp[`${i-1}.${j}`];
      if (!dynamic) {
        dynamic = dp[`${i-1}.${j-1}`]
      }

      if (i === j) {
        dp[key] = dp[`${i}.${j-1}`] + dynamic + dp[`${i}.${j-2}`]
      } else {
        dp[key] = dp[`${i}.${j-1}`] + dynamic
      }
    }
  }
  return dp[`${max}.${min}`];
};
```
