---
description: Назначение фабрики - создавать объекты
---

# Factory

С помощью паттерна можно решить следующие проблемы: 

* Выполнение повторяющихся операций при создании объектов.
* Позволяет создавать объекты не зная их тип на этапе компиляции \(важно для языков со статической типизацией\)

```javascript
// Родительский конструктор
function Control() { }

// Метод родителя
Control.prototype.render = function (type) {
    document.write("Control:  " + this.name + "<br /> " + this.control + "<br /> <br />");
}

// Фабричный метод
Control.create = function (type) {
    var ctor = type,
        newControl;

    // Проверка наличия конструктора для указанного типа объекта.
    if (typeof Control[ctor] !== "function") {
        // Выбрасываем исключение если конструктор не найден.
        throw {
            name: "Error",
            message: "Конструктор " + ctor + "не найден."
        };
    }

    // На этом этапе существование конструктора проверено.
    // Устанавливаем для конструктора в качестве прототипа объект Control.
    // Выполняем данную операцию один раз.
    if (typeof Control[ctor].prototype.render !== "function") {
        Control[ctor].prototype = new Control();
    }

    // Создаем экземпляр указанного типа.
    newControl = new Control[ctor];

    // Дополнительно можно выполнить настройку созданного объекта.

    return newControl;
}


// Специализированные конструкторы.
Control.Button = function () {
    this.name = "Button";
    this.control = "<input type='button' value='testButton' />";
}

Control.TextBox = function () {
    this.name = "TextBox";
    this.control = "<input type='text' />";
}

Control.RadioButton = function () {
    this.name = "RadioButton";
    this.control = "<input type='radio' /> RadioButton";
}


// Использование фабрики

var btn = Control.create("Button");
var txt = Control.create("TextBox");
var rbtn = Control.create("RadioButton");

btn.render();
txt.render();
rbtn.render();
```

