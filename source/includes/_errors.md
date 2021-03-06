﻿# Errors

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