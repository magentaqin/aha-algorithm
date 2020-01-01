# Longest Increasing Subsequence
Given an unsorted array of integers, find the length of longest increasing subsequence.

Example:

Input: [10,9,2,5,3,7,101,18]
Output: 4
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4.
Note:

There may be more than one LIS combination, it is only necessary for you to return the length.
Your algorithm should run in O(n2) complexity.

#### Answer One(68ms, Tabulation Way)
* Code
```javascript
var lengthOfLIS = function(nums) {
  if (!nums.length) return 0
  let dp = new Array(nums.length).fill(1), glbMax = 1
  for (let i = 1; i < nums.length; i++) {
      let max = 1
      for (let j = 0; j < i; j++) {
        if (nums[i] > nums[j]) {
          max = Math.max(max, dp[j] + 1)
        }
      }
      dp[i] = max;
      glbMax = Math.max(max, glbMax)
  }
  return glbMax
};
```

* Basic Idea
- `dp[i]` is the length of longest increasing subsequence till i if we take nums[i] as the largest number in this subsequence. (Be careful to the condition. We take the current element as the largest number in this case.)

- `max` is in charge of finding dp[i] value.
Take [3, 6, 9] as an example. 3 and 6 are both smaller than. Max will choose the bigger dp[j]. In this case, dp[0] is 1, and dp[6] is 2. Max will choose `j=1`, and calculate the max value 3.

- `glbMax` is the final answer. It compares every current max value with the global max value.