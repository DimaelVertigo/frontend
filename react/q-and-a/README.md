# Basics

## Какую проблему решает react?

Самые «тяжелые» операции в web — работа с DOM. Реакт оптимизирует эту работу. Как? Virtual DOM + обновление страницы за минимум «телодвижений».

## Мгновенно ли срабатывает `setState`? Если нет, то как выполнить код, который 100% выполнится после того, как новый state будет установлен?

setState \([документация](https://reactjs.org/docs/react-component.html#setstate)\) — асинхронная функция. Чтобы выполнить, что-либо заведомо после обновления state, нужно использовать запись с callback’омJs

```javascript
this.setState({data: [1,2,3]}, () => {
  console.log('здесь, уже точно state будет обновлен')
})
```

Так же, чтобы обновить состояние, основываясь на предыдущее, подойдет следующая запись:Js

```javascript
this.setState((prevState, props) => {
  return {counter: prevState.counter + props.step};
});
```



