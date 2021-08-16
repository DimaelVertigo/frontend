# Timeouts

## WindowTimers.setTimeout\(\)

Вызов функции или выполнение фрагмента кода после указанной задержки.

```javascript
var timeoutID = window.setTimeout(func, [, delay, param1, param2, ...]);
var timeoutID = window.setTimeout(code [, delay]);
```

где

* `timeoutID -` это _числовой_ ID, который может быть использован позже с [`window.clearTimeout()` \(en-US\)](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/clearTimeout).
* `func -` это [функция](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function), которую требуется вызвать после `delay` миллисекунд.
* `code` - в альтернативном варианте применения это строка, содержащая код, который вы хотите выполнить после `delay` миллисекунд \(использовать этот метод **не рекомендуется** по тем же причинам, что и [eval\(\)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/eval#don't_use_eval!)\)
* `delay`  Необязательный -  задержка в миллисекундах \(тысячных долях секунды\), после которой будет выполнен вызов функции. Реальная задержка может быть больше; см. [Notes](https://developer.mozilla.org/ru/docs/Web/API/WindowOrWorkerGlobalScope/setTimeout#notes) ниже.

## setInterval\(\)

Метод **`setInterval()`** предложен для [`Window`](https://developer.mozilla.org/ru/docs/Web/API/Window) и [`Worker`](https://developer.mozilla.org/ru/docs/Web/API/Worker) интерфейсов. Он циклически вызывает функцию или участок кода с фиксированной паузой между каждым вызовом. Уникальный идентификатор intervalID, возвращаемый методом, позволяет впоследствии удалить запущенный **`setInterval`** c помощью [`clearInterval()` \(en-US\)](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/clearInterval). Метод определён с помощью миксина [`WindowOrWorkerGlobalScope`](https://developer.mozilla.org/ru/docs/Web/API/WindowOrWorkerGlobalScope).

### [Синтаксис](https://developer.mozilla.org/ru/docs/Web/API/WindowOrWorkerGlobalScope/setInterval#%D1%81%D0%B8%D0%BD%D1%82%D0%B0%D0%BA%D1%81%D0%B8%D1%81) <a id="&#x441;&#x438;&#x43D;&#x442;&#x430;&#x43A;&#x441;&#x438;&#x441;"></a>

```text
var intervalID = scope.setInterval(func, delay[, param1, param2, ...]);
var intervalID = scope.setInterval(code, delay);
```

#### [Параметры](https://developer.mozilla.org/ru/docs/Web/API/WindowOrWorkerGlobalScope/setInterval#%D0%BF%D0%B0%D1%80%D0%B0%D0%BC%D0%B5%D1%82%D1%80%D1%8B) <a id="&#x43F;&#x430;&#x440;&#x430;&#x43C;&#x435;&#x442;&#x440;&#x44B;"></a>

`func`[`function`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Function) - функция, которая будет вызываться каждые `delay` миллисекунд. Ожидается, что функция не принимает параметры и ничего не возвращает.`code`Этот необязательный синтаксис позволяет вам включать строку вместо функции, которая компилируется и выполняется каждые `delay` миллисекунд. Однако такая форма не рекомендуется по тем же причинам, которые делают [`eval()`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/eval) угрозой безопасности.`delay`Время в миллисекундах \(одна тысячная секунды\), на которое таймер выполнит задержку между вызовом функции. Если задано значение меньше 10, то будет использовано число 10. На самом деле задержка может быть больше чем указано, дополнительное объяснение приведено здесь:  [Reasons for delays longer than specified](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/setTimeout#reasons_for_delays_longer_than_specified) в [WindowOrWorkerGlobalScope.setTimeout\(\)](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/setTimeout).`param1, ..., paramN` НеобязательныйДополнительные параметры, передаваемые в функцию _func_.

