# Micro tasks

{% embed url="https://jakearchibald.com/2015/tasks-microtasks-queues-and-schedules/" %}

| task queu \(macrotask queue\) | microtask queu |
| :--- | :--- |
| [`Promises`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Promise)\`\` | [`setTimeout`](https://developer.mozilla.org/ru/docs/Web/API/WindowTimers/setTimeout)\`\` |
| [`MutationObserver`](https://developer.mozilla.org/ru/docs/Web/API/MutationObserver)\`\` | [`setInterval`](https://learn.javascript.ru/settimeout-setinterval#setinterval)\`\` |
| \`\`[`process.nextTick`](https://medium.com/devschacht/event-loop-timers-and-nexttick-18579cd122e0)\`\` | \`\`[`setImmediate`](https://learn.javascript.ru/setimmediate)\`\` |
| \`\`[`Object.observe`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Object/observe)\`\` | \`\`[`requestAnimationFrame`](https://developer.mozilla.org/ru/docs/DOM/window.requestAnimationFrame)\`\` |
|  | I / O |



