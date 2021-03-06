# Тестовое задание на позицию .Net backend developer

Необходимо спроектировать и разработать приложение для анализа изменений содержимого сайтов.

## Сценарий использования

Конечный пользователь передаёт в приложение адрес веб-страницы, за которой следует наблюдать, например http://fictionalsite.com. На момент обращения её содержимое представляет собой следующую разметку:
```html
<html>
  <head></head>
</html>
```
Через 15 минут автор сайта вносит и публикует изменения, что обновляет разметку до:
```html
<html>
  <head>
    <title>Site</title>
  </head>
</html>
```
Спустя ещё час страница снова меняется:
```html
<html>
  <head>
    <title>Site</title>
  </head>
  <body>
    <p>Content</p>
  </body>
</html>
```
На этом публикации изменений заканчиваются, видимо, веб-мастер достиг желаемого результата.

Спустя произвольное время, например, через неделю, пользователь обращается к приложению с целью узнать, что происходило с ресурсом по адресу http://fictionalsite.com. В ответ получает такую структуру (исключительно для примера, необязательно формат должен быть таким или хотя бы похожим):
```json
[
  {
    "ChangeId": 1283,
    "Date": "2018-07-18T16:25:43.511Z"
  },
  {
    "ChangeId": 1298,
    "Date": "2018-07-18T16:40:11.926Z"
  },
  {
    "ChangeId": 1711,
    "Date": "2018-07-18T17:39:57.034Z"
  }
]
```
Теперь пользователь может запросить детали по конкретному изменению, например, по тому, которое имеет идентификатор 1298. В ответ придёт что-то похожее на:
```html
<html>
  <head>
    <title>Site</title>
  </head>
</html>
```

## Постановка задачи

Требуется написать сервер приложения. Он должен предоставлять публичный API, позволяющий выполнять как минимум следующие действия:
  - передача url страницы, за которой следует вести наблюдение;
  - получение списка изменений по заданному url;
  - получение деталей конкретного изменения.
  
При поступлении в систему нового url, за ним должно начинаться наблюдение в полностью автоматическом режиме. Изменением считается любое отличие html-разметки страницы (включая JS и любое другое содержимое, даже если просто какой-то timestamp меняется в коде страницы при каждом новом её открытии) от сохранённой ранее. Не требуется проводить анализа изменений, строить diff'ы и так далее. Если простое сравнение срок показывает, что они различаются, значит, изменение произошло и его следует учитывать. Пример строк с отличиями:
```html
<html></html>
<html> </html>
 <html></html>
<htMl></html>
```
При выполнении задания следует учитывать изменения только в рамках указанной страницы, не нужно анализировать граф всего сайта.

При передаче деталей изменения не требуется проводить дополнительной обработки сохранённых данных. Клиенту отдаётся полное содержимое страницы на момент обнаружения/сохранения изменения. Как эти данные будут использованы на клиентской стороне остаётся за рамками этой задачи.

Сервер не должен терять данные при перезапуске и в случае аварийного завершения работы, не должен падать при получении некорректных запросов.

## Требования к решению

Работа должна быть выполнена на языке C#. Допускаются зависимости от сторонних nuget-модулей. Для хранения данных можно использовать как собственное решение, так и любую существующую базу данных. Прочие технические решения (такие как, например, выбор версии фреймворка, модели хостинга, транспортного протокола и формата сообщений, message-брокера) остаются на усмотрение разработчика.
