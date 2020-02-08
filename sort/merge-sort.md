# Merge Sort

#### Basic Idea
* Divide the array into two subarrays(n/2).
* Merge sort the two subarrays.
* Merge the two sorted subarrays.

#### Algorithm
Take `[9, 3, 12, 8, 5]` for example.

First, divide it into two subarrays: `[9, 3]` and `[12, 8, 5]`.

Then, mergeSort `[9, 3]` to `[3, 9]`.

Then, mergeSort `[12, 8, 5]` to `[5, 8, 12]`.

At last, merge `[3, 9]` and `[5, 8, 12]`.

Final result is [3, 5, 8, 9, 12].

```javascript
function merge(left, right) {
  const result = [];
  while(left.length && right.length) {
    if (left[0] <= right[0]) {
      result.push(left.shift())
    } else {
      result.push(right.shift())
    }
  }

  while(left.length) {
    result.push(left.shift())
  }
  while(right.length) {
    result.push(right.shift())
  }
  return result;
}

function mergeSort(arr) {
  const len = arr.length;
  if (len < 2) {
    return arr;
  }
  const middle = Math.floor(len / 2);
  const left = arr.slice(0, middle);
  const right = arr.slice(middle);
  return merge(mergeSort(left), mergeSort(right));
}

const arr = [9, 3, 12, 8, 5];
mergeSort(arr);
```