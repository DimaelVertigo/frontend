# Stack

```javascript
const braceToIndex = {
  '[': -1,
  '{': -2,
  '<': -3,
  '(': -4,
  ']': 1,
  '}': 2,
  '>': 3,
  ')': 4
};

const check = str => {
  const stack = [];
  
  for (const char of str) {
    const index = braceToIndex[char];
    const len = stack.length;
    if (len && stack[len - 1] === -index) { // если стек не пуст и последний элемент стека содержит противоположную скобку,
      stack.pop(); // то удаляю последний элемент из стека
    } else {
      stack.push(index);
    }
  }

  return stack.length === 0; // вернем true, если стек пуст
}

console.log(check('({}))'));
```

