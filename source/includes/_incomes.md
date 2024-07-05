# 3. Работа со складскими документами

## 1. Создание входящей накладной

> Пример простейшего кода

```python
import requests

api_key = "CAFFESTA-X-API-KEY"

url = 'https://{account_name}.caffesta.com/a/v1.0/storage/invoices'
headers = {"X-API_KEY": api_key}

with open('invoice.json', encoding='utf-8') as json_file:
    json_data = json_file.read()

resp = requests.post(url=url, data=json_data.encode('utf-8'), headers=headers)

print(resp)
```

```shell
curl "https://{account_name}.caffesta.com/a/v1.0/storage/invoices" \
  -H "X-API-KEY: {CAFFESTA-X-API-KEY}" \
  -X POST \
  -d invoice.json \
```

### HTTP запрос

`GET https://{account_name}.caffesta.com/a/v1.0/storage/invoices`

Создание документа накладной происходит при обращении по пути `/a/v1.0/storage/invoices` с использованием метода **POST** и
передачей JSON-объекта в теле запроса следующей структуры:

> JSON-объект для передачи

```json
{
  "number": "0155484",
  "date": "2018-06-13 14:28",
  "sum": 15520,
  "comment": "комментарий",
  "shipper": {
    "ref_code": "0348741",
    "name": "поставщик"
  },
  "storage": {
    "caf_id": 1
  },
  "items": [
    {
      "amount": 25,
      "package_code": "pcs",
      "product_code": "prod1",
      "price": 2,
      "caf_tax_id": 1,
      "price_with_tax": 2,
      "cost": 50,
      "cost_with_tax": 50
    }
  ],
  "categories": [
    {
      "ref_code": "cat1",
      "name": "category1",
      "parent": null
    },
    {
      "ref_code": "cat2",
      "name": "category2",
      "parent": "cat1"
    }
  ],
  "workspaces": [
    {
      "ref_code": "ws111",
      "name": "Бар"
    }
  ],
  "packages": [
    {
      "ref_code": "pcs"
    },
    {
      "ref_code": "l"
    },
    {
      "ref_code": "kg"
    },
    {
      "ref_code": "pack222",
      "name": "коробка 10 шт",
      "measure": "pcs",
      "value": 10
    }
  ],
  "products": [
    {
      "ref_code": "prod1",
      "type": "product",
      "cancellation_type": 1,
      "for_sale": true,
      "name": "1000",
      "measure": "pcs",
      "barcode": "2003079000000",
      "country": "RB",
      "weight_in_gram": 1000,
      "caf_tax_id": 1,
      "category_ref_code": "cat2",
      "use_discounts": true,
      "workspace_ref_code": "ws111",
      "prices": [
        {
          "caf_shop_id": 2,
          "caf_storage_id": 1,
          "price": 0.475
        }
      ]
    }
  ]
}
```

