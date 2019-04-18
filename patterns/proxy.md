---
description: Прокси - объект контролирующий работу с другим объектом.
---

# Proxy

```javascript
var http = {
    makeRequest: function (id, callback) {
        // Запрос на сервер
        setTimeout(function () {
            callback("Данные от сервера " + new Date().getTime());
        }, 3000);
    }
}

// Прокси объект
var proxy = (function () {

    // кэш
    var cache = {};

    return {
        makeRequest: function (id, callback) {
            if (cache[id]) {
                callback(cache[id]);
            }
            else {
                http.makeRequest(id, function (data) {
                    cache[id] = data;
                    callback(data);
                });
            }
        }
    }
})();
```

