# Memory Leak

Substring of huge string retains huge string in memory

[https://bugs.chromium.org/p/v8/issues/detail?id=2869](https://bugs.chromium.org/p/v8/issues/detail?id=2869)

```javascript
var s = "a huge, huge, huge string...";
s = s.substring(0, 5);
```

Expected results: s takes five bytes of memory, plus some overhead.

Actual results: s takes a huge, huge, huge amount of memory.

Unfortunately, most String functions use substring\(\) or no-ops internally: concatenating with empty string, `trim(), slice(), match(), search(), replace() with no match, split(), substr(), substring(), toString(), trim(), valueOf().`

