# LexicalEnvironment

**LexicalEnvironment** - объект переменных куда функция записывает аргументы, функции и переменные.

* Каждая функция при создании получает ссылку `[[Scope]]` на объект с переменными, в контексте которого была создана.
* При запуске функции создаётся новый объект с переменными `LexicalEnvironment`. Он получает ссылку на внешний объект переменных из `[[Scope]]`.
* При поиске переменных он осуществляется сначала в текущем объекте переменных, а потом – по этой ссылке.

{% embed url="https://learn.javascript.ru/closures\#leksicheskoe-okruzhenie" %}


