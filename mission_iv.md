# Задание IV

## Реализуем переход по папкам в левом меню

Текущая папка, та которая выбрана. Для текущей папки отображаются задачи в правой области.
Состояние текущей папки будем хранить в state.

Это состояние можно представить в виде свойства, которое хранит имя этой папки.

```js
const initialState = {
  currentFolder: null // Потому при выборе папки ТуДу значение будет 'ТуДу'
};

function uiReducer(state = initialState, action = {}) {
  // Тут нужно будет добавить SET_CURRENT_FOLDER
}
```

У нас уже есть один редьюсер secondReducer, стейт которого мы используем для хранения папок и задач. (secondReducer, которому вы надеюсь дали более понятное имя?)
Необходимо добавить uiReducer, и не забыть добабвить в combineReducers({ ui: uiReducer, ... })

Далее добавить экшн SET_CURRENT_FOLDER, payload будет содержать свойства folderName.
Редьюсер получая этот экнш должен устанавливать значение currentFolder из folderName.

Потом добавить экшн креатор initCurrentFolder используя thunk.
Внутри initCurrentFolder вызывать const state = getState(); из state брать первое название папки элементи из свойства folders.
Это название использовать в экшене сделав dispatch({type: 'SET_CURRENT_FOLDER', payload: { folderName: ИМЯ_ПАПКИ }})

Так теперь нам нужено после loadTodoData сделать dispatch(initCurrentFolder()).
loadTodoData асинхронный, дожидаться его выполнения будем используя промисы.
В loadTodoData нужно поставить return перед fetch, чтобы loadTodoData вернул промис.

```js
// TodoApp.jsx

dispatch(loadTodoData())
  .then( () => { dispatch(initCurrentFolder())  })
```

Далее в компонент FolderList нужно прокинуть currentFolder и выделить цветом выбранную папку. Компоненту Folder понадобятся два пропса: selected и folderName;

При клике на компонент Folder нужно делать dispatch({type: 'SET_CURRENT_FOLDER', payload: { folderName: ИМЯ_ПАПКИ }})
Для SET_CURRENT_FOLDER лучше сделать обычный экшн креатор setCurrentFolder(currentFolder)

В TaskList.jsx в map выводить только те задачи у которых folder соответствует currentFolder.
