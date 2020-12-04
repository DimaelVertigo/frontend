# Binary search algorithm

```javascript
const array = [1, 3, 5, 7, 11, 13, 17, 19];

function binarySearch(arr, search) {
  let left = 0,
    right = arr.length - 1;

  while (left < right) {
    const mid = Math.floor((left + right) / 2);

    if (arr[mid] >= search) {
      right = mid;
    } else {
      left = mid + 1;
    }
  }

  return arr[right] === search ? right : -1;
}

console.log(binarySearch(array, 13)); // 5
```

{% hint style="info" %}
_O\(log N\)_
{% endhint %}

