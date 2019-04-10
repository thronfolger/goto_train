# Задание VI

## Добавляем бэкенд

Бэкен реализован на Node.js. Даннын храняться в файле, по-этому устанавливать БД не требуется

На данный момент в `loadTodoData` данные загружаются из https://api.myjson.com/bins/w4fr

Теперь есть заготовка для своего бэкенда.

https://github.com/thronfolger/backend

Нужно скачать код и закинуть в текущий репозиторий, в папку backend

Инструкция по запуску бэкенда тут:

https://github.com/thronfolger/backend/blob/master/README.md

В `loadTodoData` нужно будет прописать http://localhost:3003/todoapp

http://localhost:3003/todoapp возвращает содержимое нашей базы (файла) backend/src/database/storage/todoapp.json

В файле backend/app.js видно что возвращается JSON из двух свойств { success, data } и этот момент нужно учесть в коллбэки промиса, который фозвращает fetch

```js
app.get('/todoapp', (req, res) => {
    const result = { success: true, data: db.getData() }; // { success, data }
    res.json(result);
});

```

## Postman для тестирования

Для тестовых запросов на сервер удобно использовать Postman

https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop?hl=ru

Урок по Postman: https://www.guru99.com/postman-tutorial.html

На тему запросов
https://developer.mozilla.org/ru/docs/Web/HTTP/Methods/POST

Стоит базово освоить. На старых проектах часто использовал.

## Сохранение и изменение задач на бэке

Эта задача касательно бэкенда.

Сейчас бэкенд умеет только возвращать данные и создавать задачи.

Откройте `app.js`.

Для возврата всех данных: `/todoapp`
Для создания задачи: `/todoapp/task/create`

Протестируйте эти запросы через Postman.

`/todoapp` - это GET запрос
`/todoapp/task/create` - это POST запрос, который принимает JSON данные { title '', folder: ''} 

Пример на JS

```
fetch('/todoapp/task/create/', {
    method: 'post',
    headers: {
        'Accept': 'application/json',
        'Content-Type': 'application/json'
    },
    body: JSON.stringify({ title: 'Название', folder: 'Имя папки' })
}).then(() => {})
```

Вот этот текст браузер пошлет на сервер (там видно наши заголовк headers):

```
POST http://localhost:3003/todoapp/task/create/
Host: localhost:3003
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.11; rv:66.0) Gecko/20100101 Firefox/66.0
Accept: application/json
Accept-Language: ru-RU,ru;q=0.8,en-US;q=0.5,en;q=0.3
Accept-Encoding: gzip, deflate
Referer: http://localhost:3003/test
Content-Type: application/json
Origin: http://localhost:3003
Content-Length: 57
Connection: keep-alive
Pragma: no-cache
Cache-Control: no-cache

{"title":"Название","folder":"Имя папки"}
```

Если хочеться разобраться, то на данную тему советую почитать первые три главы из книги Котерова "PHP 7(5). В подлиннике"

И так, теперь нужно написать код, чтобы бэкенд умел изменять задачи.

В `app.js` для изменения уже существующей задачи уже есть роутинг `/todoapp/task/update`

Найдите этот этот роутинг, там уже есть заготовка. Нужно будет написать код, который непосредственно работает с БД. Он расположен в `src/TodoDatabase.js` функция `updateTask`

В `src/TodoDatabase.js` есть инструкция в коде.

Делайте ориентируясь на уже готовый код для создания задач `/todoapp/task/create`

Сделаете, теструйте в Postman.