# Arrow



**Выражения стрелочных функций** имеют более короткий синтаксис по сравнению с [функциональными выражениями](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Operators/function) и лексически привязаны к значению [this](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Operators/this) \(но не привязаны к собственному [this](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Operators/this), [arguments](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Functions/arguments), [super](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Operators/super), или [new.target](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/new.target)\). Выражение стрелочных функций не позволяют задавать имя, поэтому стрелочные функции [анонимны](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Function/name), если их ни к чему не присвоить.

### Синтаксис <a id="Syntax"></a>

#### Базовый синтаксис <a id="&#x411;&#x430;&#x437;&#x43E;&#x432;&#x44B;&#x439;_&#x441;&#x438;&#x43D;&#x442;&#x430;&#x43A;&#x441;&#x438;&#x441;"></a>

```javascript
(param1, param2, …, paramN) => { statements }
(param1, param2, …, paramN) => expression
// эквивалентно: (param1, param2, …, paramN) => { return expression; }

// Круглые скобки не обязательны для единственного параметра:
(singleParam) => { statements }
singleParam => { statements }

// Функция без параметров нуждается в круглых скобках:
() => { statements }
() => expression 
// Эквивалентно: () => { return expression; }
```

#### Расширенный синтаксис <a id="&#x420;&#x430;&#x441;&#x448;&#x438;&#x440;&#x435;&#x43D;&#x43D;&#x44B;&#x439;_&#x441;&#x438;&#x43D;&#x442;&#x430;&#x43A;&#x441;&#x438;&#x441;"></a>

```javascript
// Когда возвращаете литеральное выражение объекта, заключите тело в скобки
params => ({foo: bar})

// Rest параметры и параметры по умолчанию поддерживаются
(param1, param2, ...rest) => { statements }
(param1 = defaultValue1, param2, …, paramN = defaultValueN) => { statements }

// Деструктуризация тоже поддерживается
var f = ([a, b] = [1, 2], {x: c} = {x: a + b}) => a + b + c;
f();  // 6
```

