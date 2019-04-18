# Army

```javascript
function makeArmy() {

  var shooters = [];

  for (var i = 0; i < 10; i++) {
    var shooter = function() { // функция-стрелок
      alert( i ); // выводит свой номер
    };
    shooters.push(shooter);
  }

  return shooters;
}

var army = makeArmy();

army[0](); // стрелок выводит 10, а должен 0
army[5](); // стрелок выводит 10...
// .. все стрелки выводят 10 вместо 0,1,2...9
```

Решение 1 привязать значение непосредственно к функции-стрелку

```javascript
function makeArmy() {

  var shooters = [];

  for (var i = 0; i < 10; i++) {

    var shooter = function me() {
      alert( me.i );
    };
    shooter.i = i;

    shooters.push(shooter);
  }

  return shooters;
}

var army = makeArmy();

army[0](); // 0
army[1](); // 1
```

Решение 2 - использовать дополнительную функцию для того, чтобы «поймать» текущее значение `i`

```javascript
function makeArmy() {

  var shooters = [];

  for (var i = 0; i < 10; i++) {

    var shooter = (function(x) {

      return function() {
        alert( x );
      };

    })(i);

    shooters.push(shooter);
  }

  return shooters;
}

var army = makeArmy();

army[0](); // 0
army[1](); // 1
```



