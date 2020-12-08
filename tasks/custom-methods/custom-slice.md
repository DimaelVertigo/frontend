---
description: >-
  Метод slice() возвращает новый массив, содержащий копию части исходного
  массива.
---

# custom slice\(\)

> arr.slice\(\[begin\[, end\]\]\)

```javascript
const array = [1, 2, 3, 4, 5];

function slice(array, start = 0, end = array.length) {
  let normStart = start < 0 ? array.length + start : start;
  let normEnd = Math.min(array.length, end < 0 ? array.length + end : end);
  const result = [];

  while (normStart < normEnd) {
    result.push(array[normStart++]);
  }

  return result;
}

console.log(slice(array, 0, 4));
```

