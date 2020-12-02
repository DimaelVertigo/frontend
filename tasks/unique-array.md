---
description: create function returns array of unique elements
---

# Unique array

{% hint style="info" %}
Метод **`filter()`**создаёт новый массив со всеми элементами, прошедшими проверку, задаваемую в передаваемой функции.
{% endhint %}

```javascript
function unique(arr) {
  return arr.filter((value, index, array) => array.indexOf(value) === index);
}

let strings = ['a', 1, 'a', 2, '1']

console.log(unique(strings));
```



{% hint style="info" %}
Объекты **`Set`** позволяют сохранять _уникальные_ значения любого типа, как [примитивы](https://developer.mozilla.org/ru/docs/%D0%A1%D0%BB%D0%BE%D0%B2%D0%B0%D1%80%D1%8C/Primitive), так и другие типы объектов.
{% endhint %}

```javascript
var myArray = ['a', 1, 'a', 2, '1'];


let unique = [...new Set(myArray)];

console.log(unique); // unique is ['a', 1, 2, '1']
```



