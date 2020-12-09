# Reflection

[`Reflect`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Reflect) это встроенный объект, предоставляющий методы для перехватываемых операций JavaScript. Это те же самые методы, что имеются в [обработчиках Proxy](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Proxy/handler). Объект `Reflect` не является функцией.

`Reflect` помогает при пересылке стандартных операций из обработчика к целевому объекту.

Например, метод [`Reflect.has()`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Reflect/has) это тот же [`оператор in`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Operators/in) но в виде функции:

```javascript
Reflect.has(Object, 'assign'); // true
```

#### Улучшенная функция `apply` <a id="&#x423;&#x43B;&#x443;&#x447;&#x448;&#x435;&#x43D;&#x43D;&#x430;&#x44F;_&#x444;&#x443;&#x43D;&#x43A;&#x446;&#x438;&#x44F;_apply"></a>

В ES5 обычно используется метод [`Function.prototype.apply()`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Function/apply) для вызова функции в определенном контексте \(с определенным `this)` и с параметрами, заданными в виде массива \(или [массиво-подобного объекта](https://developer.mozilla.org/ru/docs/Web/JavaScript/Guide/Indexed_collections#Working_with_array-like_objects)\).

```javascript
Function.prototype.apply.call(Math.floor, undefined, [1.75]);
```

С методом [`Reflect.apply`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Reflect/apply) эта операция менее громоздка и более понятна:

```javascript
Reflect.apply(Math.floor, undefined, [1.75]); 
// 1;

Reflect.apply(String.fromCharCode, undefined, [104, 101, 108, 108, 111]);
// "hello"

Reflect.apply(RegExp.prototype.exec, /ab/, ['confabulation']).index;
// 4

Reflect.apply(''.charAt, 'ponies', [3]);
// "i"
```

####   <a id="&#x41F;&#x440;&#x43E;&#x432;&#x435;&#x440;&#x43A;&#x430;_&#x443;&#x441;&#x43F;&#x435;&#x448;&#x43D;&#x43E;&#x441;&#x442;&#x438;_&#x43E;&#x43F;&#x440;&#x435;&#x434;&#x435;&#x43B;&#x435;&#x43D;&#x438;&#x44F;_&#x43D;&#x43E;&#x432;&#x43E;&#x433;&#x43E;_&#x441;&#x432;&#x43E;&#x439;&#x441;&#x442;&#x432;&#x430;"></a>

