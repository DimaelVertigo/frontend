---
description: >-
  An isogram is a word that has no repeating letters, consecutive or
  non-consecutive. Implement a function that determines whether a string that
  contains only letters is an isogram. Assume the empty str
---

# Isogram



```javascript
function isIsogram(str) {
  let arr = str.toLowerCase().split('');
  let unique = arr.reduce((x, y) => x.includes(y) ? x : [...x, y], []);
  
  return (arr.join('') === unique.join('')) ? true : false;
}

console.log(isIsogram('ata'));
```

