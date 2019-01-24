# Задание II

~~Напишу, если кто-нибудь сделает первое задание!~~

И так. Что будет за приложение, для которого будут даваться задания. Туду лист. Простой туду лист делать не будем. Будем делать навороченный с тайм-трекингом и фичами из Wunderlist.

Начнем с малого. Постепенно приложение будет обрастать возможностями, усложняться. Такими возможностями, которые дадут попрактиковать не только React/Redux, но и другие библиотеки, например для роутинга и модальных окон.

А теперь задание.

1) Нужно немного поверстать. Особых требований к дизайну нет. Можно делать на свой вкус, а можно использовать тот Bootstrap. Но функционал важнее дизайна, так что мы сосредоточимся на реализации функционала.

Я решил не рисовать весь интерфейс сразу. Так что  мы будем верстать с в каждой новой задачей, которая вносит новую фичу в интерфейсе.

Что для начала верстать я нарисовал. Фотка в папке mission_ii.

На бумажке нарисовано: левое меню с папками, поле для добавления новой задачи, список задач, с чекбоксом и звездочкой для добавления  в закладки.

Cоветую верстать по БЭМ. Читать всю документацию по БЭМу не нужно. Достаточно следовать соглашению:
https://ru.bem.info/methodology/naming-convention/

Для стилей можно использовать SCSS, а новых версиях create-react-app он идет из коробки. Удобно использовать вложенность и &:
https://marksheet.io/sass-nesting.html

```jsx
<div className="Clock">
  <div className="Clock__unit Clock__hours">22</div>
  <div className="Clock__unit Clock__minutes">41</div>
  <div className="Clock__unit Clock__seconds">14</div>
</div>
```

```scss
.Clock {

  &__unit {
    font-weight: bold;
  }

  &__hours {
    color: red;
  }

  &__minutes {
    color: green;
  }

  &__seconds {}

}

```


2) После, нужно всю верстку разбить на компоненты React. И положить в папку components. Пока можно сделать в виде классов React.Component.

Пойдет примерно такая структура:

```
TodoApp.jsx
  LeftMenu.jsx
    FolderList.jsx
      Folder.jsx // будут рисоваться через map в render FolderList
      Folder.jsx
      Folder.jsx

    AddFolderButton.jsx

  TaskArea.jsx
    CreateTaskInput.jsx
    TaskList.jsx
        Task.jsx
        Task.jsx
        Task.jsx
```

Компоненты ниже будут играть роль обертки для:

TodoApp.jsx

LeftMenu.jsx

TaskArea.jsx


```jsx
render() {

  return (
      <div className="TodoApp">
        {this.props.children}
      </div>
  );

}
```

```jsx
<TodoApp>

  <LeftMenu>
    <FolderList />
  </LeftMenu>

  <TaskArea>
    <CreateTaskInput />
    <TaskList />
  </TaskArea>

</TodoApp>
```
