# Linked Lists

**Связный список** - это структура данных, в которой несколько значений хранятся линейно. Каждое значение содержит своё собственное значение узла, а также содержит данные вместе со ссылкой на следующий узел в списке. **Ссылка** - это указатель на другой объект узла или на `null`, если следующего узла нет. Если у каждого узла есть только один указатель на другой узел \(чаще всего называется `next`\), то этот список считается односвязный \(singly linked list\); тогда как если у каждого узла есть две ссылки \(обычно `previous` и `next`\), то он считается двусвязный \(doubly linked list\).

Основное преимущество связных списков состоит в том, что они могут содержать произвольное количество значений, используя необходимый объем памяти. Сохранение памяти было очень важно на старых компьютерах, где мало памяти. Для массивов требовалось резервировать необходимый обьем памяти под элементы и можно было легко исчерпать доступную память или наоборот, не использовать то, что было зарезервировано. Связные списки были созданы, чтобы обойти эту проблему.

[https://frontend-stuff.com/blog/linked-lists-with-javascript/\#%D1%83%D0%B7%D0%B5%D0%BB-linkedlistnode](https://frontend-stuff.com/blog/linked-lists-with-javascript/#%D1%83%D0%B7%D0%B5%D0%BB-linkedlistnode)
