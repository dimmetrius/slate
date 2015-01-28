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
POST /RUZService.svc/token HTTP/1.1
User-Agent: MobileClient/1.0.0
Host: dimmetrius.ru

{"Login":"администратор", "Password":"****", "OSType":"0", "DeviceID":"ID"}
```

> Отправляются данные по протоколу https в формате:

```json
{
    "Login":"администратор",
    "Password":"****",
    "OSType":"0",
    "DeviceID":"ID"
}
```

где `OSType` принимает значение:

Значение |  Описание
--------- | -----------
null | Не задан для HTML5 версии
0 | Неизвестное устройство
1 | Android
2 | iOs
3 | Windows Phone

Сервер возвращает специальный токен-ключ, который используется для доступа к методам подбора аудитории и регистрации заявки

<aside class="notice">
DeviceId и DeviceType передаются только для мобильных приложений.
</aside>

# Факультеты

## Все факультеты

> Для списка факультетов используется следующий запрос:


```http
GET /RUZService.svc/faculties HTTP/1.1
User-Agent: MobileClient/1.0.0
Host: dimmetrius.ru

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

### Пример

`GET http://dimmetrius.ru/RUZService.svc/faculties`

# Группы

## Получить все группы

> Для получения групп используется следующий запрос:


```http
GET /RUZService.svc/groups HTTP/1.1
User-Agent: MobileClient/1.0.0
Host: dimmetrius.ru

```

> Команда вернет JSON, структура которого будет следующей:

```json
[
    {
        "chairOid": 0,
        "course": 1,
        "faculty": "Б 09.03.04 Программная инженерия",
        "facultyOid": 5591,
        "formOfEducation": "очная форма обучения",
        "groupOid": 321,
        "number": "101ПИ",
        "speciality": ""
    },
    { 
        "chairOid": 0,
        "course": 1,
        "faculty": "Б 09.03.04 Программная инженерия",
        "facultyOid": 5591,
        "formOfEducation": "очная форма обучения",
        "groupOid": 334,
        "number": "102ПИ",
        "speciality": ""
    }
]
```

<aside class="success">
Запрос кэшируется на сервере
</aside>

## Получить группы определенного факультета

```http
GET /RUZService.svc/groups?facultyOid=5591 HTTP/1.1
User-Agent: MobileClient/1.0.0
Host: dimmetrius.ru

```

### Параметры Запроса

Параметр | Тип | Описание
--------- | ------- | -----------
facultyOid | number | ID факультета



This endpoint retrieves a specific kitten.

<aside class="warning">Запросы на подбор аудитории, просмотр и регистрацию заявки могут выполнить только авторизованные пользователи</aside>

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

