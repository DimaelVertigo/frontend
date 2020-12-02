# Memo \[code\]

```javascript
// простая функция, прибавляющая 10 к переданному ей числу
const add = (n) => (n + 10);
add(9);
// аналогичная функция с мемоизацией
const memoizedAdd = () => {
  let cache = {};
  return (n) => {
    if (n in cache) {
      console.log('Fetching from cache');
      return cache[n];
    }
    else {
      console.log('Calculating result');
      let result = n + 10;
      cache[n] = result;
      return result;
    }
  }
}
// эту функцию возвратит memoizedAdd
const newAdd = memoizedAdd();
console.log(newAdd(9)); // вычислено
console.log(newAdd(9)); // взято из кэша
```

