# 2. Работа со складом

## 1. Получение списка складов

> Пример простейшего запроса

```python
import requests

api_key = "CAFFESTA-X-API-KEY"

url = 'https://{account_name}.caffesta.com/a/v1.0/draft/storages'
headers = {"X-API_KEY": api_key}

resp = requests.get(url=url, headers=headers)

print(resp)
```

```shell
curl "https://{account_name}.caffesta.com/a/v1.0/draft/storages" \
  -H "X-API-KEY: api_key" 
```

Получение списка складов происходит при обращении по пути `/a/v1.0/draft/storages` при использовании метода **GET**.

### HTTP запрос

`GET https://{account_name}.caffesta.com/a/v1.0/draft/storages`

> Пример ответа на запрос

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

| Название             | Тип    | Описание                                                  |
|----------------------|--------|-----------------------------------------------------------|
| "2"                  | int    | Порядковый номер склада в запросе                         |
| "(id:2) Общий склад" | string | Строка, содержащая ID товара `(ID)` и наименование склада |

## 2. Получение остатка товаров на складе

> Пример простейшего запроса

```python
import requests

api_key = "CAFFESTA-X-API-KEY"

url = 'https://{account_name}.caffesta.com/a/v1.0/draft/store/{caf_storage_id}'
headers = {"X-API_KEY": api_key}

resp = requests.get(url=url, headers=headers)

print(resp)
```

```shell
curl "https://{account_name}.caffesta.com/a/v1.0/draft/store/{caf_storage_id}" \
  -H "X-API-KEY: {CAFFESTA-X-API-KEY}" 
```

Получение остатков происходит при обращении по пути `/a/v1.0/draft/store/{caf_storage_id}` при использовании метода
**GET**, где:

* `{caf_storage_id}` - это ID склада точки продаж или заведения в Caffesta.

### HTTP запрос

`GET https://{account_name}.caffesta.com/a/v1.0/draft/store/{caf_storage_id}`

> Пример ответа на запрос

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

| Название | Тип    | Описание                       |
|----------|--------|--------------------------------|
| id       | int    | ID товара в Caffesta           |
| name     | string | Название товара                |
| measure  | string | Единица измерения товара       |
| cost     | string | Себестоимость товара           |
| summ     | string | Сумма остатка товара на складе |
| balance  | string | Количество товара на складе    |
