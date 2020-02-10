# Insertion Sort

#### Basic Idea
* Iterate the array from the second element.
* Items before the current element are taken as "sorted items".
* Iterate the "sorted items" from the last one. If it is bigger than the current element, move it to the next position.
* At the end of each outer loop, insert the current element after the inner loop iterator index.

```javascript
function insertionSort(arr) {
  for (let i = 1; i < arr.length; i++) {
    const element = arr[i];
    let j = i - 1;
    while(j >= 0 && arr[j] > element) {
      arr[j+1] = arr[j];
      j--;
    }
    arr[j+1] = element;
  }
  return arr;
}

console.log(insertionSort([3, 44, 38, 5, 15]))
```