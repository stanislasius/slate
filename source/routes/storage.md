# 3. Работа со складом

## 1. Получение списка складов

> Пример запроса

```python
import requests

url = 'https://{account_name}.caffesta.com/a/v1.0/draft/storages'
headers = {"X-API-KEY": "CAFFESTA-X-API-KEY"}
resp = requests.get(url=url, headers=headers)

print(resp)
```

Получение списка складов происходит при обращении по пути `/a/v1.0/draft/storages` при использовании метода **GET**.

### HTTP запрос

`GET https://{account_name}.caffesta.com/a/v1.0/draft/storages`

> 200 ответ

```json
{
  "1": "(id:1) Склад кафе Буран",
  "2": "(id:2) Общий склад",
  "3": "(id:3) Резервный склад",
  "4": "(id:4) Склад магазина К звёздам",
  "5": "(id:5) Склад бара Берико"
}
```

Ответом будет JSON-объект:

| Название             | Тип                     | Описание                                                  |
|----------------------|-------------------------|-----------------------------------------------------------|
| "2"                  | Обязательный<br/>int    | Порядковый номер склада в запросе                         |
| "(id:2) Общий склад" | Обязательный<br/>string | Строка, содержащая ID товара `(ID)` и наименование склада |

## 2. Получение остатка товаров на складе

> Пример запроса

```python
import requests

url = 'https://{account_name}.caffesta.com/a/v1.0/draft/store/3'
headers = {"X-API-KEY": "CAFFESTA-X-API-KEY"}
resp = requests.get(url=url, headers=headers)

print(resp)
```

Получение остатков происходит при обращении по пути `/a/v1.0/draft/store/{caf_storage_id}` при использовании метода
**GET**, где:

* `{caf_storage_id}` - это ID склада точки продаж или заведения в Caffesta.

### HTTP запрос

`GET https://{account_name}.caffesta.com/a/v1.0/draft/store/{caf_storage_id}`

> 200 ответ

```json
[
  {
    "id": 1,
    "name": "Клюквенный морс",
    "measure": "kg",
    "cost": "0.000000",
    "summ": "0",
    "balance": "-3.000"
  },
  {
    "id": 2,
    "name": "Вафельный батончик",
    "measure": "pcs",
    "cost": "0.245614",
    "summ": "12.28",
    "balance": "50"
  }
]

```

Ответом будет JSON-объект:

| Название | Тип                     | Описание                       |
|----------|-------------------------|--------------------------------|
| id       | Обязательный<br/>int    | ID товара в Caffesta           |
| name     | Обязательный<br/>string | Название товара                |
| measure  | Обязательный<br/>string | Единица измерения товара       |
| cost     | Обязательный<br/>string | Себестоимость товара           |
| summ     | Обязательный<br/>string | Сумма остатка товара на складе |
| balance  | Обязательный<br/>string | Количество товара на складе    |