Подробные примеры синтаксиса можно посмотреть [здесь](http://wiki.ecmascript.org/doku.php?id=harmony:arrow_function_syntax).

### Описание <a id="&#x41E;&#x43F;&#x438;&#x441;&#x430;&#x43D;&#x438;&#x435;"></a>

Смотрите также ["ES6 In Depth: Arrow functions" on hacks.mozilla.org](https://hacks.mozilla.org/2015/06/es6-in-depth-arrow-functions/).

Два фактора повлияли на появление стрелочных функции: более короткий синтаксис и лексика `this`.

#### Короткие функции <a id="&#x41A;&#x43E;&#x440;&#x43E;&#x442;&#x43A;&#x438;&#x435;_&#x444;&#x443;&#x43D;&#x43A;&#x446;&#x438;&#x438;"></a>

В некоторых функциональных шаблонах приветствуются более короткие функции. Сравните:

```javascript
var elements = [
  'Hydrogen',
  'Helium',
  'Lithium',
  'Beryllium'
];

elements.map(function(element) {
  return element.length;
}); // Это выражение вернет массив [8, 6, 7, 9]

// Функцию выше можно записать как стрелочную функцию:
elements.map((element) => {
  return element.length;
}); // [8, 6, 7, 9]

// Если единственным оператором в выражении стрелочной функции является return,
// можно удалить return и окружающие фигурные скобки

elements.map(element => element.length); // [8, 6, 7, 9]

// В данном случае, поскольку нам нужно только свойство length, мы можем использовать деструктуризированный параметр:
// Обратите внимание, что строка `"length"` соответствует свойству, которое мы хотим получить,
// в то время как `lengthFooBArX` это просто имя переменной, которую можно назвать как вы хотите
elements.map(({ "length": lengthFooBArX }) => lengthFooBArX); // [8, 6, 7, 9]

// Это задание деструктуризированного параметра может быть записано, как показано ниже. Тем не менее, обратите внимание,
// что нет строки `"length"`, чтобы выбрать, какое свойство мы хотим получить. Вместо этого в качестве свойства,
// которое мы хотим извлечь из объекта, используется само литеральное имя переменной `length`
elements.map(({ length }) => length); // [8, 6, 7, 9]
```

#### Отсутствие связывания с `this` <a id="&#x41E;&#x442;&#x441;&#x443;&#x442;&#x441;&#x442;&#x432;&#x438;&#x435;_&#x441;&#x432;&#x44F;&#x437;&#x44B;&#x432;&#x430;&#x43D;&#x438;&#x44F;_&#x441;_this"></a>

До появления стрелочных функций, каждая новая функция имела своё значение [`this`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Operators/this) \(новый объект в случае конструктора, undefined в [strict](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Strict_mode) режиме вызова функции, контекст объекта при вызове функции как "метода объекта" и т.д.\). Это очень раздражало при использовании объектно-ориентированного стиля программирования.

```javascript
function Person() {
  // В конструкторе Person() `this` указывает на себя.
  this.age = 0;

  setInterval(function growUp() {
    // В нестрогом режиме, в функции growUp() `this` указывает 
    // на глобальный объект, который отличается от `this`,
    // определяемом в конструкторе Person().
    this.age++;
  }, 1000);
}

var p = new Person();
```

В ECMAScript 3/5, данная проблема решалась присваиванием значения `this` переменной:

```javascript
function Person() {
  var that = this;
  that.age = 0;

  setInterval(function growUp() {
    // Функция с обратным вызовом(callback) содержит переменную that, которая 
    // ссылается на требуемый объект this.
    that.age++;
  }, 1000);
}
```

Кроме этого, может быть создана [привязанная функция](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Function/bind), в которую передаётся требуемое значение [`this`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Operators/this) для функции \(функция `growUp()` в примере выше\).

Стрелочные функции не содержат собственный контекст [`this`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Operators/this), а используют значение [`this`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Operators/this) окружающего контекста. Поэтому нижеприведенный код работает как предполагалось:

```javascript
function Person(){
  this.age = 0;

  setInterval(() => {
    this.age++; // `this` указывает на объект Person
  }, 1000);
}

var p = new Person();
```

**Строгий режим исполнения**

Поскольку значение `this` определяется лексикой, правила строгого режима \([strict mode](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Strict_mode)\) относительно `this` игнорируются:

```javascript
var f = () => { 'use strict'; return this; };
f() === window; // или глобальный объект
```

Все остальные правила строгого режима применяются как обычно.

**Вызов с помощью call или apply**

Так как значение `this` определяется лексикой, вызов стрелочных функций с помощью методов `call()` или `apply()`, даже если передать аргументы в эти методы, не влияет на значение `this`:

```javascript
var adder = {
  base : 1,
    
  add : function(a) {
    var f = v => v + this.base;
    return f(a);
  },

  addThruCall: function(a) {
    var f = v => v + this.base;
    var b = {
      base : 2
    };
            
    return f.call(b, a);
  }
};

console.log(adder.add(1));         // Выводит 2
console.log(adder.addThruCall(1)); // Всё равно выводит 2
```

#### Не имеет собственного объекта arguments <a id="&#x41D;&#x435;_&#x438;&#x43C;&#x435;&#x435;&#x442;_&#x441;&#x43E;&#x431;&#x441;&#x442;&#x432;&#x435;&#x43D;&#x43D;&#x43E;&#x433;&#x43E;_&#x43E;&#x431;&#x44A;&#x435;&#x43A;&#x442;&#x430;_arguments"></a>

Стрелочные функции не имеют собственного объекта arguments, поэтому в теле стрелочных функций arguments будет ссылаться на переменную в окружающей области.

```javascript
var arguments = 42;
var arr = () => arguments;

arr(); // 42

function foo() {
  var f = (i) => arguments[0] + i; // Неявное связывание ссылки arguments 
                                   // стрелочной функции f
                                   // c объектом arguments функции foo
  return f(2);
}

foo(1); // 3
```

В большинстве случаев лучшей заменой объекта arguments в стрелочных функциях являются [rest параметры](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters):

```javascript
function foo() { 
  var f = (...args) => args[0]; 
  return f(2); 
}

foo(1); // 2
```

#### Использование стрелочных функций как методов <a id="&#x418;&#x441;&#x43F;&#x43E;&#x43B;&#x44C;&#x437;&#x43E;&#x432;&#x430;&#x43D;&#x438;&#x435;_&#x441;&#x442;&#x440;&#x435;&#x43B;&#x43E;&#x447;&#x43D;&#x44B;&#x445;_&#x444;&#x443;&#x43D;&#x43A;&#x446;&#x438;&#x439;_&#x43A;&#x430;&#x43A;_&#x43C;&#x435;&#x442;&#x43E;&#x434;&#x43E;&#x432;"></a>

Как показано ранее, стрелочные функции лучше всего подходят для функций без методов. Посмотрим, что будет, когда мы попробуем их использовать как методы:

```javascript
'use strict';
var obj = {
  i: 10,
  b: () => console.log(this.i, this),
  c: function() {
    console.log(this.i, this);
  }
}
obj.b(); // prints undefined, Window {...} (или глобальный объект)
obj.c(); // prints 10, Object {...}
```

Стрелочные функции не объявляют привязку \("bind"\) их контекста `this`. Другой пример включает [`Object.defineProperty()`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty):

```javascript
'use strict';
var obj = {
  a: 10
};

Object.defineProperty(obj, 'b', {
  get: () => {
    console.log(this.a, typeof this.a, this);
    return this.a + 10; 
    // представляет глобальный объект 'Window', но 'this.a' возвращает 'undefined'
  }
});
```

#### Использование оператора `new` <a id="&#x418;&#x441;&#x43F;&#x43E;&#x43B;&#x44C;&#x437;&#x43E;&#x432;&#x430;&#x43D;&#x438;&#x435;_&#x43E;&#x43F;&#x435;&#x440;&#x430;&#x442;&#x43E;&#x440;&#x430;_new"></a>

Стрелочные функции не могут быть использованы как конструктор и вызовут ошибку при использовании с `new`:

```javascript
var a = new (function() {}) 
// переменной "a" будет присвоено значение экземпляра анонимной функции

var b = new (() => {})
// будет выброшено исключениe
// Uncaught TypeError: (intermediate value) is not a constructor
```

#### Использование ключевого слова `yield` <a id="&#x418;&#x441;&#x43F;&#x43E;&#x43B;&#x44C;&#x437;&#x43E;&#x432;&#x430;&#x43D;&#x438;&#x435;_&#x43A;&#x43B;&#x44E;&#x447;&#x435;&#x432;&#x43E;&#x433;&#x43E;_&#x441;&#x43B;&#x43E;&#x432;&#x430;_yield"></a>

Ключевое слово [`yield`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/yield) не может быть использовано в теле стрелочной функции \(за исключением случаев, когда разрешается использовать в функциях, вложенных в тело стрелочной функции\). Как следствие стрелочные функции не могут быть использованы как генераторы.

### Тело функции <a id="&#x422;&#x435;&#x43B;&#x43E;_&#x444;&#x443;&#x43D;&#x43A;&#x446;&#x438;&#x438;"></a>

Тело стрелочной функции может иметь краткую \(concise body\) или блочную \(block body\) форму.

Блочная форма не возвращает значение, необходимо явно вернуть значение.

```javascript
var func = x => x * x;                  // краткий синтаксис, 
                                        // неявно возвращает результат
var func = (x, y) => { return x + y; }; // блочный синтаксис, 
                                        // явно возвращает результат
```

### Возвращаемые объектные строки \(литералы\) <a id="&#x412;&#x43E;&#x437;&#x432;&#x440;&#x430;&#x449;&#x430;&#x435;&#x43C;&#x44B;&#x435;_&#x43E;&#x431;&#x44A;&#x435;&#x43A;&#x442;&#x43D;&#x44B;&#x435;_&#x441;&#x442;&#x440;&#x43E;&#x43A;&#x438;_&#x43B;&#x438;&#x442;&#x435;&#x440;&#x430;&#x43B;&#x44B;"></a>

Помните о том, что возвращаемые [объектные строки](https://developer.mozilla.org/ru/docs/Web/JavaScript/Guide/Grammar_and_types#%D0%9B%D0%B8%D1%82%D0%B5%D1%80%D0%B0%D0%BB_%D0%BE%D0%B1%D1%8A%D0%B5%D0%BA%D1%82%D0%B0/) используют сокращённый синтаксис: `params => {object:literal}` будет работать не так, как ожидается.

```javascript
var func = () => { foo: 1 };               
// Вызов func() возвращает undefined!

var func = () => { foo: function() {} };   
// SyntaxError: function statement requires a name
```

Это происходит потому что код в скобках \({}\) распознаётся как цепочка выражений \(т.е. `foo` трактуется как наименование, а не как ключ в объектной строке\).

Не забывайте оборачивать скобками объектные строки.

```javascript
var func = () => ({ foo: 1 });
```

### Разрывы строк <a id="&#x420;&#x430;&#x437;&#x440;&#x44B;&#x432;&#x44B;_&#x441;&#x442;&#x440;&#x43E;&#x43A;"></a>

Стрелочная функция не может содержать разрывы строк между параметрами и стрелкой.

```javascript
var func = ()
           => 1; 
// SyntaxError: expected expression, got '=>'
```

### Разбор порядка следования <a id="&#x420;&#x430;&#x437;&#x431;&#x43E;&#x440;_&#x43F;&#x43E;&#x440;&#x44F;&#x434;&#x43A;&#x430;_&#x441;&#x43B;&#x435;&#x434;&#x43E;&#x432;&#x430;&#x43D;&#x438;&#x44F;"></a>

Поскольку стрелка в стрелочной функции не является оператором, то стрелочные функции имеют специальные правила разбора \(парсинга\), которые взаимодействуют с [приоритетами операторов](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Operators/Operator_Precedence/)  иначе, чем в обычных функциях.

```javascript
let callback;

callback = callback || function() {}; // ok

callback = callback || () => {};      
// SyntaxError: invalid arrow-function arguments

callback = callback || (() => {});    // ok
```

### Больше примеров <a id="&#x411;&#x43E;&#x43B;&#x44C;&#x448;&#x435;_&#x43F;&#x440;&#x438;&#x43C;&#x435;&#x440;&#x43E;&#x432;"></a>

```javascript
// Пустая стрелочная функция возвращает undefined
let empty = () => {};

(() => 'foobar')(); 
// Вернёт "foobar"
// (Это Immediately Invoked Function Expression 
// смотри 'IIFE' в справочнике)

var simple = a => a > 15 ? 15 : a; 
simple(16); // 15
simple(10); // 10

let max = (a, b) => a > b ? a : b;

// Удобные операции над массивами: filter, map, ...

var arr = [5, 6, 13, 0, 1, 18, 23];

var sum = arr.reduce((a, b) => a + b);  
// 66

var even = arr.filter(v => v % 2 == 0); 
// [6, 0, 18]

var double = arr.map(v => v * 2);       
// [10, 12, 26, 0, 2, 36, 46]

// Более короткие цепочки promise-ов
promise.then(a => {
  // ...
}).then(b => {
   // ...
});

// Стрелочные функции без параметров, которые визуально легче разбирать
setTimeout( () => {
  console.log('Я буду раньше');
  setTimeout( () => {
    // deeper code
    console.log('Я буду позже');
  }, 1);
}, 1);
```

