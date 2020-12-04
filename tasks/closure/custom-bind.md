# Custom bind\(\)

```javascript
function bind(context, fn) {
  return function(...args) {
    fn.apply(context, args);
  };
}

function logPerson() {
  console.log(`Person ${this.name}, ${this.age}, ${this.job}`);
}

const person1 = {
  name: "Dmitry",
  age: 34,
  job: "JS Engeneer"
};

const person2 = {
  name: "Iria",
  age: 32,
  job: "PM"
};

bind(person1, logPerson)();
bind(person2, logPerson)();
```

