# Fibonacci

```javascript
const fibonacci = n => {
  return n > 1 ? fibonacci(n - 1) + fibonacci(n - 2) : n;
};

console.log(fibonacci(7))
```

