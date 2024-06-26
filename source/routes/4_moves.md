# 5. Работа с перемещениями

## 1. Создание документа перемещения

> Пример запроса

```python
#TODO
```

Создание документа переменещения происходит при обращении по пути `/a/v1.0/storage/moves` с использованием
метода **POST** и передачей в теле запроса JSON-объекта.

### HTTP запрос

`POST https://{account_name}.caffesta.com/a/v1.0/storage/invoices/3`


> Пример JSON-объекта

```json
{
  "comment": "",
  "date": "2022-06-01 15:15",
  "storage_from": {
    "caf_id": 1
  },
  "storage_to": {
    "caf_id": 7
  },
  "items": [
    {
      "amount": 25,
      "product_code": "1"
    },
    {
      "": 45,
      "product_code": "2"
    }
  ]
}
```

Структура передаваемого JSON-объекта:

| Название     | Тип    | Описание                                     |
|--------------|--------|----------------------------------------------|
| comment      | str    | //комментарий к перемещению                  |
| date         | str    | //дата документа                             |
| storage_from | object |                                              |
| caf_id       | int    | //ID склада, с которого нужно взять товар    |
| storage_to   | object |                                              |
| caf_id       | int    | //ID склада, на который нужно положить товар |
| items        | object | //список товаров в документе перемещения     |
| amount       | int    | //кол-во товара                              |
| product_code | str    | //внешний код товара                         |

> 200 ответ (успешно)

```json
{
  "success": true,
  "data": {
    "id": 7
  }
}
```

> 200 ответ (неудача)

```json
{
  "success": true,
  "data": {
    "error": "описание ошибки"
  }
}
```

В ответ будет возвращён JSON-объект с информацией о статусе создания документа следующей структуры:

| Название | Тип    | Описание                                                      |
|----------|--------|---------------------------------------------------------------|
| success  | bool   | Отображает успешность (`true/false`)                          |
| data     | object | Объект, содержащий дополнительную информацию                  |
| id       | int    | При успешности запроса возвращается ID созданного перемещения |
| error    | str    | Возвращаемая ошибка, если обработать запрос не удалось        |

## 2. Проводка или распроводка документа перемещения

> Пример запроса

```python
#TODO
```

> JSON-объект для передачи

```json
{
  "action": "wire"
}
```

Проведение документа происходит при обращении по пути `/a/v1.0/storage/moves/{id}` с использованием метода **PUT**,
а также с указанием в пути вместо `{id}` ID перемещения для проводки или распроводки и передачей JSON-объекта в
теле запроса:

| Название | Тип | Описание                                                          |
|----------|-----|-------------------------------------------------------------------|
| action   | str | Действие документа (провести (`wire`) или распровести (`unwire`)) |

### HTTP запрос

`PUT https://{account_name}.caffesta.com/a/v1.0/storage/invoices/3`


> 200 ответ (успешный)

```json
{
  "success": true
}
```

> 200 ответ (не успешный)

```json
{
  "success": false,
  "data": {
    "error": "текст ошибки"
  }
}
```

Структура возвращаемого JSON-объекта:

| Название | Тип    | Описание                                               |
|----------|--------|--------------------------------------------------------|
| success  | bool   | Отображает успешность (`true` либо `false`)            |
| data     | object | Объект, содержащий дополнительную информацию           |
| error    | str    | Возвращаемая ошибка, если обработать запрос не удалось |
