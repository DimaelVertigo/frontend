# Event loop

JavaScript **однопоточный**: одновременно может выполняться только одна задача. Обычно в этом нет ничего страшного, но теперь представьте, что вы выполняете задачу, которая занимает 30 секунд ... Да ... Во время этой задачи мы ждем 30 секунд, прежде чем что-то еще может произойти \(по умолчанию JavaScript выполняется в основном потоке браузера, поэтому весь пользовательский интерфейс завис\) 😬 Сейчас 2019 год, никому не нужен медленный, не отвечающий на запросы веб-сайт.

К счастью, браузер предоставляет нам некоторые функции, которые сам движок JavaScript не предоставляет: **WEB-API**. Это включает **DOM API, setTimeout, HTTP-запросы** и так далее. Это может помочь нам создать асинхронное неблокирующее поведение 🚀

Когда мы вызываем функцию, она добавляется к тому, что называется стеком вызовов. Стек вызовов является частью движка JS и не зависит от браузера. Это стопка, то есть она идет первой и последней \(представьте себе стопку блинов\). Когда функция возвращает значение, оно извлекается из стека.

{% hint style="info" %}
**LIFO** \([англ.](https://ru.wikipedia.org/wiki/%D0%90%D0%BD%D0%B3%D0%BB%D0%B8%D0%B9%D1%81%D0%BA%D0%B8%D0%B9_%D1%8F%D0%B7%D1%8B%D0%BA) last in, first out, «последним пришёл — первым ушёл»\) — способ организации и манипулирования данными относительно времени и приоритетов. В структурированном линейном списке, организованном по принципу LIFO, элементы могут добавляться и выбираться только с одного конца, называемого «вершиной списка».[\[1\]](https://ru.wikipedia.org/wiki/LIFO#cite_note-1) Структура LIFO может быть проиллюстрирована на примере стопки тарелок: чтобы взять вторую сверху, нужно снять верхнюю, а чтобы снять последнюю, нужно снять все лежащие выше.
{% endhint %}

![](https://res.cloudinary.com/practicaldev/image/fetch/s--44yasyNX--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://devtolydiahallie.s3-us-west-1.amazonaws.com/gid1.6.gif)

Функция `respond` возвращает функцию `setTimeout`. `setTimeout` предоставляется нам Web-API: он позволяет нам откладывать задачи, не блокируя основной поток. Функция обратного вызова, которую мы передали функции `setTimeout`, стрелочная функция `() => { return` `'Hey'}`, добавляется в веб-API. Тем временем функция `setTimeout` и `respond` ответа извлекаются из стека, они обе вернули свои значения!

![](https://res.cloudinary.com/practicaldev/image/fetch/s--d_n4m4HH--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://devtolydiahallie.s3-us-west-1.amazonaws.com/gif2.1.gif)

В Web API таймер работает до тех пор, пока второй аргумент, который мы ему передали, составляет 1000 мс. Обратный вызов не сразу добавляется в стек вызовов, вместо этого он передается так называемой очереди.

![](https://res.cloudinary.com/practicaldev/image/fetch/s--MewGMdte--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://devtolydiahallie.s3-us-west-1.amazonaws.com/gif3.1.gif)

Это может сбивать с толку: это не означает, что функция обратного вызова добавляется в стек вызовов \(таким образом, возвращает значение\) через 1000 мс! Она просто добавляется в очередь через 1000 мс. Но это очередь и функция должна дождаться своей очереди!

Теперь это то, чего мы все ждали ... Пора event loop выполнить свою единственную задачу: **соединить очередь со стеком вызовов**! Если стек вызовов **пуст**, т.е.  все ранее вызванные функции вернули свои значения и были извлечены из стека, то _первый элемент_ в очереди добавляется в стек вызовов. В этом случае никакие другие функции не вызывались, а это означает, что стек вызовов был пуст к тому времени, когда функция обратного вызова была первым элементом в очереди.

![](https://res.cloudinary.com/practicaldev/image/fetch/s--b2BtLfdz--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://devtolydiahallie.s3-us-west-1.amazonaws.com/gif4.gif)

The callback is added to the call stack, gets invoked, and returns a value, and gets popped off the stack.

Сallback добавляется в стек вызовов, вызывается, возвращает значение и извлекается из стека.

![](https://res.cloudinary.com/practicaldev/image/fetch/s--NYOknEYi--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://devtolydiahallie.s3-us-west-1.amazonaws.com/gif5.gif)

Читать статью - это весело, но вы почувствуете себя комфортно с этим, только работая с ней снова и снова. Попытайтесь выяснить, что записывается в консоль, если мы запустим следующее:

```javascript
const foo = () => console.log("First");
const bar = () => setTimeout(() => console.log("Second"), 500);
const baz = () => console.log("Third");

bar();
foo();
baz();
```

Давайте быстро посмотрим, что происходит, когда мы запускаем этот код в браузере:

![](https://res.cloudinary.com/practicaldev/image/fetch/s--BLtCLQcd--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_66%2Cw_880/https://devtolydiahallie.s3-us-west-1.amazonaws.com/gif14.1.gif)

1. Мы вызываем `bar`. `bar` возвращает `setTimeout` .
2. Callback, который мы передали `setTimeout` добавляется в Web API, `setTimeout` и `bar` извлекаются из callstack.
3. Запускается таймер, тем временем `foo` вызывается и логирует `First`. `foo` возвращается \(undefined\),`baz` вызывается, и callback добавляется в очередь.
4. `baz` логирует `Third`. Event loop видит, что стек вызовов пуст после возврата`baz` , после чего обратный вызов добавляется в стек вызовов.
5. Сallback логирует `Second`.

[https://dev.to/lydiahallie/javascript-visualized-event-loop-3dif](https://dev.to/lydiahallie/javascript-visualized-event-loop-3dif)

