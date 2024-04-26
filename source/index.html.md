---
title: Caffesta API

language_tabs: # must be one of https://github.com/rouge-ruby/rouge/wiki/List-of-supported-languages-and-lexers
  - php
  - python

<!--toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>-->

includes:
  - errors

search: false

code_clipboard: true

meta:
  - name: Caffesta API
    content: Протокол интеграции с Caffesta посредством API
---

# Описание протокола взаимодействия с Caffesta

```text
Выше выберите язык, для которого Вам нужны примеры.
```

Данный протокол взаимодействия по API с Caffesta предназначен для получения или передачи данных по разным разделам
программы.

Весь обмен данными происходит с использованием **JSON-объектов**.

Кодировка передаваемого текста - **UTF-8**.

<aside class="warning">
Все объекты, у которых есть <b>ref_code</b> создаются на основе данных из интегрирующейся программы. 
</aside>

Caffesta будет сначала искать объект с переданным значением в `ref_code`, и если не найдёт совпадений, создаст новый
объект.

Примеры таких объектов:

* Внешний код категории - `category_ref_code`;
* Внешний код цеха/отдела - `workspace_ref_code`;
* Внешний код товара - `product_code`.

Также при реализации некоторых возможностей через API могут встречаться объекты (формата `caf_*_id`), которые должны уже
быть созданы. Например:

* `caf_storage_id` - ID склада точки продаж или
  заведения ([создаётся в административной части программы]("https://caffesta.com/ru/help/41"))
* `caf_shop_id` - ID точки продаж(нужно связаться с отделом продаж Caffesta).

# Аутентификация

> Пример запроса

```php
require 'kittn'

api = Caffesta::APIClient.authorize!('wow_caffesta_api')
```

```python
import requests

url = "https://{account_name}.caffesta.com/a/v1.0/storage/test"
headers = {"X-API_KEY": "CAFFESTA-X-API-KEY"}
resp = requests.get(url, headers=headers)

print(resp)
```

Аутентификация происходит путем передачи параметра *X-API-KEY* в заголовке запроса. Проверить прохождение аутентификации
можно, обратившись по тестовому пути `/a/v1.0/storage/test`.

#### HTTP запрос

`GET https://{account_name}.caffesta.com/a/v1.0/storage/test`

<aside class="notice">
Обязательно проверьте, заменили ли вы <b>CAFFESTA-X-API-KEY</b> на <u>нужный X-API-KEY</u>, а также и <b>{account_name}</b> 
в запросе на <u>нужное</u> имя аккаунта.
</aside>

**Структура ответа**

> 200 Ответ

```json
{
  "success": true,
  "data": {
    "id": "test"
  }
}
```

| Название | Тип                     | Описание                            |
|----------|-------------------------|-------------------------------------|
| status   | Обязательный<br/>bool   | Успешна ли операция                 |
| data     | Обязательный<br/>object | Объект, содержащий тестовые данные  |
| id       | Обязательный<br/>string | Строка с тестовыми данными - `test` |

# Работа со складом

## Получение списка складов

> Пример запроса

```python
import requests

url = 'https://{account_name}.caffesta.com/a/v1.0/draft/storages'
headers = {"X-API-KEY": "CAFFESTA-X-API-KEY"}
resp = requests.get(url=url, headers=headers)

print(resp)
```

Получение списка складов происходит при обращении по пути `/a/v1.0/draft/storages` при использовании метода **GET**.

#### HTTP запрос

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

## Получение остатка товаров на складе

> Пример запроса

```python
import requests

url = 'https://{account_name}.caffesta.com/a/v1.0/draft/store/3'
headers = {"X-API-KEY": "CAFFESTA-X-API-KEY"}
resp = requests.get(url=url, headers=headers)

print(resp)
```

Получение остатков происходит при обращении по пути `/a/v1.0/draft/store/{caf_storage_id}` при использовании метода *
*GET**, где:

* `{caf_storage_id}` - это ID склада точки продаж или заведения в Caffesta.

#### HTTP запрос

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

# Работа со складскими документами

## Создание входящей накладной

> Пример запроса

```python
import requests

url = 'https://{account_name}.caffesta.com/a/v1.0/draft/store/3'
headers = {"X-API-KEY": "CAFFESTA-X-API-KEY"}
resp = requests.get(url=url, headers=headers)

print(resp)
```

Получение остатков происходит при обращении по пути `/a/v1.0/storage/invoices` с использованием метода **POST** и
передачей JSON-объекта в теле запроса.

#### HTTP запрос

`GET https://{account_name}.caffesta.com/a/v1.0/storage/invoices`

> Тело(body) запроса

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

Структура передаваемого JSON-объекта:

| Название       | Тип                     | Описание                            |
|----------------|-------------------------|-------------------------------------|
| number         | Обязательный<br/>string | Номер или имя накладной             |
| date           | Обязательный<br/>string | Дата проведения накладной **(UTC)** |
| sum            | Обязательный<br/>int    | Итоговая сумма по накладной         |
| comment        | Обязательный<br/>string | Комментарий к накладной             |
| shipper        | Обязательный<br/>object | Данные о поставщике                 |
| ref_code       | Обязательный<br/>string | Внешний код поставщика              |
| name           | Обязательный<br/>string | Название поставщика                 |
| storage        | Обязательный<br/>object | Данные по складу                    |
| caf_id         | Обязательный<br/>string | ID склада в Caffesta                |
| items          | Обязательный<br/>string | Количество товара на складе         |
| amount         | Обязательный<br/>string | Количество товара на складе         |
| package_code   | Обязательный<br/>string | Количество товара на складе         |
| product_code   | Обязательный<br/>string | Количество товара на складе         |
| price          | Обязательный<br/>string | Количество товара на складе         |
| caf_tax_id     | Обязательный<br/>string | Количество товара на складе         |
| price_with_tax | Обязательный<br/>string | Количество товара на складе         |
| cost           | Обязательный<br/>string | Количество товара на складе         |
| cost_with_tax  | Обязательный<br/>string | Количество товара на складе         |
| categories     | Обязательный<br/>string | Количество товара на складе         |
| ref_code       | Обязательный<br/>string | Количество товара на складе         |
| name           | Обязательный<br/>string | Количество товара на складе         |
| parent         | Обязательный<br/>string | Количество товара на складе         |
| workspaces     | Обязательный<br/>string | Количество товара на складе         |
| ref_code       | Обязательный<br/>string | Количество товара на складе         |
| name           | Обязательный<br/>string | Количество товара на складе         |
| packages       | Обязательный<br/>string | Количество товара на складе         |
| ref_code       | Обязательный<br/>string | Количество товара на складе         |
| name           | Обязательный<br/>string | Количество товара на складе         |
| measure        | Обязательный<br/>string | Количество товара на складе         |
| value          | Обязательный<br/>string | Количество товара на складе         |


