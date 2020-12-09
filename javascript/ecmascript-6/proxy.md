# Proxy

Введенный в ECMAScript 6, объект [`Proxy`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Proxy) позволяет перехватить и определить пользовательское поведение для определенных операций. Например, получение свойства объекта:

```javascript
var handler = {
  get: function(target, name) {
    return name in target ? target[name] : 42;
}};
var p = new Proxy({}, handler);
p.a = 1;
console.log(p.a, p.b); // 1, 42
```

Объект `Proxy` определяет _target_ \(в данном случае новый пустой объект\) и _handler_ - объект в котором реализована особая _функция-ловушка_ `get`. "Проксированный" таким образом объект, при доступе к его несуществующему свойству вернет не `undefined,` а числовое значение 42.

