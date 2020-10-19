---
description: >-
  Директива let объявляет переменную с блочной областью видимости с возможностью
  инициализировать её значением.
---

# Let

### Синтаксис <a id="&#x421;&#x438;&#x43D;&#x442;&#x430;&#x43A;&#x441;&#x438;&#x441;"></a>

```javascript
let var1 [= value1] [, var2 [= value2]] [, ..., varN [= valueN]];
```

#### Параметры <a id="&#x41F;&#x430;&#x440;&#x430;&#x43C;&#x435;&#x442;&#x440;&#x44B;"></a>

`var1`, `var2`, …, `varN`Имя переменной. Может использоваться любой допустимый идентификатор.`value1`, `value2`, …, `valueN`Значение переменной. Любое допустимое выражение.

### Описание <a id="&#x41E;&#x43F;&#x438;&#x441;&#x430;&#x43D;&#x438;&#x435;"></a>

Директива **`let`** позволяет объявить локальную переменную с областью видимости, ограниченной текущим блоком кода . В отличие от ключевого слова [`var`](https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Statements/var), которое объявляет переменную глобально или локально во всей функции, независимо от области блока.

Объяснение, почему было выбрано название "**let**" можно найти [здесь](https://stackoverflow.com/questions/37916940/why-was-the-name-let-chosen-for-block-scoped-variable-declarations-in-javascri).

#### Правила области видимости <a id="&#x41F;&#x440;&#x430;&#x432;&#x438;&#x43B;&#x430;_&#x43E;&#x431;&#x43B;&#x430;&#x441;&#x442;&#x438;_&#x432;&#x438;&#x434;&#x438;&#x43C;&#x43E;&#x441;&#x442;&#x438;_2"></a>

Областью видимости переменных, объявленных ключевым словом `let`, является блок, в котором они объявлены, и все его подблоки. В этом работа директива `let` схожа с работой директивы `var`. Основная разница заключается в том, что областью видимости переменной, объявленной директивой `var`, является вся функция, в которой она объявлена:

```javascript
function varTest() {
  var x = 1;
  if (true) {
    var x = 2;  // та же переменная!
    console.log(x);  // 2
  }
  console.log(x);  // 2
}

function letTest() {
  let x = 1;
  if (true) {
    let x = 2;  // другая переменная
    console.log(x);  // 2
  }
  console.log(x);  // 1
}
```

####  <a id="sect1"></a>

#### Чище код во вложенных функциях <a id="&#x427;&#x438;&#x449;&#x435;_&#x43A;&#x43E;&#x434;_&#x432;&#x43E;_&#x432;&#x43B;&#x43E;&#x436;&#x435;&#x43D;&#x43D;&#x44B;&#x445;_&#x444;&#x443;&#x43D;&#x43A;&#x446;&#x438;&#x44F;&#x445;"></a>

`let` иногда делает код чище при использовании вложенных функций.

```javascript
var list = document.getElementById("list");

for (let i = 1; i <= 5; i++) {
  let item = document.createElement('li');
  item.appendChild(document.createTextNode('Item ' + i));

  item.onclick = function(ev) {
    console.log('Item ' + i + ' is clicked.');
  };
  list.appendChild(item);
}

// чтобы получить такой же эффект с использованием 'var'
// необходимо создать новый контекст
// используя замыкание, чтобы сохранить значение неизменённым
for (var i = 1; i <= 5; i++) {
  var item = document.createElement("li");
  item.appendChild(document.createTextNode("Item " + i));

    (function(i){
        item.onclick = function(ev) {
            console.log('Item ' + i + ' is clicked.');
        };
    })(i);
  list.appendChild(item);
}
```

Пример выше будет выполнен как и ожидается, так как пять экземпляров внутренней функции \(анонимной\) будут ссылаться на пять разных экземпляров переменной `i`. Пример будет выполнен неверно, если заменить директиву `let` на `var,` или удалить переменную `i` из параметров вложенной функции и использовать внешнюю переменную `i` во внутренней функции.

На верхнем уровне скриптов и функций `let, в отличии от var, не создает свойства на глобальном объекте`. Например:

```javascript
var x = 'global_x';
let y = 'global_y';
console.log(this.x); // 'global_x'
console.log(this.y); // undefined
```

В выводе программы будет отображено слово "global\_x" для `this.x`, но `undefined` для `this.y`.

#### Эмуляция приватных членов <a id="&#x42D;&#x43C;&#x443;&#x43B;&#x44F;&#x446;&#x438;&#x44F;_&#x43F;&#x440;&#x438;&#x432;&#x430;&#x442;&#x43D;&#x44B;&#x445;_&#x447;&#x43B;&#x435;&#x43D;&#x43E;&#x432;"></a>

При взаимодействии с [конструкторами](https://developer.mozilla.org/en-US/docs/Glossary/Constructor) можно использовать выражение **`let`** чтобы открыть доступ к одному или нескольким приватным членам через использование [замыканий](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures):

```javascript
var SomeConstructor;

{
    let privateScope = {};

    SomeConstructor = function SomeConstructor() {
        this.someProperty = 'foo';
        privateScope.hiddenProperty = 'bar';
    }

    SomeConstructor.prototype.showPublic = function() {
        console.log(this.someProperty); // foo
    }

    SomeConstructor.prototype.showPrivate = function() {
        console.log(privateScope.hiddenProperty); // bar
    }

}

var myInstance = new SomeConstructor();

myInstance.showPublic();
myInstance.showPrivate();

console.log(privateScope.hiddenProperty); // error
```

Эта техника позволяет получить только "статичное" приватное состояние - в примере выше, все экземпляры полученные из конструктора `SomeConstructor` будут ссылаться на одну и ту же область видимости `privateScope`.

#### Временные мертвые зоны и ошибки при использовании `let` <a id="&#x412;&#x440;&#x435;&#x43C;&#x435;&#x43D;&#x43D;&#x44B;&#x435;_&#x43C;&#x435;&#x440;&#x442;&#x432;&#x44B;&#x435;_&#x437;&#x43E;&#x43D;&#x44B;_&#x438;_&#x43E;&#x448;&#x438;&#x431;&#x43A;&#x438;_&#x43F;&#x440;&#x438;_&#x438;&#x441;&#x43F;&#x43E;&#x43B;&#x44C;&#x437;&#x43E;&#x432;&#x430;&#x43D;&#x438;&#x438;_let"></a>

Повторное объявление той же переменной в том же блоке или функции приведет к выбросу исключения [SyntaxError](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/SyntaxError).

```javascript
if (x) {
  let foo;
  let foo; // SyntaxError thrown.
}
```

В стандарте ECMAScript 2015 переменные, объявленные директивой let, переносятся в начало блока. Но если вы сошлетесь в блоке на переменную, до того как она объявлена директивой let, то это приведет к выбросу исключения [`ReferenceError`](https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Global_Objects/ReferenceError), потому что переменная находится во "временной мертвой зоне" с начала блока и до места ее объявления. \(В отличии от переменной, объявленной через `var`, которая просто будет содержать значение `undefined`\)

```javascript
function do_something() {
  console.log(bar); // undefined
  console.log(foo); // ReferenceError: foo is not defined
  var bar = 1;
  let foo = 2;
}
```

Вы можете столкнуться с ошибкой в операторах блока  [`switch`](https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Statements/switch), так как он имеет только один подблок.

```javascript
switch (x) {
  case 0:
    let foo;
    break;

  case 1:
    let foo; // Выброс SyntaxError из-за повторного объявления переменной
    break;
}
```

#### `Использование let в циклах` `for` <a id="&#x418;&#x441;&#x43F;&#x43E;&#x43B;&#x44C;&#x437;&#x43E;&#x432;&#x430;&#x43D;&#x438;&#x435;_let_&#x432;_&#x446;&#x438;&#x43A;&#x43B;&#x430;&#x445;_for"></a>

Вы можете использовать ключевое слово `let` для привязки переменных к локальной области видимости цикла `for`. Разница с использованием `var` в заголовке цикла `for`, заключается в том, что переменные объявленные `var`, будут видны во всей функции, в которой находится этот цикл.

```javascript
var i=0;
for ( let i=i ; i < 10 ; i++ ) {
  console.log(i);
}
```

#### Правила области видимости <a id="&#x41F;&#x440;&#x430;&#x432;&#x438;&#x43B;&#x430;_&#x43E;&#x431;&#x43B;&#x430;&#x441;&#x442;&#x438;_&#x432;&#x438;&#x434;&#x438;&#x43C;&#x43E;&#x441;&#x442;&#x438;_3"></a>

```javascript
for (let expr1; expr2; expr3) statement
```

В этом примере expr2, expr3, statement  заключены в неявный блок, который содержит блок локальных переменных, объявленых конструкцией `let` _`expr1`_. Пример приведен выше.

### Примеры <a id="&#x41F;&#x440;&#x438;&#x43C;&#x435;&#x440;&#x44B;"></a>

#### `let` vs `var` <a id="let_vs_var"></a>

Когда let используется внутри блока, то область видимости переменной ограничивается этим блоком. Напомним, что отличие заключается в том, что областью видимости переменных, объявленных диретивой var, является вся функция, в которой они были объявлены.

```javascript
var a = 5;
var b = 10;

if (a === 5) {
  let a = 4; // The scope is inside the if-block
  var b = 1; // The scope is inside the function

  console.log(a);  // 4
  console.log(b);  // 1
} 

console.log(a); // 5
console.log(b); // 1
```

#### `let` в циклах <a id="let_&#x432;_&#x446;&#x438;&#x43A;&#x43B;&#x430;&#x445;"></a>

Вы можете использовать ключевое слово `let` для привязки переменных к локальной области видимости цикла `for`, вместо того что бы использовать глобальные переменные \(объявленные с помощью `var`\).

```javascript
for (let i = 0; i<10; i++) {
  console.log(i); // 0, 1, 2, 3, 4 ... 9
}

console.log(i); // i is not defined
```

### Нестандартизированные расширения `let` <a id="&#x41D;&#x435;&#x441;&#x442;&#x430;&#x43D;&#x434;&#x430;&#x440;&#x442;&#x438;&#x437;&#x438;&#x440;&#x43E;&#x432;&#x430;&#x43D;&#x43D;&#x44B;&#x435;_&#x440;&#x430;&#x441;&#x448;&#x438;&#x440;&#x435;&#x43D;&#x438;&#x44F;_let"></a>

#### `let` блок <a id="let_&#x431;&#x43B;&#x43E;&#x43A;"></a>

`Поддержка let` блоков была убрана в Gecko 44  [баг 1023609](https://bugzilla.mozilla.org/show_bug.cgi?id=1023609).

**let блок** предоставляет способ, ассоциировать значения с перемеными внутри области видимости этого блока, без влияния на значения переменных с теми же именами вне этого блока.

**Синтаксис**

```javascript
let (var1 [= value1] [, var2 [= value2]] [, ..., varN [= valueN]]) block;
```

**Описание**

**`let`** блок предоставляет локальную область видимости для переменных. Работа его заключается в привязке нуля или более переменных к области видимости этого блока кода, другими словами, он является блоком операторов. Отметим, что область видимости переменных, объявленных директивой `var`, в **блоке `let`**, будет той же самой, что и если бы эти переменные были объявленны вне **блока `let`**, иными словами областью видимости таких переменных по-прежнему является функция. Скобки в **блоке `let`** являются обязательными. Опускание их приведет к синтаксической ошибке.

**Пример**

```javascript
var x = 5;
var y = 0;

let (x = x+10, y = 12) {
  console.log(x+y); // 27
}

console.log(x + y); // 5
```

Правила для этого блока кода аналогичны как и для любого другого блока кода в JavaScript. Он может содержать свои локальные переменные, объявленные `let`.

**Правила области видимости**

Областью видимости переменных, объявленных директивой `let`, в **блоке `let`** является сам блок и все подблоки в нем, если они не содержат объявлений переменных с теми же именами. 

#### `let` выражения <a id="let_&#x432;&#x44B;&#x440;&#x430;&#x436;&#x435;&#x43D;&#x438;&#x44F;"></a>

`Поддержка let выражений` была убрана в Gecko 41  [баг 1023609](https://bugzilla.mozilla.org/show_bug.cgi?id=1023609).

**`let выражение`** позволяет объявить переменные с областью видимости ограниченной одним выражением.

**Синтаксис**

```javascript
let (var1 [= value1] [, var2 [= value2]] [, ..., varN [= valueN]]) expression;
```

**Пример**

Вы можете использовать let для объявления переменных, областью видимости которых является только одно выражение:

```javascript
var a = 5;
let(a = 6) console.log(a); // 6
console.log(a); // 5
```

**Правила области видимости**

В данном **`let` выражении**:

```javascript
let (decls) expr
```

_`expr`_ оборачивается в неявный блок.

{% embed url="https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Statements/let" %}



