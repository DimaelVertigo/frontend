---
description: >-
  Метод slice() возвращает новый массив, содержащий копию части исходного
  массива.
---

# custom slice\(\)

> arr.slice\(\[begin\[, end\]\]\)

```javascript
function slice(array, start, end) {
  let result = [];
  let len = array.length;

  if (start < 0) {
    start = len + start;
  }
  if (end < 0) {
    end = len + end;
  }
  if (start < end) {
    for (let i = start; i < end; i++) {
      result.push(array[i]);
    }
  } else if (start === undefined && end !== undefined) {
    for (let i = end; i < len; i++) {
      result.push(array[i]);
    }
  } else if (end === undefined && start !== undefined) {
    for (let i = start; i < len; i++) {
      result.push(array[i]);
    }
  } else if (start > end || start === end) {
    return [];
  }
  return result;
}

console.log(slice(array, 0, 1));
```