| Название           |   Тип    | Описание                                                                         |
|--------------------|:--------:|----------------------------------------------------------------------------------|
| number             |  string  | Номер или имя накладной                                                          |
| date               |  string  | Дата проведения накладной **(UTC)**                                              |
| sum                |   int    | Итоговая сумма по накладной                                                      |
| comment            |  string  | Комментарий к накладной                                                          |
| shipper            |  object  | Данные о поставщике                                                              |
| ref_code           |  string  | Внешний код поставщика                                                           |
| name               |  string  | Название поставщика                                                              |
| storage            |  object  | Данные по складу                                                                 |
| caf_id             |   int    | ID склада в Caffesta                                                             |
| items              |  object  | Информация для накладной о передаваемых товарах                                  |
| amount             |   int    | Количество товара по накладной                                                   |
| package_code       |  string  | Единица измерения фасовки `(может быть либо pcs(шт.), либо kg(кг.), либо l(л.))` |
| product_code       |  string  | Внешний код товара                                                               |
| price              |   int    | цена товара за единицу измерения                                                 |
| caf_tax_id         |   int    | ID налога в Caffesta                                                             |
| price_with_tax     |   int    | Цена товара с НДС                                                                |
| cost               |   int    | Стоимость без НДС                                                                |
| cost_with_tax      |   int    | Стоимость с НДС                                                                  |
| categories         |  object  | Данные по категориям товаров, передаваемых с этой накладной                      |
| ref_code           |  string  | Внешний код категории                                                            |
| name               |  string  | Название категории                                                               |
| parent             |  string  | Родительская категория для этой категории (может быть **null**)                  |
| workspaces         |  object  | Данные по цехам товаров, передаваемых с этой накладной                           |
| ref_code           |  string  | Внешний код цеха                                                                 |
| name               |  string  | Название цеха                                                                    |
| packages           |  object  | Данные по фасовкам, используемых в этой накладной                                |
| ref_code           |  string  | Внешний код фасовки                                                              |
| name               |  string  | Название фасовки                                                                 |
| measure            |  string  | Единица измерения фасовки (`pcs - шт, kg - кг, l - л`)                           |
| products           |  object  | Данные по товарам, передаваемым в этой накладной                                 |
| ref_code           |  string  | Внешний код товара                                                               |
| type               |  string  | Тип товара (`product - товар, tech_map - техкарта, sub_product - полуфабрикат`)  |
| cancellation_type  |   int    | Метод списания (`0 - Не списывать, 1 - По товару, 2 - По техкарте (рецептуре)`)  |
| for_sale           |   bool   | Доступен ли для продажи (`True - да, False - нет`)                               |
| name               |  string  | Название товара                                                                  |
| measure            |  string  | Единица измерения товара (`pcs - шт, kg - кг, l - л`)                            |
| barcode            | Неstring | Штрихкод товара                                                                  |
| country            | Неstring | Страна-производитель                                                             |
| weight_in_gram     |   int    | Вес за штуку в граммах/вес за литр в граммах                                     |
| caf_tax_id         |   int    | ID налога в Caffesta                                                             |
| category_ref_code  |  string  | Внешний код категории, в которой товар будет находится                           |
| use_discounts      |   bool   | Использует ли скидки (`True - да, False - нет`)                                  |
| workspace_ref_code |  string  | Внешний код цеха, к которому будет относиться товар                              |
| prices             |  object  | Данные о ценах на товар                                                          |
| caf_shop_id        |   int    | ID точки продаж в Caffesta, для которой нужно указать цены                       |
| caf_storage_id     |   int    | ID склада, откуда должно происходить списание при продаже товара                 |
| price              |   int    | Цена на указанной точке продаж                                                   |

В ответ будет возвращён JSON-объект следующего содержания:

> Ответ на запрос (успешный)

```json
{
  "success": true,
  "data": {
    "id": 7
  }
}
```

> Ответ на запрос (неудачный)

```json
{
  "success": false,
  "data": {
    "error": "текст ошибки"
  }
}
```

| Название | Тип    | Описание                                                   |
|----------|--------|------------------------------------------------------------|
| success  | bool   | Отображает успешность (`true` либо `false`)                |
| data     | object | Объект, содержащий дополнительную информацию               |
| id       | int    | При успешности запроса возвращается ID созданной накладной |
| error    | str    | Возвращаемая ошибка, если обработать запрос не удалось     |

## 2. Проводка или распроводка входящей накладной

> Пример простейшего кода

```python
import requests

api_key = "CAFFESTA-X-API-KEY"
invoice_action = "wire"
pos_id = 3

url = f'https://{account_name}.caffesta.com/a/v1.0/storage/invoices/{pos_id}'
headers = {"X-API_KEY": api_key}

resp = requests.put(url=url, headers=headers, json={"action":invoice_action})

print(resp)
```

```shell
curl "https://{account_name}.caffesta.com/a/v1.0/storage/invoices/3" \
  -X PUT \
  -d "{\"action\":\"wire\"}" \
  -H "X-API-KEY: CAFFESTA-X-API-KEY" \
  -H "content-type: application/json" 
```

### HTTP запрос

`PUT https://{account_name}.caffesta.com/a/v1.0/storage/invoices/3`

Проведение документа происходит при обращении по пути `/a/v1.0/storage/invoices/{id}` с использованием метода **PUT**,
а также с указанием в пути вместо `{id}` ID входящей накладной для проводки или распроводки и передачей JSON-объекта в
теле запроса:

| Название | Тип | Описание                              |
|----------|-----|---------------------------------------|
| action   | str | wire - провести, unwire - распровести |


В ответ будет возвращён JSON-объект следующего содержания:

> Ответ на запрос (успешный)

```json
{
  "success": true
}
```

> Ответ на запрос (неудачный)

```json
{
  "success": false,
  "data": {
    "error": "текст ошибки"
  }
}
```

| Название | Тип    | Описание                                               |
|----------|--------|--------------------------------------------------------|
| success  | bool   | Отображает успешность (`true` либо `false`)            |
| data     | object | Объект, содержащий дополнительную информацию           |
| error    | str    | Возвращаемая ошибка, если обработать запрос не удалось |

## 3. Получение списка накладных

> Пример простейшего кода

