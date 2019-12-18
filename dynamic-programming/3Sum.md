# [3Sum](https://leetcode.com/problems/3sum/)
Given an array nums of n integers, are there elements a, b, c in nums such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.

The solution set must not contain duplicate triplets.

Example
```
Given array nums = [-1, 0, 1, 2, -1, -4],

A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```

#### Answer One[POOR ANSWER, O(n^3)]
It contains three loops to find exact x + y + z = 0.
For small arrays like [-1, 0, 1, 2, -1, -4], it works with poor performance. However, for big arrays, it will throw runtime error.
```javascript
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var threeSum = function(nums) {
  const result = []
  // sort array
  nums.sort(function(a, b) { return a - b; });

  const isArrayEqual = (arr1, arr2) => {
    let isEqual = false;
    if (arr1.length === arr2.length) {
      isEqual = arr1.every(item => {
        if (!arr2.includes(item)) {
          return false;
        }
        return true;
      })
    }
    return isEqual;
  }

  for (let indexA = 0; indexA < nums.length; indexA++) {
    let x = nums[indexA]
    for (let indexB = indexA + 1; indexB < nums.length; indexB++) {
      let y = nums[indexB]
      for (let indexC = indexB + 1; indexC < nums.length; indexC++) {
        let z = nums[indexC]
        if (x + y + z === 0) {
          // duplicate array should not be pushed.
          let isExist = false;
          result.some(item => {
            if (isArrayEqual(item, [x, y, z])) {
              isExist = true;
              return true;
            }
            return false;
          })
          if (!isExist) {
            result.push([x, y, z])
          }
        }
      }
    }
  }
  return result;
};
```

#### Answer Two[OPTIMIZED ANSWER, O(n^2)]
This solution only loops the array once to find the first element, and use while loop to find the rest two elements.

* Trick One:
It reduces the outer looped elements length by replacing `nums.length` with `nums.length - 2`. (Simply because the other two elements are y and z.)

* Trick Two:
Unlike the previous solution (indexC loops from indexB+1), this solution puts indexC pointer to the last index.

* Trick Three:
Previous solution only makes use of the `equal` pair. (put the matched x, y, z to result).

This solution not only makes use of the `equal` pair, but also the matched pair.

If `x+y+z < 0`, it means y is not big enough. Then moves the indexB pointer to the next one.

If `x+y+z > 0`, it means z is not small enough. Then moves the indexC pointer to the previous one.

* Trick Four:
Previous solution removes duplication by comparing the incoming [x, y, z] with the one in `result` array. It can not avoid array loops.

This solution removes duplication within the loop process. As the nums are sorted, you only need to compare the current x/y/z with the ajacent x/y/z.

```javascript
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var threeSum = function(nums) {
  const result = [];
  // ascend sort
  nums.sort((a, b) => a - b);

  for (let indexA = 0; indexA < nums.length - 2; indexA++) {
    const x = nums[indexA];

    // if the minimum is above 0, break the loop. Because any elements after x is above 0.
    if (x > 0) break;

    // prevent duplicates. if x is the same as the previous element, then there is no need to find other two elements.
    if (x === nums[indexA - 1]) continue;


    let indexB = indexA + 1;
    let indexC = nums.length - 1;

    while(indexB < indexC) {
      const y = nums[indexB];
      const z = nums[indexC];
      const sum = x + y + z;

      if (sum === 0) {
        result.push([x, y, z])
      }

      if (sum >= 0) {
        // if previous element is the same value, skip the previous element.
        while(nums[indexC - 1] === z) { indexC--; }
        indexC--;
      }

      if (sum <= 0) {
        // if next element is the same value, skip the next element.
        while(nums[indexB+1] === y) { indexB++; }
        indexB++;
      }
    }
  }
  return result;
};
```
