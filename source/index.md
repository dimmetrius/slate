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
для авторизации используется POST запрос:
`POST http://dimmetrius.ru/RUZService.svc/faculties`

в теле запроса по протоколу https в формате JSON передаются учетные данные

> Отправляются данные по протоколу https в формате:

```json
{
    "Login":"администратор",
    "Password":"****",
    "OSType":"0",
    "DeviceID":"ID"
}
```

для определения типа устройства `OSType` используются следующие коды:

Значение |  Описание
--------- | -----------
null | Не задан для HTML5 версии
0 | Неизвестное устройство
1 | Android
2 | iOs
3 | Windows Phone

Сервер возвращает специальный токен-ключ, который используется для доступа к методам подбора аудитории и регистрации заявки

Например:
`"VtwgVyvp8XhdoAAjQXrWtC+vbWp4CBfui0IBMCj2TBXKkUyx9LhB+LkbeNWyLAs7oPHvGc+YzI961z7cr4qO4nQQ7eCApRP6kMWAw1D9+tf9kunBLlgwNqcaJsgMOQ4C+z1ZW79fzrdoxZvZrM5hxcCqBx1Bb93qsTsRMSSR/EG1kkcKWawlr0F+McHb/WEO347YmcOD0UHXUFyZSi7fqVMm04A/ety72IbMjzf/D1/9Dz4h8whCLHBqJl8MJKrVH8HWYBFDMpVnUBSnmwPOp/kg66y6ysaJhUa7SNlUhWAQKtv1K59z8cshzoQc1Eam/o3ArAkhldgqKTuIRkmKDg=="`

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


#Состав группы

> Для получения состава групы используется следующий запрос:


```http
GET /RUZService.svc/staffofgroup?groupOid=323 HTTP/1.1
User-Agent: MobileClient/1.0.0
Host: dimmetrius.ru

```

### Параметры Запроса

Параметр | Тип | Описание
--------- | ------- | -----------
groupOid | number | ID группы

> Команда вернет JSON, структура которого будет следующей:

```json
[
    {
        "fio": "Бабич Кирилл Сергеевич",
        "shortFIO": "Бабич К.С.",
        "studentOid": 19790
    },
    {
        "fio": "Барков Никита Антонович",
        "shortFIO": "Барков Н.А.",
        "studentOid": 19783
    }
]
```
<aside class="success">
Запрос кэшируется на сервере
</aside>

#Кафедры

> Для получения кафедр используется следующий запрос:


```http
GET /RUZService.svc/chairs HTTP/1.1
User-Agent: MobileClient/1.0.0
Host: dimmetrius.ru

```

> Команда вернет JSON, структура которого будет следующей:

```json
[
    {
        "abbr": null,
        "chairOid": 1,
        "code": null,
        "faculty": "",
        "facultyOid": 0,
        "name": "!Не определена"
    },
    {
        "abbr": "Базкаф\"МаметсисанаИD",
        "chairOid": 655,
        "code": "02.26.10",
        "faculty": "Факультет компьютерных наук",
        "facultyOid": 5572,
        "name": "Базовая кафедра \"Математические методы системного анализа\" ИDA6C"
    }
]
```

# Преподаватели

## Получить всех преподавателей

> Для получения списка преподавателей используется следующий запрос:


```http
GET /RUZService.svc/lecturers HTTP/1.1
User-Agent: MobileClient/1.0.0
Host: dimmetrius.ru

```

> Команда вернет JSON, структура которого будет следующей:

```json
[
    { 
        "chair": "Департамент анализа данных и искусственного интеллекта",
        "chairOid": 663,
        "fio": "Алексеевский Даниил Андреевич",
        "lecturerOid": 6601,
        "shortFIO": "Алексеевский Д.А."
    },
    {
        "chair": "Департамент анализа данных и искусственного интеллекта",
        "chairOid": 663,
        "fio": "Большакова Елена Игоревна",
        "lecturerOid": 6690,
        "shortFIO": "Большакова Е.И."
    }
]
```

<aside class="success">
Запрос кэшируется на сервере
</aside>

## Получить преподавателей определенной кафедры

```http
GET /RUZService.svc/lecturers?chairOid=663 HTTP/1.1
User-Agent: MobileClient/1.0.0
Host: dimmetrius.ru

```

### Параметры Запроса

Параметр | Тип | Описание
--------- | ------- | -----------
chairOid | number | ID кафедры

# Расписание
# Заявки

<aside class="warning">Запросы на подбор аудитории, просмотр и регистрацию заявки могут выполнить только авторизованные пользователи</aside>

## Подбор аудитории
## Оформление заявки



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

