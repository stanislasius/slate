# Работа с товарами

## Импорт товаров и категорий товаров

> Пример запроса

```python
# TODO
```

Импорт товаров в Caffesta происходит при обращении по пути `/a/v1.0/product/import` с использованием
метода **POST** и передачей в теле запроса JSON-объекта.

<aside class="notice">
Путь <code>/a/v1.0/product/import</code> можно использовать для переноса меню из Вашей программы в "Настраиваемое меню" 
в Caffesta. Имейте в виду, что в меню будет добавлено всё, что указано в JSON-объекте, 
а что <b>не было указано в переданном JSON-объекте - будет из меню удалено</b>.
</aside>

Также для импорта товаров в Caffesta можно использовать путь `/a/v1.0/product/import_part_menu` с использованием
метода **POST** и передачей в теле запроса JSON-объекта.

<aside class="notice">
Этот путь <code>/a/v1.0/product/import_part_menu</code> также можно использовать для переноса меню из Вашей программы в "Настраиваемое меню" 
в Caffesta. Этот путь можно использовать, когда нужно импортировать меню частично и не нужно удалять то, что не было передано в этом запросе/
</aside>

### HTTP запрос

`POST https://{account_name}.caffesta.com/a/v1.0/product/import`

`POST https://{account_name}.caffesta.com/a/v1.0/product/import_part_menu`


> Пример JSON-объекта

```json
{
  "update_balances": false,
  "categories": [
    {
      "id": "123",
      "name": "МЯСО ПТИЦЫ И ПФ ИЗ НЕЕ",
      "level": 1,
      "parent": null
    },
    {
      "id": "124",
      "name": "Прочее",
      "level": 2,
      "parent": "123"
    }
  ],
  "menus": [
    {
      "id": "1",
      "name": "Раздел в Меню1",
      "level": 1,
      "parent": null,
      "caf_id": 1
    },
    {
      "id": "2",
      "name": "Подраздел в меню1",
      "level": 2,
      "parent": 1,
      "caf_id": 1
    },
    {
      "id": "5",
      "name": "Раздел в меню 2",
      "level": 1,
      "parent": null,
      "caf_id": 2
    }
  ],
  "taxes": [],
  "shops": [],
  "items": [
    {
      "id": "41044225",
      "deleted": false,
      "name": "БЕДРО Ц.БР ОХЛ",
      "barcode": "",
      "country": "",
      "weight_in_gram": null,
      "for_sale": true,
      "measure": "pcs",
      "use_discounts": true,
      "prices": [
        {
          "shop_id": 2,
          "value": 0,
          "value_day": 0.474
        }
      ],
      "balances": [
        {
          "storage_id": 1,
          "balance": 10
        }
      ],
      "category_id": "123",
      "menus": [
        1,
        2,
        5
      ],
      "tax_id": null,
      "work_space_code": "1",
      "description": ""
    }
  ]
}

```

Структура передаваемого JSON-объекта:

|    Название     |  Тип   | Обязательный | Описание                                                          |
|:---------------:|:------:|:------------:|:------------------------------------------------------------------|
| update_balances |  bool  |      -       | Обновлять ли остатки (`true` либо `false`)                        |
|   categories    | object |      да      | Список категорий                                                  |
|       id        |  str   |      да      | Код категории в стороннем ПО                                      |
|      name       |  str   |      да      | Только для настраиваемого меню                                    |
|      level      |  int   |      да      | Уровень вложенности                                               |
|     parent      |  str   |      да      | ID родительского пункта из этого же запроса                       |
|      menus      | object |     нет      | Настраиваемое меню, можно не передавать                           |
|       id        |  str   |      да      | Код раздела меню в стороннем ПО                                   |
|      name       |  str   |      да      | Имя раздела меню                                                  |
|      level      |  int   |      да      | Уровень вложенности                                               |
|     parent      |  str   |      да      | ID родительского пункта из этого же запроса                       |
|     caf_id      |  int   |      да      | ID пункта меню в Каффесте                                         |
|      taxes      |   -    |      -       |                                                                   |
|      shops      |   -    |      -       |                                                                   |
|      items      | object |      да      |                                                                   |
|       id        |  str   |      да      | Код товара в стороннем ПО                                         |
|     deleted     |  bool  |      -       | Удалён ли товар (`true` либо `false`)                             |
|      name       |  str   |      да      | Наименование товара                                               |
|     barcode     |  str   |     нет      | Штрихкод товара                                                   |
|     country     |  str   |     нет      | Страна-производитель                                              |
| weight_in_gram  |  str   |     нет      | Вес в граммах                                                     |
|    for_sale     |  bool  |      да      | Доступность на точке продаж (`true` либо `false`)                 |
|     measure     |  str   |      да      | Единица измерения                                                 |
|  use_discounts  |  bool  |      да      | Распространяются ли скидки (`true` либо `false`)                  |
|     prices      | object |      да      | Цена на точке продаж                                              |
|     shop_id     |  int   |      да      | ID точки продаж                                                   |
|      value      |  int   |      да      | Себестоимость                                                     |
|    value_day    |  int   |      да      | Розничная цена товара                                             |
|    balances     | object |     нет      | Остатки по складу                                                 |
|   storage_id    |  int   |      да      | ID склада в Caffesta                                              |
|     balance     |  int   |      да      | Количество остатка                                                |
|   category_id   |  str   |      да      | ID категории в Каффесте                                           |
|      menus      | object |     нет      | В данном случае товар будет привязан к папкам с ID 1, 2 и 5 сразу |
|     tax_id      |  int   |      да      | ID налога в Caffesta                                              |
| work_space_code |  str   |      да      | Цех                                                               |
|   description   |  str   |     нет      | Описание товара                                                   |

