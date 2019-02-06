# Задание III

Разберемся с загрузкой данных в наше приложение, попробуем thunk.

1) Загрузить данные в store

Загружать в store будем тестовые данные, по запросу выдаются тут: https://api.myjson.com/bins/w4fr4

Для начала подготовим редьюсер todoReducer.js, предустановим его state:

```js
const initialState = {
  folders: []
  tasks: []
};
```

Также заменил switch на условие if для наглядности.

```js
function todoReducer(state = initialState, action = {}) {

    // Когда будет вызван dispatch({ type: 'FILL_STATE', payload: { folders: [...], tasks: [...] } })
    // Будет выполнено следующие условие
    // Содержимое state будет целиком перезаписано payload
    if (action.type === 'FILL_STATE') {
        state = action.payload;
        return state;
    }

    // Если никакой action.type не удовлетворил условие, то возвращается state без изменения
    // Экшены проходят через все редьюсеры, которые добавили в приложение, и не все экшены могут интересовать нашь редьюсер
    // В итоге редьюсер думает: если экшн с таким типом меня не инстересует, то я верну свое текущее состояние
    // Если не вернуть state, то функция todoReducer вернет по-умолчанию undefined и значением state будет undefined
    return state;

}
```

Инициируем загрузку данных в хуке componentDidMount компонента TodoApp.jsx
Нужно вызвать fetch('https://api.myjson.com/bins/w4fr4') ...
Пришедшие данные нужно записать в state, вызвав: dispatch({ type: 'FILL_STATE', payload: jsonDataFromServer });

Тут про fetch https://learn.javascript.ru/fetch#ispolzovanie

2) Отобразить данные

Сначала нужно прокинуть данные в компоненты c помощью mapStateToProps: FolderList с state.todoReducer.folders и TaskList с state.todoReducer.tasks.

Потом как обычно в render используя map вывести списки данных.

3) Переделать на thunk

Всякую логику с обработкой данных лучше вынести из компонента.

В нашем случае мы можем вынести fetch из componentDidMount. Нашему компоненту все равно как загружаются данные, нечего его захламлять.

Куда же можно вынести fetch? Есть соблазн засунуть туда, где эти данные потом сохраняем, то есть в todoReducer.

```js
if (action.type === 'FILL_STATE') {

  // Но это работать не будет!

  // Инициализируем переменную
  let newState = null;

  // Запускаем асинхронный код
  fetch('https://api.myjson.com/bins/w4fr4')
    .then(function(response) {
      return response.json();
     })
    .then(function(state) {
      newState = state;
    });

    // Выолнение продолжается и не ждет, когда там придет ответ от сервера

    // В итоге newState так и остался null

    state = newState;

    // И мы не вернули то что хотели
    return state;
}
```

Тут и нужно использовать thunk https://github.com/reduxjs/redux-thunk (там кстати есть ссылки, если углубляться в суть)

thunk перехватывает экшены такого вида:

```js
function loadTodoData() {

  // Эту анонимную функцию он выполняет, передав аргуметом dispatch
  return function(dispatch, getState) {

    fetch('https://api.myjson.com/bins/w4fr4')
      .then(function(response) {
        return response.json();
       })
      .then(function(state) {
        // Ну вот данные получили, теперь можно диспатчить наш экшн
        dispatch({ type: 'FILL_STATE', payload: state });
      });

  };

}

dispatch(loadTodoData()); // А так вызываем
```

Этот код нужно вынести в отдельный файл loadTodoData.js в папку actions.

Короче, надо чтобы в componentDidMount не было fetch, а был только this.props.init();

```js
export function mapDispatchToProps(dispatch) {

  return {
    init() {
        dispatch(loadTodoData()); // loadTodoData нужно импортировать из actions/loadTodoData
    }
  }

}
```

Кстати, вот код библиотеки thunk:

```js
function thunkMiddleware({ dispatch, getState }) {
  return next => action =>
    typeof action === 'function' ? // Оу, dispatch получил функцию?
      action(dispatch, getState) : // дайка я вызову ее передав ей dispatch и getState
      next(action); // Ох, это обычный скучный экшн { type: 'BORING_ACTION', ... }, гуляй дальше (next)...
}

module.exports = thunkMiddleware
```
