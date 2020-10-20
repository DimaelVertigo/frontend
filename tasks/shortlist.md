---
description: console training
---

# Shortlist

## References

```javascript
// references
var a = 5;
var b = { test: 10 }

function upd(a, b) {
	a = 10;
	b.test = 5;
}

upd(a, b);

console.log(a);
console.log(b.test);
```

{% hint style="info" %}
 Super-powers are granted randomly so please submit an issue if you're not happy with yours.
{% endhint %}

## Prototype

```javascript
var prototype = {
   a: 1000
}

var concrete = Object.create(prototype);

prototype.a = 2000;
console.log(concrete.a);

concrete.a = 3000;
console.log(prototype.a);
```

## Types

```javascript
var r1 = ['a', 'b'] == ['a', 'b'];
var r2 = ['a', 'b'] === ['a', 'b'];

console.log(r1);
console.log(r2);
```

## Promise

```javascript
var p = new Promise(function (resolve) {
	window.setTimeout(function () {
		resolve(1);
	}, 3000);
});

p.then(function (r3) {
	console.log(r3);
});

window.setTimeout(function () {
	p.then(function (r4) {
		console.log(r4);
	});
}, 60000);

```

{% page-ref page="../javascript/asynchronous-execution/promises.md" %}



## This

```javascript
var obj = {
	test1: function () {
		console.log(this);
	},
	test2: function (callback) {
		callback();
	},
	test3: function (callback) {
		callback.call(this);
	},
	test4: function (callback) {
		callback(function () {
			console.log(this);
		});
	}
}

obj.test1();
obj.test2(function () {
	console.log(this);
});
obj.test3(function () {
	console.log(this);
});
obj.test4(function (callback) {
	callback();
});
```

## Closures

{% tabs %}
{% tab title="Closures" %}
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
{% endtab %}

{% tab title="Answer" %}
```

```
{% endtab %}
{% endtabs %}

## ES6

{% tabs %}
{% tab title="ES6" %}
```javascript
const o1 = {
  countries: {
    ukraine: "some country 1"
  }
}

const o2 = { ...o1 };

o2.countries = {
  poland: "some country 2"
}

console.log(o1.countries);
```
{% endtab %}

{% tab title="Answer" %}
```
// 
```

{% hint style="info" %}

{% endhint %}
{% endtab %}
{% endtabs %}



## DOM/events

{% tabs %}
{% tab title="DOM/events" %}
```javascript
function createElement(n) {
	var a = document.createElement('a');

	a.innerText = 'a_' + n;

	document.body.appendChild(a);
	document.addEventListener('testEvent', function () {
		console.log(a.innerText);
	});

	return a;
}

var a1 = createElement(1);
var a2 = createElement(2);

document.body.removeChild(a1);
document.dispatchEvent(new CustomEvent('testEvent', {}));
```
{% endtab %}

{% tab title="Answer" %}
```

```

{% hint style="info" %}

{% endhint %}
{% endtab %}
{% endtabs %}

## Contexts

{% tabs %}
{% tab title="Contexts" %}
```javascript
var arr = ['foo', 'bar'],
	handlers = [];

for (var i = 0; i < arr.length; i++) {
	handlers.push(function () {
		console.log(arr[i]);
	});
}

handlers.forEach((h) => h());
```
{% endtab %}

{% tab title="Answer" %}
```

```
{% endtab %}
{% endtabs %}

