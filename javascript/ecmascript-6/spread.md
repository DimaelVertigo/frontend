# Spread

**Spread syntax** позволяет расширить доступные для итерации элементы \(например, массивы или строки\) в местах

* для функций: где ожидаемое количество аргументов для вызовов функций равно нулю или больше нуля
* для элементов \(литералов массива\)
* для выражений объектов: в местах, где количество пар "ключ-значение" должно быть равно нулю или больше \(для объектных литералов\)

```javascript
function sum(x, y, z) {
  return x + y + z;
}

const numbers = [1, 2, 3];

console.log(sum(...numbers));
// expected output: 6

console.log(sum.apply(null, numbers));
// expected output: 6

```

### Синтаксис <a id="&#x421;&#x438;&#x43D;&#x442;&#x430;&#x43A;&#x441;&#x438;&#x441;"></a>

Для вызовов функций:

```javascript
myFunction(...iterableObj);
```

Для литералов массива или строк:

```javascript
[...iterableObj, '4', 'five', 6];
```

Для литералов объекта \(новое в ECMAScript 2018\):

```javascript
let objClone = { ...obj };
```

### Примеры <a id="&#x41F;&#x440;&#x438;&#x43C;&#x435;&#x440;&#x44B;"></a>

#### Spread в вызовах функций <a id="Spread_&#x432;_&#x432;&#x44B;&#x437;&#x43E;&#x432;&#x430;&#x445;_&#x444;&#x443;&#x43D;&#x43A;&#x446;&#x438;&#x439;"></a>

**Замена apply**

Обычно используют [`Function.prototype.apply`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Function/apply) в случаях, когда хотят использовать элементы массива в качестве аргументов функции.

```javascript
function myFunction(x, y, z) { }
var args = [0, 1, 2];
myFunction.apply(null, args);
```

С **spread syntax** вышеприведенное можно записать как:

```javascript
function myFunction(x, y, z) { }
var args = [0, 1, 2];
myFunction(...args);
```

Любой аргумент в списке аргументов может использовать **spread syntax**, и его можно использовать несколько раз.

```javascript
function myFunction(v, w, x, y, z) { }
var args = [0, 1];
myFunction(-1, ...args, 2, ...[3]);
```

**Apply для new**

Вызывая конструктор через ключевое слово `new`, невозможно использовать массив и `apply` **напрямую** \(`apply` выполняет `[[Call]]`, а не `[[Construct]]`\).Однако благодаря spread syntax, массив может быть с легкостью использован со словом `new:`

```javascript
var dateFields = [1970, 0, 1];  // 1 Jan 1970
var d = new Date(...dateFields);
```

Чтобы использовать `new` с массивом параметров без spread syntax, вам потребуется использование частичного применения:

```javascript
function applyAndNew(constructor, args) {
   function partial () {
      return constructor.apply(this, args);
   };
   if (typeof constructor.prototype === "object") {
      partial.prototype = Object.create(constructor.prototype);
   }
   return partial;
}


function myConstructor () {
   console.log("arguments.length: " + arguments.length);
   console.log(arguments);
   this.prop1="val1";
   this.prop2="val2";
};

var myArguments = ["hi", "how", "are", "you", "mr", null];
var myConstructorWithArguments = applyAndNew(myConstructor, myArguments);

console.log(new myConstructorWithArguments);
// (internal log of myConstructor):           arguments.length: 6
// (internal log of myConstructor):           ["hi", "how", "are", "you", "mr", null]
// (log of "new myConstructorWithArguments"): {prop1: "val1", prop2: "val2"}
```

#### Spread в литералах массива[Раздел](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Operators/Spread_syntax#Spread_%D0%B2_%D0%BB%D0%B8%D1%82%D0%B5%D1%80%D0%B0%D0%BB%D0%B0%D1%85_%D0%BC%D0%B0%D1%81%D1%81%D0%B8%D0%B2%D0%B0) <a id="Spread_&#x432;_&#x43B;&#x438;&#x442;&#x435;&#x440;&#x430;&#x43B;&#x430;&#x445;_&#x43C;&#x430;&#x441;&#x441;&#x438;&#x432;&#x430;"></a>

**Более мощный литерал массива**

Без spread syntax, применение синтаксиса литерала массива для создания нового массива на основе существующего недостаточно и требуется императивный код вместо комбинации методов `push`, `splice`, `concat` и т.д. С spread syntax реализация становится гораздо более лаконичной:

```javascript
var parts = ['shoulders', 'knees']; 
var lyrics = ['head', ...parts, 'and', 'toes']; 
// ["head", "shoulders", "knees", "and", "toes"]
```

Аналогично развертыванию в массиве аргументов, `...` может быть использован повсеместно и многократно в литерале массива.

**Копирование массива**

```javascript
var arr = [1, 2, 3];
var arr2 = [...arr]; // like arr.slice()
arr2.push(4); 

// arr2 becomes [1, 2, 3, 4]
// arr remains unaffected
```

**Примечание:** Spread syntax на самом деле переходит лишь на один уровень глубже при копировании массива. Таким образом, он может не подходить для копирования многоразмерных массивов, как показывает следующий пример: \(также как и [`Object.assign()`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)\)

```javascript
var a = [[1], [2], [3]];
var b = [...a];
b.shift().shift(); // 1
// Now array a is affected as well: [[], [2], [3]]
```

**Лучший способ конкатенации массивов**

Для конкатенации массива часто используется [`Array.concat`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/concat):

