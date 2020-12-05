# custom map\(\)

```javascript
const array = [1, 2, 3, 4, 5];

Array.prototype.myMap = function(fn) {
  const len = this.length;
  const result = new Array(len);

  for (let i = 0; i < len; i++) {
    result[i] = fn(this[i], i, this);
  }
  return result;
};

console.log(array.myMap(item => item + 1)); 
// [2, 3, 4, 5, 6]
```

