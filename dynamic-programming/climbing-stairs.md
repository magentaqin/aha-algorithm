# Climbing Stairs [https://leetcode.com/problems/climbing-stairs/]
You are climbing a stair case. It takes n steps to reach to the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

Note: Given n will be a positive integer.

Example 1:

Input: 2
Output: 2
Explanation: There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps
Example 2:

Input: 3
Output: 3
Explanation: There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step

#### Answer One (Tabulation Way. 64ms)
`climbWays[i]` represents the number of distinct ways to reach i.
The number of distinct ways to reach i depends on `climbWays[i-1]` + `climbWays[i-2]`. (Simply because if we want to reach i, we can either climb from i -1 or from i -2.)
```javascript
var climbStairs = function(n) {
  const climbWays = Array(n).fill(0)
  climbWays[0] = 0;
  for (let i = 1; i <= n; i++) {
    if (i > 2) {
      climbWays[i] = climbWays[i-1] + climbWays[i-2];
    } else {
      climbWays[i] = climbWays[i-1]+1;
    }
  }
  return climbWays[n];
};
```

#### Answer Two(Memoization Way. 52ms)
```javascript
var climbStairs = function(n) {
  const cache = [0,1,2]
  const getPrev = (x) => {
    let value = cache[x];
    if(value) {
      return value;
    }
    value = climb(x);
    cache[x] = value;
    return value;
  }
  const climb = (k) => {
    if (k === 0) return 0;
    if (k === 1) return 1;
    if (k === 2) return 2;
    return getPrev(k-1) + getPrev(k-2);
  }
  return climb(n);
};
```