```javascript
var arr1 = [0, 1, 2];
var arr2 = [3, 4, 5];
// Append all items from arr2 onto arr1
arr1 = arr1.concat(arr2);
```

С использованием spread syntax:

```javascript
var arr1 = [0, 1, 2];
var arr2 = [3, 4, 5];
arr1 = [...arr1, ...arr2];
```

[`Array.unshift`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/unshift) часто используется для вставки массива значений в начало существующего массива. Без spread syntax:

```javascript
var arr1 = [0, 1, 2];
var arr2 = [3, 4, 5];
// Prepend all items from arr2 onto arr1
Array.prototype.unshift.apply(arr1, arr2) // arr1 is now [3, 4, 5, 0, 1, 2]
```

С использованием spread syntax \[Следует отметить, что такой способ создает новый массив `arr1`. В отличие от [`Array.unshift`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/unshift), исходный массив не мутируется\]:

```javascript
var arr1 = [0, 1, 2];
var arr2 = [3, 4, 5];
arr1 = [...arr2, ...arr1]; // arr1 is now [3, 4, 5, 0, 1, 2]
```

#### Spread в литералах объекта[Раздел](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Operators/Spread_syntax#Spread_%D0%B2_%D0%BB%D0%B8%D1%82%D0%B5%D1%80%D0%B0%D0%BB%D0%B0%D1%85_%D0%BE%D0%B1%D1%8A%D0%B5%D0%BA%D1%82%D0%B0) <a id="Spread_&#x432;_&#x43B;&#x438;&#x442;&#x435;&#x440;&#x430;&#x43B;&#x430;&#x445;_&#x43E;&#x431;&#x44A;&#x435;&#x43A;&#x442;&#x430;"></a>

Предложение [Rest/Spread Properties for ECMAScript](https://github.com/tc39/proposal-object-rest-spread) \(стадия 4\) добавляет свойства spread в [литералы объекта](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Operators/Object_initializer). Оно копирует собственные перечисляемые свойства данного объекта в новый объект.

Поверхностное копирование \(без прототипа\) или объединение объектов теперь возможно с использованием более короткого, чем [`Object.assign()`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Object/assign), синтаксиса.

```javascript
var obj1 = { foo: 'bar', x: 42 };
var obj2 = { foo: 'baz', y: 13 };

var clonedObj = { ...obj1 };
// Object { foo: "bar", x: 42 }

var mergedObj = { ...obj1, ...obj2 };
// Object { foo: "baz", x: 42, y: 13 }
```

Обратите внимание, что [`Object.assign()`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) запускает [setters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/set), а **spread syntax** нет.

Обратите внимание, что вы не можете заменить или имитировать функцию [`Object.assign()`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Object/assign):

```javascript
var obj1 = { foo: 'bar', x: 42 };
var obj2 = { foo: 'baz', y: 13 };
const merge = ( ...objects ) => ( { ...objects } );

var mergedObj = merge ( obj1, obj2);
// Object { 0: { foo: 'bar', x: 42 }, 1: { foo: 'baz', y: 13 } }

var mergedObj = merge ( {}, obj1, obj2);
// Object { 0: {}, 1: { foo: 'bar', x: 42 }, 2: { foo: 'baz', y: 13 } }
```

В приведенном выше примере оператор распространения не работает так, как можно было бы ожидать: он распространяет _массив_ аргументов в литерал _объекта_ благодаря параметру rest.

#### Только для итерируемых объектов[Раздел](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Operators/Spread_syntax#%D0%A2%D0%BE%D0%BB%D1%8C%D0%BA%D0%BE_%D0%B4%D0%BB%D1%8F_%D0%B8%D1%82%D0%B5%D1%80%D0%B8%D1%80%D1%83%D0%B5%D0%BC%D1%8B%D1%85_%D0%BE%D0%B1%D1%8A%D0%B5%D0%BA%D1%82%D0%BE%D0%B2) <a id="&#x422;&#x43E;&#x43B;&#x44C;&#x43A;&#x43E;_&#x434;&#x43B;&#x44F;_&#x438;&#x442;&#x435;&#x440;&#x438;&#x440;&#x443;&#x435;&#x43C;&#x44B;&#x445;_&#x43E;&#x431;&#x44A;&#x435;&#x43A;&#x442;&#x43E;&#x432;"></a>

Spread syntax \( кроме случаев spread properties\) может быть применен только к итерируемым объектам \([iterable](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol/iterator) objects\) :

```javascript
var obj = {'key1': 'value1'};
var array = [...obj]; // TypeError: obj is not iterable
```

#### Spread with many values[Раздел](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Operators/Spread_syntax#Spread_with_many_values) <a id="Spread_with_many_values"></a>

When using spread syntax for function calls, be aware of the possibility of exceeding the JavaScript engine's argument length limit. See [`apply()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply) for more details.

### Rest syntax \(parameters\)[Edit](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Operators/Spread_syntax$edit#Rest_syntax_%28parameters%29)[Раздел](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Operators/Spread_syntax#Rest_syntax_%28parameters%29) <a id="Rest_syntax_(parameters)"></a>

Rest syntax looks exactly like spread syntax, but is used for destructuring arrays and objects. In a way, rest syntax is the opposite of spread syntax: spread 'expands' an array into its elements, while rest collects multiple elements and 'condenses' them into a single element. See [rest parameters.](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions_and_function_scope/rest_parameters)  


{% embed url="https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Operators/Spread\_syntax" %}