```python
import requests

url = 'https://{account_name}.caffesta.com/a/v1.0/storage/invoices?start=2022-02-22'
headers = {"X-API-KEY": "CAFFESTA-X-API-KEY"}

resp = requests.get(url=url, headers=headers)

print(resp)
```

```shell
curl "https://{account_name}.caffesta.com/a/v1.0/storage/invoices?start=2022-02-22" \
  -H "X-API-KEY: CAFFESTA-X-API-KEY" 
```

### HTTP запрос

`PUT https://{account_name}.caffesta.com/a/v1.0/storage/invoices?start=2022-02-22`


Получение списка накладных происходит при обращении по пути `/a/v1.0/storage/invoices?start={date}` с использованием
метода **GET**, где:

* `{date}`- дата, с которой нужно получить список накладных, в формате **гггг-мм-дд**.

В ответ будет возвращён JSON-объект следующего содержания:

```json
{
  "success": true,
  "data": [
    {
      "id": 1,
      "name": "1",
      "total_amount": 10.3,
      "total_wo_nds": 24.86,
      "total_nds_sum": 0,
      "type": "incoming",
      "shipper": {
        "id": 4,
        "name": "1111",
        "created_at": "2023-04-26T13:50:57+03:00",
        "deleted": false
      },
      "created_at": "2019-03-02T11:44:24+03:00",
      "created_by": {
        "id": 1
      },
      "updated_at": "2019-03-03T12:42:05+03:00",
      "updated_by": {
        "id": 1
      },
      "storage": {
        "id": 1,
        "name": "aaa2-5c628ec27312d"
      },
      "status": "not_wired",
      "finished": false,
      "invoice_date": "2019-03-02T03:00:00+03:00",
      "paid": false
    }
  ]
}
```

| Название | Тип    | Описание                                               |
|----------|--------|--------------------------------------------------------|
| success  | bool   | Отображает успешность (`true` либо `false`)            |
| data     | object | Объект, содержащий дополнительную информацию           |
| error    | str    | Возвращаемая ошибка, если обработать запрос не удалось |
| error    | str    | Возвращаемая ошибка, если обработать запрос не удалось |
| error    | str    | Возвращаемая ошибка, если обработать запрос не удалось |
| error    | str    | Возвращаемая ошибка, если обработать запрос не удалось |
| error    | str    | Возвращаемая ошибка, если обработать запрос не удалось |
| error    | str    | Возвращаемая ошибка, если обработать запрос не удалось |
| error    | str    | Возвращаемая ошибка, если обработать запрос не удалось |
| error    | str    | Возвращаемая ошибка, если обработать запрос не удалось |
| error    | str    | Возвращаемая ошибка, если обработать запрос не удалось |
| error    | str    | Возвращаемая ошибка, если обработать запрос не удалось |
| error    | str    | Возвращаемая ошибка, если обработать запрос не удалось |
| error    | str    | Возвращаемая ошибка, если обработать запрос не удалось |
| error    | str    | Возвращаемая ошибка, если обработать запрос не удалось |
| error    | str    | Возвращаемая ошибка, если обработать запрос не удалось |
| error    | str    | Возвращаемая ошибка, если обработать запрос не удалось |
| error    | str    | Возвращаемая ошибка, если обработать запрос не удалось |
| error    | str    | Возвращаемая ошибка, если обработать запрос не удалось |
| error    | str    | Возвращаемая ошибка, если обработать запрос не удалось |

## 4. Получение информации по одной накладной

> Пример простейшего кода

```python
import requests

api_key = "CAFFESTA-X-API-KEY"
invoice_id = 123


url = f'https://{account_name}.caffesta.com/a/v1.0/storage/invoices/{invoice_id}'
headers = {"X-API_KEY": api_key}

resp = requests.get(url=url, headers=headers)

print(resp)
```

```shell
curl "https://{account_name}.caffesta.com/a/v1.0/storage/invoices/123" \
  -H "X-API-KEY: {CAFFESTA-X-API-KEY}" 
```

### HTTP запрос

`GET https://{account_name}.caffesta.com/a/v1.0/storage/invoices/3`

Получение данных по одной накладной происходит при обращении по пути `/a/v1.0/storage/invoices/{id}` с использованием
метода **GET**, где:

*  `{id}` - это ID входящей накладной, по которой нужно получить данные.

В ответ будет возвращён JSON-объект следующего содержания:

