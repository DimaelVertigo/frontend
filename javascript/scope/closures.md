---
description: >-
  Замыкание – это функция вместе со всеми внешними переменными, которые ей
  доступны.
---

# Closures

{% embed url="https://developer.mozilla.org/ru/docs/Web/JavaScript/Closures" %}



```javascript
var obj2 = {
	test: function () {
		var a = 'a';

		function inner() {
			var b = 'b';

			return a || b;
		}

		return inner;
	}
}

console.log(obj2.test()());
```

