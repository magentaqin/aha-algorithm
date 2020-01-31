# [Majority Element](https://leetcode.com/problems/majority-element/)
Given an array of size n, find the majority element. The majority element is the element that appears more than ⌊ n/2 ⌋ times.

You may assume that the array is non-empty and the majority element always exist in the array.

Example 1:

Input: [3,2,3]
Output: 3
Example 2:

Input: [2,2,1,1,1,2,2]
Output: 2

#### Solution
```javascript
var majorityElement = function(nums) {
  const numsCount = {};
  const majorityTimes = Math.ceil(nums.length / 2);
  let element;
  nums.every(item => {
    if (numsCount[item]) {
      numsCount[item] = numsCount[item] + 1;
    } else {
      numsCount[item] = 1;
    }
    if (numsCount[item] === majorityTimes) {
      element = item;
      return false;
    }
    return true;
  })
  return element;
};
```