[//]: # (> 200 ответ &#40;успешно&#41;)

[//]: # ()

[//]: # (```json)

[//]: # ({)

[//]: # (  "success": true,)

[//]: # (  "data": {)

[//]: # (    "id": 7)

[//]: # (  })

[//]: # (})

[//]: # (```)

[//]: # ()

[//]: # (> 200 ответ &#40;неудача&#41;)

[//]: # ()

[//]: # (```json)

[//]: # ({)

[//]: # (  "success": true,)

[//]: # (  "data": {)

[//]: # (    "error": "описание ошибки")

[//]: # (  })

[//]: # (})

[//]: # (```)

[//]: # ()

[//]: # (В ответ будет возвращён JSON-объект с информацией о статусе создания документа следующей структуры:)

[//]: # ()

[//]: # (| Название | Тип    | Описание                                                      |)

[//]: # (|----------|--------|---------------------------------------------------------------|)

[//]: # (| success  | bool   | Отображает успешность &#40;`true/false`&#41;                          |)

[//]: # (| data     | object | Объект, содержащий дополнительную информацию                  |)

[//]: # (| id       | int    | При успешности запроса возвращается ID созданного перемещения |)

[//]: # (| error    | str    | Возвращаемая ошибка, если обработать запрос не удалось        |)

## Cоздание товара

> Пример запроса

```python
# TODO
```

> JSON-объект для передачи

```json
{
  "ref_code": "12345",
  "cancellation_type": 0,
  "for_sale": true,
  "name": "Вино7",
  "measure": "pcs",
  "barcode": "",
  "country": "",
  "use_discounts": true,
  "type": "product",
  "weight_in_gram": null,
  "caf_tax_id": 1,
  "category_ref_code": "cat2",
  "workspace_ref_code": "ws111",
  "prices": [
    {
      "caf_shop_id": 2,
      "caf_storage_id": 1,
      "price": 0.6,
      "self_cost": 0.3
    }
  ]
}
```

### HTTP запрос

`POST https://{account_name}.caffesta.com/a/v1.0/product/`

Создание товара происходит при обращении по пути `/a/v1.0/product/` с использованием метода **POST**,
а также передачей JSON-объекта в теле запроса:

| Название           |  Тип   | Обязателен ли | Описание                                               |
|--------------------|:------:|:-------------:|--------------------------------------------------------|
| ref_code           |  str   |      да       | внешний код товара                                     |
| cancellation_type  |  str   |      да       | метод списания                                         |
| for_sale           |  bool  |      да       | доступен ли для продажи (`true` либо `false`)          |
| name               |  str   |      да       | наименование товара                                    |
| measure            |  str   |      да       | единица измерения                                      |
| barcode            |  str   |      нет      | штрихкод                                               |
| country            |  str   |      нет      | страна-производитель                                   |
| use_discounts      |  bool  |      да       | распространяется ли скидка (`true` либо `false`)       |
| type               |  str   |      да       | тип товара                                             |
| weight_in_gram     |  int   |      нет      | вес за штуку в граммах                                 |
| caf_tax_id         |  int   |      да       | ID налога в Каффесте                                   |
| category_ref_code  |  str   |      да       | внешний код категории                                  |
| workspace_ref_code |  str   |      да       | внешний код цеха                                       |
| prices             | object |      да       | список точек продаж с установленными ценами и складами |
| caf_shop_id        |  int   |      да       | ID точки продаж в Каффесте                             |
| caf_storage_id     |  int   |      да       | ID склада                                              |
| price              | float  |      да       | цена продажи товара                                    |
| self_cost          | float  |      да       | себестоимость товара                                   |

<aside class="notice">
Обратите внимание, что поле <b>self-cost**</b> в запросе будет обрабатываться только в том случае, если в 
административной панели будет отключён модуль "Склад".
</aside>

## Обновление товара по ID в Caffesta

> Пример запроса

```python
# TODO
```

> JSON-объект для передачи

```json
{
  "ref_code": "12345",
  "cancellation_type": 0,
  "for_sale": true,
  "name": "Вино7",
  "measure": "pcs",
  "barcode": "",
  "country": "",
  "use_discounts": true,
  "type": "product",
  "weight_in_gram": null,
  "caf_tax_id": 1,
  "category_ref_code": "cat2",
  "workspace_ref_code": "ws111",
  "prices": [
    {
      "caf_shop_id": 2,
      "caf_storage_id": 1,
      "price": 0.6,
      "self_cost": 0.3
    }
  ]
}
```

### HTTP запрос

`PUT https://{account_name}.caffesta.com/a/v1.0/product/{id}`

Создание товара происходит при обращении по пути `/a/v1.0/product/{id}` с использованием метода **PUT**,
а также с указанием в пути вместо `{id}` ID нужного товара и передачей JSON-объекта в
теле запроса:

| Название           |  Тип   | Обязателен ли | Описание                                               |
|--------------------|:------:|:-------------:|--------------------------------------------------------|
| ref_code           |  str   |      да       | внешний код товара                                     |
| cancellation_type  |  str   |      да       | метод списания                                         |
| for_sale           |  bool  |      да       | доступен ли для продажи (`true` либо `false`)          |
| name               |  str   |      да       | наименование товара                                    |
| measure            |  str   |      да       | единица измерения                                      |
| barcode            |  str   |      нет      | штрихкод                                               |
| country            |  str   |      нет      | страна-производитель                                   |
| use_discounts      |  bool  |      да       | распространяется ли скидка (`true` либо `false`)       |
| type               |  str   |      да       | тип товара                                             |
| weight_in_gram     |  int   |      нет      | вес за штуку в граммах                                 |
| caf_tax_id         |  int   |      да       | ID налога в Каффесте                                   |
| category_ref_code  |  str   |      да       | внешний код категории                                  |
| workspace_ref_code |  str   |      да       | внешний код цеха                                       |
| prices             | object |      да       | список точек продаж с установленными ценами и складами |
| caf_shop_id        |  int   |      да       | ID точки продаж в Каффесте                             |
| caf_storage_id     |  int   |      да       | ID склада                                              |
| price              | float  |      да       | цена продажи товара                                    |
| self_cost          | float  |      да       | себестоимость товара                                   |

<aside class="notice">
Обратите внимание, что поле <b>self-cost</b> в запросе будет обрабатываться только в том случае, если в 
административной панели будет отключён модуль "Склад".
</aside>

## Получение информации о товаре с помощью внешнего кода

> Пример запроса

```python
# TODO
```

> JSON-объект для передачи

```json
{
  "ref_code": "12345",
  "cancellation_type": 0,
  "for_sale": true,
  "name": "Вино7",
  "measure": "pcs",
  "barcode": "",
  "country": "",
  "use_discounts": true,
  "type": "product",
  "weight_in_gram": null,
  "caf_tax_id": 1,
  "category_ref_code": "cat2",
  "workspace_ref_code": "ws111",
  "prices": [
    {
      "caf_shop_id": 2,
      "caf_storage_id": 1,
      "price": 0.6,
      "self_cost": 0.3
    }
  ]
}
```

### HTTP запрос

`GET https://{account_name}.caffesta.com/a/v1.0/product/by_ref_code/{ref_code}`

Получение информации о товаре происходит при обращении по пути `/a/v1.0/product/by_ref_code/{ref_code}` с использованием
метода **GET**, а также с указанием в пути вместо `{ref_code}` внешнего кода нужного товара.

> 200 ответ (успешный)

```json
{
  "success": true,
  "data": {
    "id": 200,
    "ref_code": "23452432",
    "name": "Печенье домашнее",
    "cancellation_type": "cancel_dish",
    "measure": "kg",
    "category": {
      "id": 11
    },
    "tax": {
      "id": 1
    },
    "work_space": {
      "id": 1
    },
    "created_at": "2018-04-03T17:35:40+03:00",
    "updated_at": "2022-04-02T20:15:46+03:00",
    "use_discounts": true,
    "type": "weighting",
    "code": "",
    "active": true,
    "country": "",
    "has_g_m_o": true,
    "free_price": false,
    "entity_type": "product"
  }
}
```

Структура полученного JSON-объекта:

| Название          |  Тип   | Описание                                      |
|-------------------|:------:|-----------------------------------------------|
| success           |  bool  | -                                             |
| data              | object | -                                             |
| id                |  int   | ID товара в Caffesta                          |
| ref_code          |  str   | внешний код товара                            |
| name              |  str   | наименование товара                           |
| cancellation_type |  str   | метод списания                                |
| measure           |  str   | единица измерения                             |
| category          | object |                                               |
| id                |  int   | ID категории товара                           |
| tax               | object |                                               |
| id                |  int   | ID налога на товар                            |
| work_space        | object |                                               |
| id                |  int   | ID цеха товара                                |
| created_at        |  str   | дата создания товара                          |
| updated_at        |  str   | последнее обновление карточки товара          |
| use_discounts     |  bool  | участвует в скидочных программах              |
| type              |  str   | тип товара                                    |
| code              |  str   | штрихкод                                      |
| active            |  bool  | доступен ли для продажи (`true` либо `false`) |
| country           |  str   | страна-производитель                          |
| has_g_m_o         |  bool  | есть ли ГМО (`true` либо `false`)             |
| free_price        |  bool  | свободная цена на товар (`true` либо `false`) |
| entity_type       |  str   | вид реализации                                |

## Обновление информации о товаре с помощью внешнего кода

> Пример запроса

```python
# TODO
```

> JSON-объект для передачи

```json
{
  "ref_code": "12345",
  "cancellation_type": 0,
  "for_sale": true,
  "name": "Вино7",
  "measure": "pcs",
  "barcode": "",
  "country": "",
  "use_discounts": true,
  "type": "product",
  "weight_in_gram": null,
  "caf_tax_id": 1,
  "category_ref_code": "cat2",
  "workspace_ref_code": "ws111",
  "prices": [
    {
      "caf_shop_id": 2,
      "caf_storage_id": 1,
      "price": 0.6,
      "self_cost": 0.3
    }
  ]
}
```

### HTTP запрос

`PUT https://{account_name}.caffesta.com/a/v1.0/product/by_ref_code/{ref_code}`

Получение информации о товаре происходит при обращении по пути `/a/v1.0/product/by_ref_code/{ref_code}` с использованием
метода **PUT**, а также с указанием в пути вместо `{ref_code}` внешнего кода нужного товара и передачей JSON-объекта с
информацией по товару в теле запроса:

| Название           |  Тип  | Обязателен ли | Описание                                               |
|--------------------|:-----:|:-------------:|--------------------------------------------------------|
| ref_code           |  str  |      да       | Внешний код товара                                     |
| type               |  str  |      да       | Тип товара                                             |
| cancellation_type  |  str  |      да       | Метод списания                                         |
| for_sale           | bool  |      да       | Доступен ли для продажи (`true` либо `false`)          |
| name               |  str  |      да       | Наименование товара                                    |
| measure            |  str  |      да       | Единица измерения                                      |
| barcode            |  str  |      нет      | Штрихкод                                               |
| country            |  str  |      нет      | Страна-производитель                                   |
| use_discounts      | bool  |      да       | Участвует в скидочных программах (`true` либо `false`) |
| weight_in_gram     |  int  |      нет      | Вес за единицу                                         |
| caf_tax_id         |  int  |      да       | ID налога на товара                                    |
| category_ref_code  |  str  |      да       | Внешний код категории                                  |
| workspace_ref_code |  int  |      да       | Внешний код цеха                                       |
| prices             |  str  |      да       | Цены на точках продаж и склад списания                 |
| caf_shop_id        |  int  |      да       | ID точки продаж в Каффесте                             |
| caf_storage_id     |  int  |      да       | ID cклада в Каффесте                                   |
| price              | float |      да       | Цена на точке продаж                                   |
| self_cost          | float |      да       | Себестоимость товара                                   |

<aside class="notice">
Обратите внимание, что поле <b>self-cost</b> в запросе будет обрабатываться только в том случае, если в 
административной панели будет отключён модуль "Склад".
</aside>