```json
{
  "success": true,
  "data": {
    "id": 132,
    "type": "incoming",
    "name": "20231204-1554",
    "total_amount": 1,
    "shipper": {
      "id": 4,
      "name": "1111"
    },
    "items": [
      {
        "count": 1,
        "price": 2,
        "cost": 2,
        "package": {
          "id": 1,
          "ref_code": "pcs",
          "name": "шт",
          "measure": "pcs",
          "value": 1
        },
        "product": {
          "id": 41111,
          "ref_code": "",
          "name": "Item 41111",
          "cancellation_type": "cancel_dish",
          "measure": "pcs",
          "category": {
            "id": 174,
            "ref_code": "123456"
          },
          "work_space": {
            "id": 17
          },
          "created_at": "2023-08-29T17:38:11+03:00",
          "updated_at": "2023-09-04T12:34:42+03:00",
          "use_discounts": false,
          "type": "simple",
          "code": "",
          "active": true,
          "country": "",
          "has_g_m_o": false,
          "free_price": false,
          "weight_in_gram": 900,
          "entity_type": "product"
        },
        "tax": {
          "id": 1
        },
        "tax_sum": 0,
        "cost_with_tax": 2
      }
    ],
    "storage": {
      "id": 1,
      "name": "Кафе \"Буран\""
    },
    "doc_date": "2023-12-02T17:54:00+03:00",
    "paid": false,
    "status": "wired",
    "finished": true,
    "total_wo_nds": 2,
    "total_nds_sum": 0,
    "total_cost": 2,
    "created_at": "2023-12-04T17:54:20+03:00",
    "created_by": {
      "id": 1
    },
    "updated_at": "2023-12-04T17:56:19+03:00",
    "updated_at": {
      "id": 1
    }
  }
}
```


| Название          | Тип    | Описание                                    |
|-------------------|--------|---------------------------------------------|
| success           | bool   | Отображает успешность (`true` либо `false`) |
| data              | object | Объект, содержащий информацию по накладной  |
| id                | str    | ID накладной в Caffesta                     |
| type              | str    | Не используется                             |
| name              | str    | Имя или номер накладной                     |
| total_amount      | int    | Итоговое количество товара по накладной     |
| shipper           | object | Данные по поставщику                        |
| id                | str    | ID поставщика в Caffesta                    |
| name              | str    | Имя поставщика                              |
| items             | object | Массив передаваемых товаров                 |
| count             | str    | Количество товара                           |
| price             | str    | Цена за 1 единицу измерения                 |
| cost              | str    | Общая стоимость                             |
| package           | object | Данные о фасовке товара                     |
| id                | int    | ID фасовки                                  |
| ref_code          | str    | Внешний код фасовки                         |
| name              | str    | Наименование фасовки                        |
| measure           | str    | единица измерения фасовки                   |
| value             | int    | количество товара в фасовке                 |
| product           | object |                                             |
| id                | int    | ID товара                                   |
| ref_code          | str    | внешний код товара                          |
| name              | str    | наименование товара                         |
| cancellation_type | str    | метод списания                              |
| measure           | str    | единица измерения                           |
| category          | str    |                                             |
| id                | int    | ID категории                                |
| ref_code          | str    | внешний код категории                       |
| work_space        | str    |                                             |
| id                | int    | ID цеха                                     |
| created_at        | str    | время создания товара                       |
| updated_at        | str    | время последнего обновления                 |
| use_discounts     | bool   | применяется ли скидка `true/false`          |
| type              | str    | вид реализации                              |
| code              | str    |                                             |
| active            | bool   | активен ли товар   `true/false`             |
| country           | str    | страна-производитель                        |
| has_g_m_o         | bool   | содержит ли ГМО   `true/false`              |
| free_price        | bool   | свободная ли цена  `true/false`             |
| weight_in_gram    | int    | вес за штуку в граммах                      |
| entity_type       | str    | тип товара                                  |
| tax               | str    |                                             |
| id                | int    | ID налога                                   |
| tax_sum           | int    | Сумма НДС                                   |
| cost_with_tax     | int    | Стоимость с НДС                             |
| storage           | str    |                                             |
| id                | int    | ID склада                                   |
| name              | str    | Наименование склада                         |
| doc_date          | str    | дата документа                              |
| paid              | bool   | Оплачена ли                                 |
| status            | str    | Статус проводки `wired/unwired`             |
| finished          | str    |                                             |
| total_wo_nds      | int    | сумма без НДС                               |
| total_nds_sum     | int    | сумма НДС                                   |
| total_cost        | int    | стоимость с НДС                             |
| created_at        | str    | время создания накладной                    |
| created_by        | str    |                                             |
| id                | int    | ID пользователя, создавшего накладную       |
| updated_at        | str    | время последнего обновления накладной       |
| updated_by        | str    |                                             |
| id                | int    | ID пользователя, обновившего накладную      |
