# custom map\(\)

> The **`map()`** method creates a new array with the results of calling a provided function on every element in the calling array.

As Array method:

```javascript
const array = [1, 2, 3, 4, 5];

Array.prototype.myMap = function(callback) {
  const len = this.length;
  const result = new Array(len);

  for (let i = 0; i < len; i++) {
    result[i] = callback(this[i], i, this);
  }
  return result;
};

console.log(array.myMap(item => item + 1));
// [2, 3, 4, 5, 6]
```

As a function:

```javascript
const array = [1, 2, 3, 4, 5];

function map(arr, callback) {
  const len = arr.length;
  const result = new Array(len);

  for (let i = 0; i < len; i++) {
    result[i] = callback(arr[i], i, arr);
  }
  return result;
}

console.log(map(array, item => item + 1));
```

