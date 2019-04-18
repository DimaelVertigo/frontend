---
description: >-
  Используя шаблон Декоратор к объекту динамически можно добавить новую
  функциональность на этапе выполнения.
---

# Decorator

```javascript
function Coffee(price) {
    this.price = price || 10;
}

Coffee.prototype.getPrice = function () {
    return this.price;
}

// Метод для добавления декораторов
Coffee.prototype.add = function (decorator) {
    var F = function () { },
        overrides = this.constructor.decorators[decorator],
        i,
        newObj;

    // Прототипом для декоратора устанавливается тот объект, который декорируется.
    F.prototype = this;
    newObj = new F();
    // сохраняем ссылку на тот объект, который декорируем
    newObj.super = F.prototype;

    // Все свойства и методы декоратора копируем в декорируемый объект
    for (i in overrides) {
        if (overrides.hasOwnProperty(i)) {
            newObj[i] = overrides[i];
        }
    }

    return newObj;
}

// Все декораторы будут храниться в свойстве конструктора.
Coffee.decorators = {};

// добавление декораторов
Coffee.decorators.milk = {
    getPrice: function () {
        var price = this.super.getPrice();
        price = price + 2;
        return price;
    }
}

Coffee.decorators.sugar = {
    getPrice: function () {
        var price = this.super.getPrice();
        price = price + 1;
        return price;
    }
}

Coffee.decorators.cinnamon = {
    getPrice: function () {
        var price = this.super.getPrice();
        price = price + 3;
        return price;
    }
}


// Использование объекта созданного с применением шаблона декоратор.
var coffee = new Coffee(10);

coffee = coffee.add("milk");
coffee = coffee.add("sugar");
coffee = coffee.add("cinnamon");

var price = coffee.getPrice();
document.write("Price - " + price);
```

