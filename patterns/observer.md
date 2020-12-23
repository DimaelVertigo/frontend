---
description: Формирует зависимости один к многим
---

# Observer

Шаблон Observer \(или "Издатель-подписчик"\) - наиболее часто встречающийся шаблон, где есть пользовательский интерфейс. С помощью шаблона Observer можно создать механизм у объекта, который позволит получать сообщения от других объектов об изменениях их состояния, тем самым наблюдая за ними. В JavaScript этот шаблон еще называют "Механизм собственных событий". Например, возьмем кнопку: **кнопка** - это _издатель_, потому что, когда мы нажимаем на кнопку, происходит событие. На это событие должен отреагировать **обработчик**, который является _подписчиком_.

Задача Издателя - определить хранилище для функций и определить интерфейс, в который можно было бы добавить/убрать эти функции. Задача Подписчика - определить момент, когда взять из этого хранилища функцию и запустить ее.

Функция makePublisher\(obj\) просто перегоняет все свойств из эталонного Издателя \(объекта publisher\) в тот, объект, который мы подали на вход функции \(передали в качестве аргумента\).

```javascript
// объект-издатель - содержит методы для создания подписчиков и оповещения их о изменениях.
var publisher = {

    // коллекция подписчиков
    // все подписчики хранятся в виде массива функций.
    subscribers: {
        // событие defaultEvent на которое пока нет подписчиков
        defaultEvent: []
    },

    // метод для добавления подписчиков fn - функция обработчик, event - имя события, на которое вешается обработчик.
    subscribe: function (fn, event) {

        // если имя события не было указано - рассматриваем вызов метода как подписку на событие по умолчанию.
        event = event || 'defaultEvent';

        // если в коллекции подписчиков еще нет подписчиков на данное событие то добавить свойство заполнив его пустым массивом.
        if (typeof this.subscribe[event] === "undefined") {
            this.subscribers[event] = [];
        }
        // добавление подписчика на событие
        this.subscribers[event].push(fn);
    },

    // метод инициирует событие указанное в первом параметре
    publish: function (args, type) {
        this.visitSubscribers('publish', args, type);
    },

    // метод удаляет указанную функцию подписчика
    unsubscribe: function (fn, type) {
        this.visitSubscribers('unsubscribe', fn, type);
    },

    // вспомогательный метод для работы с подписчиками
    visitSubscribers: function (action, arg, event) {
        var eventType = event || 'defaultEvent',
            subscribers = this.subscribers[eventType],
            i,
            max = subscribers.length;

        for (i = 0; i < max; i++) {
            if (action == 'publish') {
                // запуск событияе - вызов функций-обработчиков всех подписчиков
                subscribers[i](arg);
            } else {
                // удаление определенного подписчика из массива функций-обработчиков подписчиков
                if (subscribers[i] === arg) {
                    subscribers.splice(i, 1);
                }
            }
        }
    }
}

// метод для преобразования любого объекта в издателя
function makePublicsher(obj) {
    var i;
    for (i in publisher) {
        if (publisher.hasOwnProperty(i) && typeof publisher[i] === "function") {
            obj[i] = publisher[i];
        }
    }

    obj.subscribers = { any: [] };
}

// объект который будет преобразован в издателя.
var button = {
    click: function () {
        // запуск события click c аргументами 123
        this.publish('123', 'click');
    },

    doubleClick: function () {
        // запуск события doubleClick c аргументами abc
        this.publish('abc', 'doubleClick');
    }
}

// превращаем button в издателя
makePublicsher(button);

// объект с функциями-обработчиками 
var handlerObject = {
    handler1: function (e) {
        document.write("handler 1 " + e + " <br />");
    },

    handler2: function (e) {
        document.write("handler 2 " + e + " <br />");
    }
}

button.subscribe(handlerObject.handler1, "click");
button.subscribe(handlerObject.handler2, "doubleClick");

button.click();
button.click();
button.click();
button.doubleClick();

button.unsubscribe(handlerObject.handler1, "click");
button.click();
```



