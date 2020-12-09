# Merge two sorted arrays

```javascript
const array = [1, 2, 3, 4, 5];
const array2 = [1, 3, 5, 7];

function mergeTwo(arr1, arr2) {
  return [...arr1, ...arr2].sort((a, b) => a - b);
}
console.log(mergeTwo(array, array2));
```

