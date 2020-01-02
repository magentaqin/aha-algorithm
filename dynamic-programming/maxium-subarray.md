# [Maxium Subarray](https://leetcode.com/problems/maximum-subarray/)
Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

Example:

Input: [-2,1,-3,4,-1,2,1,-5,4],
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.
Follow up:

If you have figured out the O(n) solution, try coding another solution using the divide and conquer approach, which is more subtle.

#### Solution One(DP, Runtime 64ms)
`dp[i]` indicates the max value you can get when moves to `i` index of nums array.

The tricky part is that you need `currentSum` to help you decide whether you need prev subarray. (Because the subarray must be contiguous, and the current value must be considered.)

```javascript
var maxSubArray = function(nums) {
  const dp = Array(nums.length).fill(-Infinity);
  dp[0] = nums[0];
  let currentSum = nums[0]
  for (let i = 1; i < nums.length; i++) {
    const current = nums[i];
    const prevCurrentSum = currentSum;
    currentSum = Math.max(current, prevCurrentSum + current);
    dp[i] = Math.max(currentSum, dp[i-1]);
  }
  return dp[nums.length-1];
};
```

Take input `[-2,1,-3,4,-1,2,1]` as an example:

| i    | current | prevCurrentSum | currentSum | dp[i] |
| ---- | ------- | :------------- | ---------- | ----- |
| 1    | 1       | -2             | 1          | 1     |
| 2    | -3      | 1              | -2         | 1     |
| 3    | 4       | -2             | 4          | 4     |
| 4    | -1      | 4              | 3          | 4     |
| 5    | 2       | 3              | 5          | 5     |
| 6    | 1       | 5              | 6          | 6     |



#### Solution Two(Divide and Conquer)