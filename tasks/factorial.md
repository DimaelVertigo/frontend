# Factorial

```javascript
const factorial = n => {
  return n === 1 ? n : n * factorial(n - 1);
}

console.log(factorial(5));
```

Можно использовать мемоизацию, чтобы не пересчитывать значения

{% page-ref page="memo-code.md" %}



