# Quick Sort

#### Basic Idea
* Select pivot element.
* Compare rest elements with this pivot element. If elements are greater, move them to the right side of the pivot element. Else, move them to the left side of the pivot element.
* Perform the same operations on the left side and right side of the pivot element.


#### Algorithm
Choose the middle element as the pivot element.
After calling `partition` function, the array is basically "qualified". That is, elements that are smaller than pivot element are on the left side, and elements that are greater than pivot element are on the right side.
To be mentioned, in the "qualified" array, the pivot element you picked at the beginning has been changed.

In the example, the pivot element is 7. After calling `partition`, the qualified array is `[5, 3, 2, 6, 7, 9]`, and the pivot index changes from `2` to `4`. Next, you only have to peform the same action on `[5, 3, 2, 6]` and `[7, 9]`.

Edge case: [1, 1, 1]

```javascript
var items = [5, 3, 7, 6, 2, 9];

function swap(items, leftPointer, rightPointer) {
  const temp = items[leftPointer];
  items[leftPointer] = items[rightPointer];
  items[rightPointer] = temp;
}

function partition(items, leftPointer, rightPointer) {
  const pivotIndex = Math.floor((leftPointer + rightPointer) / 2);
  const pivot = items[pivotIndex]; // middle element
  let i = leftPointer;
  let j = rightPointer;
  // notice i <= j, not i < j
  while (i <= j) {
    // iterate array. stop at the element that are equal or greater than pivot.
    while(items[i] < pivot) {
      i++;
    }

    // iterate array. stop at the element that are equal or less tha pivot.
    while(items[j] > pivot) {
      j--;
    }

    // swap these unqualified elements and move on. notice i<=j, not i < j.
    if (i <= j) {
      swap(items, i, j);
      i++;
      j--;
    }
  }
  return i;
}

function quickSort(items, leftPointer, rightPointer) {
  let index;
  index = partition(items, leftPointer, rightPointer);
  // sort left side
  if (leftPointer < index - 1) {
    quickSort(items, leftPointer, index - 1);
  }
  // sort right side
  if (index < rightPointer) {
    quickSort(items, index, rightPointer);
  }
  return items;
}

const sortedArray = quickSort(items, 0, items.length - 1);
console.log(sortedArray);
```