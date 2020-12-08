# custom filter\(\)

> The **`filter()`** method creates a new array with all elements that pass the test implemented by the provided function.

```javascript
const array = [1, 2, 3, 4, 5];
const array2 = [6, 7, 8, 9, 0];

Array.prototype.myFilter = function(callback, context = this) {
  const result = [];

  for (let i = 0; i < context.length; i++) {
    if (callback(context[i])) {
      result.push(context[i]);
    }
  }
  return result;
};

console.log(array.myFilter(i => i < 3));
console.log(Array.prototype.myFilter(i => i < 3, array2));
```

