---
description: create function returns array of unique elements
---

# Unique array

```javascript
function unique(arr) {
  return arr.filter((value, index, array) => array.indexOf(value) === index);
}

let strings = [
  "кришна",
  "кришна",
  "харе",
  "харе",
  "харе",
  "харе",
  "кришна",
  "кришна",
  ":-O"
];

console.log(unique(strings));
```



