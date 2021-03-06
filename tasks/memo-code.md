# Memo, cashing

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

Переменная `cache` может хранить данные между вызовами функции, так как она определена в [замыкании](https://developer.mozilla.org/en/docs/Web/JavaScript/Closures).

С использованием `Map()`

```javascript
const add = n => n + 10;
add(9);

const memoizedAdd = () => {
  let cache = new Map();

  return n => {
    if (cache.has(n)) {
      console.log("Fetching from cache");
      return cache.get(n);
    } else {
      console.log("Calculating result");
      let result = n + 10;
      cache.set(n, result);
      return result;
    }
  };
};
// эту функцию возвратит memoizedAdd
const newAdd = memoizedAdd();
console.log(newAdd(9)); // вычислено
console.log(newAdd(9)); // взято из кэша
```

Функция `memoize` способна превращать другие функции в их эквиваленты с мемоизацией

```javascript
// простая чистая функция, которая возвращает сумму аргумента и 10
const add = (n) => (n + 10);
console.log('Simple call', add(3));
// простая функция, принимающая другую функцию и
// возвращающая её же, но с мемоизацией
const memoize = (fn) => {
  let cache = {};
  return (...args) => {
    let n = args[0];  // тут работаем с единственным аргументом
    if (n in cache) {
      console.log('Fetching from cache');
      return cache[n];
    }
    else {
      console.log('Calculating result');
      let result = fn(n);
      cache[n] = result;
      return result;
    }
  }
}
// создание функции с мемоизацией из чистой функции 'add'
const memoizedAdd = memoize(add);
console.log(memoizedAdd(3));  // вычислено
console.log(memoizedAdd(3));  // взято из кэша
console.log(memoizedAdd(4));  // вычислено
console.log(memoizedAdd(4));  // взято из кэша
```



### Мемоизация рекурсивных функций

```javascript
// уже знакомая нам функция memoize
const memoize = fn => {
  let cache = {};
  return (...args) => {
    let n = args[0];
    if (n in cache) {
      console.log("Fetching from cache", n);
      return cache[n];
    } else {
      console.log("Calculating result", n);
      let result = fn(n);
      cache[n] = result;
      return result;
    }
  };
};
const factorial = memoize(n => {
  return n === 1 ? n : n * factorial(n - 1);
});

console.log(factorial(5)); // вычислено
console.log(factorial(6)); // вычислено для 6, но для предыдущих значений взято из кэша
```

