# Palindrome

### Решение в лоб

```javascript
const str = "чайка";

const isPalindrome = s => {
  return (
    s ===
    s
      .split("")
      .reverse()
      .join("")
  );
};

console.log(isPalindrome(str)); // false
```

Для строки `'чайкa'` из 5 символов:

* по 5 итераций внутри `split()`, `reverse()`, `join()`
* 1 итерация при сравнении строк
* 2 дополнительных массива и дополнительная строка
* **итого:** 16 итераций, 15 дополнительных ячеек памяти

Для строки `'топот'` из 5 символов:

* по 5 итераций внутри `split()`, `reverse()`, `join()`
* 5 итераций при сравнении строк
* 2 дополнительных массива и дополнительная строка
* **итого:** 20 итераций, 15 дополнительных ячеек памяти

{% hint style="info" %}
_O\(4n\)_ или _O\(n\)_
{% endhint %}

### Решение с использованием двух указателей

```javascript
const str = "чайка";

const isPalindrome = s => {
  let left = 0,
    right = s.length - 1;

  while (left < right) {
    if (s[left] !== s[right]) {
      return false;
    }
    left++;
    right--;
  }
  return true;
};

console.log(isPalindrome(str)); // false
```

Для строки `'чайка'` из 5 символов:

* 1 итерация и 2 числа

Для строки `'топот'` из 5 символов:

* 2 итерации и 2 числа

{% hint style="info" %}
_O\(1\)_
{% endhint %}

