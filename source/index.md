---
title: RUZ Mobile REST Api

language_tabs:
  - http
  - shell
  - javascript
  - csharp

toc_footers:
  - Для Конкурса
  - <a href='http://github.com/tripit/slate'>Сделано на основе Slate</a>

search: true
---

# RUZ Mobile REST API

Добро пожаловать в RUZ Mobile REST API! 

Данный документ содержит описание WEB службы подсистемы публикации РУЗ на мобильных устройствах.

В web сервисе реализована авторизация и аутентификация пользователей для оформления заявок и получения push уведомлений;

Запросы данных расписаний кэшируются на сервере, что позволяет серверу выдерживать серьезные нагрузки

Реализован механизм подачи и рассмотрения заявок РУЗ.


# Авторизация

> Для авторизации используется следующий запрос:

```http
GET /api HTTP/1.1
User-Agent: MyClient/1.0.0
Accept: application/vnd.travis-ci.2+json
Authorization: token "YOUR TRAVIS ACCESS TOKEN"
Host: travis.example.com

```

> Отправляется по протоколу https в формате:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Isis",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

RUZ Mobile API позволяет получить специальный токен-ключ, который используется для доступа к методам подбора аудитории и регистрации заявки

Для авторизации используется заголовок:

`Authorization: ...`

<aside class="notice">
DeviceId и DeviceType передаются только для мобильных приложений.
</aside>

# Факультеты

## Все факультеты

> Для списка факультетов используется следующий запрос:

```http
GET http://dimmetrius.ru/RUZService.svc/faculties
```

> Команда вернет JSON, структура которого будет следующей:

```json
[ 
    {
        "abbr": "МТМТ 472",
        "code": "М010301МТМТ",
        "facultyOid": 7007,
        "institute": "Факультет математики",
        "name": "Б 01.03.01 Математика"
    },
    {
        "abbr": "МТМТ 452",
        "code": "Н010301МТМТ",
        "facultyOid": 7339,
        "institute": "",
        "name": "Б 01.03.01 НН Математика"
    }
]
```

This endpoint retrieves all kittens.

### Пример

`GET http://dimmetrius.ru/RUZService.svc/faculties`

### Параметры Запроса

Параметр | Тип | Описание
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Запрос кэшируется на сервере
</aside>

## Получить Определенный факультет

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/3"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Isis",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">If you're not using an administrator API key, note that some kittens will return 403 Forbidden if they are hidden for admins only.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the cat to retrieve

# Ошибки

Ошибки могут содержать следующие коды:

Код Ошибки | Значение
---------- | -------
400 | Bad Request -- Ваш запрос не корректен
401 | Unauthorized -- Ваш запрос неавторизован
403 | Forbidden -- Вам запрещен вызов функции
404 | Not Found -- Точка не найдена
405 | Method Not Allowed -- Метод запрещен на сервере
406 | Not Acceptable -- Формат Ваших данных неправильный
410 | Gone -- Вызов устаревшей версии API
429 | Too Many Requests -- Слишком частое обращение на сервер
500 | Internal Server Error -- Ошибка на сервере. Попробуйте еще раз позже
503 | Service Unavailable -- Работы на сервере. Попробуйте еще раз позже

<aside class="notice">Сервер вместе с кодом ошибки может вернуть описание ошибки для отображения в приложении:</aside>

> Если сервер вернул описание ошибки, то его формат будет следующим:

```json
  {
    "code": 1,
    "description": "Описание ошибки"
  }
```

