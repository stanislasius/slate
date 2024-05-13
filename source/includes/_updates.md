# 7. Работа с обновлениями данных

<aside class="warning">
Настоятельно рекомендуем все полученные в этом разделе данные хранить в базе Вашего сайта, программы или сервиса для последующего быстрого доступа.
</aside>

## 1. Получение времени (timestamp) обновлений

> Пример запроса

```python
# TODO
```

### HTTP запрос

`GET https://{account_name}.caffesta.com/a/v1.0/draft/get_updates/{pos_id}`

[//]: # (`GET https://{account_name}.caffesta.com/a/v1.0/draft/get_updates` ????)

Получение последнего времени (далее **timestamp** или **ts**) обновления данных происходит при обращении по пути
`/a/v1.0/draft/get_updates/{pos_id}` c использованием метода **GET** и передачей вместо `{pos_id}` ID точки продаж в
Caffesta.

[//]: # (Если указан pos_id в запросе, то будут возвращены ts последних изменений по указанной точке продаж)

В ответ на запрос будет возвращён JSON-объект следующего содержания (описана та часть, с которой можно
взаимодействовать):

```json
{
  "code": "ok",
  "data": {
    "categories": 0,
    "menus": 0,
    "desks": 0,
    "taxes": 0,
    "workspaces": 0,
    "comments": 0,
    "discounts": 0,
    "promotions": 1648303396,
    "payments": 0,
    "settings": 1648566884,
    "balances": 1648638899,
    "products": 1648657344,
    "device_models": 1648038317,
    "clients": 1648638898
  }
}
```

| Название      |  Тип   | Описание                                    |
|---------------|:------:|---------------------------------------------|
| code          |  bool  | Успешность обработки задания (`ok/error`)   |
| data          | object |                                             |
| categories    |  int   | Категориях товаров                          |
| menus         |  int   | Меню организации             ???            |
| desks         |  int   | Столы, залы и карта зала     ???            |
| taxes         |  int   | Налоги                                      |
| workspaces    |  int   | Цеха и отделы                               |
| comments      |  int   | Быстрые комментарии          ???            |
| discounts     |  int   | Скидки                       ???            |
| promotions    |  int   | Акции                        ???            |
| payments      |  int   | Типы оплат                   ???            |
| settings      |  int   | Общие настройки              ???            |
| balances      |  int   | Остатки товаров                             |
| products      |  int   | Ингредиенты, товары, полуфабрикаты и товары |
| device_models |  int   | Устройства                   ???            |
| clients       |  int   | Зарегистрированные клиенты                  |

Принцип работы с timestamp'ами:

1) Совершается запрос на путь `/a/v1.0/draft/get_updates/{pos_id}` c указанием ID точки продаж вместо `{pos_id}` для
   получения
   последнего timestamp обновлений;
2) Полученный timestamp сравнивается с полученным предыдущим по конкретному разделу;
3) Если полученный timestamp больше предыдущего, то нужно сделать запрос по нужному пути за обновлениями данных.

## 2.1. Получение информации по товарам

> Пример запроса

```python
# TODO
```

### HTTP запрос

`GET https://{account_name}.caffesta.com/a/v1.0/draft/get_products0/{posId}/{start_id}/{ts}`

Получение информации по товарам происходит при обращении по пути
`/a/v1.0/draft/get_products0/{posId}/{start_id}/{ts}` c использованием метода **GET** и передачей уточняющих переменных:

* posId - ID точки продаж в Caffesta, для которой нужно получить данные по товарам;
* start_id - ID товара. Если это первый запрос, то указывается 0; в противном случае указывается ID товара, который был
  последним из предыдущего запроса;
* ts - timestamp предыдущего запроса. Если указать 0, то будут выгружены товары за всё время.

<aside class="notice">
Удалённые товары возвращаются только тогда, когда ts <b>НЕ равен нулю</b>.
</aside>


В ответ же будет возращён JSON-объект следующего содержания:

```json
{
  "code": "ok",
  "data": [
    {
      "id": 1,
      "name": "Сырный раф",
      "entityType": "product",
      "cancellationType": "cancel_dish",
      "measure": "pcs",
      "deleted": false,
      "useDiscounts": true,
      "color": "gray",
      "shortDescription": null,
      "alternateName": "1235",
      "buttonColor": null,
      "image": "image.jpeg",
      "type": "simple",
      "coefficients": null,
      "code": "",
      "active": true,
      "freePrice": false,
      "descr": "product",
      "category_id": 2,
      "workSpace_id": 1,
      "tax_id": 1,
      "price": 0,
      "image_path": "https://imagehosting.net/images/folder/image.jpeg"
    }
  ]
}
```

| Название         |       Тип       | Описание                               |
|------------------|:---------------:|----------------------------------------|
| code             |     string      | ???                                    |
| data             |     object      |                                        |
| id               |       int       | ID товара в Caffesta                   |
| name             |     string      | наименование товара                    |
| entityType       |     string      | тип товара                             |
| cancellationType |     string      | метод списания                         |
| measure          |     string      | единица измерения                      |
| deleted          |      bool       | удалён ли товар (`true/false`)         |
| useDiscounts     |      bool       | применяется ли скидка (`true/false`)   |
| color            |     string      | ???                                    |
| shortDescription | string или null | краткое описание товара                |
| alternateName    | string или null | название, которое отображается в меню  |
| buttonColor      | string или null | цвет кнопки товара                     |
| image            |     string      | используемая картинка (относ.путь)     |
| type             |     string      | вид реализации                         |
| coefficients     | string или null | ???                                    |
| code             |     string      | внешний код товара                     |
| active           |      bool       | доступен ли для продажи (`true/false`) |
| freePrice        |      bool       | имеет ли свободную цену (`true/false`) |
| descr            |       int       | ???                                    |
| category_id      |       int       | ID категории                           |
| workSpace_id     |       int       | ID цеха                                |
| tax_id           |       int       | ID налога на товар                     |
| price            |      float      | ???                                    |
| image_path       |     string      | абсолютный путь к картинке товара      |

## 2.2 [deprecated]Получение информации по товарам

> Пример запроса

```python
# TODO
```

### HTTP запрос

`GET https://{account_name}.caffesta.com/a/v1.0/draft/get_products/{posId}/{start_id}/{ts}`

Получение информации по товарам происходит при обращении по пути
`/a/v1.0/draft/get_products/{posId}/{start_id}/{ts}` c использованием метода **GET** и передачей уточняющих переменных:

* posId - ID точки продаж в Caffesta, для которой нужно получить данные по товарам;
* start_id - ID товара. Если это первый запрос, то указывается 0; в противном случае указывается ID товара, который был
  последним из предыдущего запроса;
* ts - timestamp предыдущего запроса. Если указать 0, то будут выгружены товары за всё время.

<aside class="notice">
Удалённые товары возвращаются только тогда, когда ts <b>НЕ равен нулю</b>.
</aside>


В ответ же будет возращён JSON-объект следующего содержания:

```json
{
  "code": "ok",
  "data": [
    {
      "id": 577,
      "name": "Белый хлеб",
      "entityType": "product",
      "cancellationType": "cancel_dish",
      "measure": "pcs",
      "deleted": false,
      "useDiscounts": true,
      "color": "gray",
      "shortDescription": null,
      "alternateName": "Белый хлеб",
      "buttonColor": null,
      "image": "1.jpeg",
      "type": "simple",
      "coefficients": null,
      "code": "",
      "active": false,
      "freePrice": false,
      "descr": "product",
      "workSpace": {
        "id": 2
      },
      "category_id": 65,
      "tax_id": 1,
      "price": 4,
      "self_cost": 0,
      "image_path": "https://imagehosting.net/images/folder/image.jpeg",
      "in_stop_list": true
    }
  ]
}
```

| Название         |       Тип       | Описание                                       |
|------------------|:---------------:|------------------------------------------------|
| code             |     string      | ???                                            |
| data             |     object      |                                                |
| id               |       int       | ID товара в Caffesta                           |
| name             |     string      | наименование товара                            |
| entityType       |     string      | тип товара                                     |
| cancellationType |     string      | метод списания                                 |
| measure          |     string      | единица измерения                              |
| deleted          |      bool       | удалён ли товар (`true/false`)                 |
| useDiscounts     |      bool       | применяется ли скидка (`true/false`)           |
| color            |     string      | ???                                            |
| shortDescription | string или null | краткое описание товара                        |
| alternateName    | string или null | название, которое отображается в меню          |
| buttonColor      | string или null | цвет кнопки товара                             |
| image            |     string      | используемая картинка (относ.путь)             |
| type             |     string      | вид реализации                                 |
| coefficients     | string или null | ???                                            |
| code             |     string      | внешний код товара                             |
| active           |      bool       | доступен ли для продажи (`true/false`)         |
| freePrice        |      bool       | имеет ли свободную цену (`true/false`)         |
| descr            |       int       | ???                                            |
| workSpace        |     object      |                                                |
| id               |       int       | ID цеха                                        |
| category_id      |       int       | ID категории                                   |
| workSpace_id     |       int       | ID цеха                                        |
| tax_id           |       int       | ID налога на товар                             |
| price            |      float      | цена товара за ед.измерения                    |
| self_cost        |      float      | себестоимость                                  |
| image_path       |     string      | абсолютный путь к картинке товара              |
| in_stop_list     |      bool       | находится ли товар в стоп-листе (`true/false`) |

## 3. Получение остатка по товарам

> Пример запроса

```python
# TODO
```

### HTTP запрос

`GET https://{account_name}.caffesta.com/a/v1.0/draft/get_balances/{posId}/{ts}`

Получение информации по остаткам товаров происходит при обращении по пути
`/a/v1.0/draft/get_balances/{posId}/{ts}` c использованием метода **GET** и передачей уточняющих переменных:

* posId - ID точки продаж в Caffesta, для которой нужно получить данные по товарам;
* ts - timestamp предыдущего запроса. Если указать 0, то будут выгружены все товары со всеми остатками за всё время.

В ответ же будет возращён JSON-объект следующего содержания:

```json
{
  "code": "ok",
  "data": [
    {
      "balance": "20.000",
      "self_cost": "0.000000",
      "product_id": 1
    }
  ]
}
```

| Название   |  Тип   | Описание       |
|------------|:------:|----------------|
| code       | string | ???            |
| data       | object |                |
| balance    | float  | Остаток товара |
| self_cost  | float  | Себестоимость  |
| product_id |  int   | ID товара      |

## 4. Получение списка клиентов

> Пример запроса

```python
# TODO
```

### HTTP запрос

`GET https://{account_name}.caffesta.com/a/v1.0/draft/get_clients/{ts}`

Получение информации по остаткам товаров происходит при обращении по пути
`/a/v1.0/draft/get_clients/{ts}` c использованием метода **GET** и передачей вместо `{ts}` timestamp предыдущего
запроса.
Если указать 0, то будут выгружены все клиенты со всеми накоплениями за всё время.

В ответ же будет возращён JSON-объект следующего содержания:

```json
{
  "code": "ok",
  "data": [
    {
      "uuid": "64b14c01-c9d1-4a72-95f6-b3bd51488293",
      "name": "Сидор",
      "lastName": "Сидоров",
      "normName": "сидор сидоров",
      "phone": "+375 29 182-14-62",
      "address": "пр.Победителей",
      "account": 0,
      "normPhone": "375291821462",
      "cardNumber": "111155553333",
      "bonusBalance": 3989.13,
      "pointBalance": 0,
      "payBalance": 20981.5,
      "email": null,
      "deletedAt": null,
      "group_id": 1,
      "birth_day_discount_amount": 15,
      "discount_type": "bonus",
      "bonus_max_bill_rate": 50,
      "discount_name": "Бонусная 20%",
      "discount_rate": 0.2,
      "group_float": false,
      "discount_group": {
        "id": 1
      },
      "promotionKeeps": [
        {
          "id": 1,
          "amount": "{\"19\":9}",
          "promotion": {
            "id": 3,
            "active": true,
            "deleted": false
          }
        },
        {
          "id": 7,
          "amount": "{\"108\":18,\"109\":16,\"110\":14}",
          "promotion": {
            "id": 7,
            "active": false,
            "deleted": false
          }
        }
      ]
    }
  ]
}
```

| Название                  |       Тип       | Описание                                                |
|---------------------------|:---------------:|---------------------------------------------------------|
| code                      |     string      | ???                                                     |
| data                      |     object      |                                                         |
| uuid                      |     string      | UUID клиента в Caffesta                                 |
| name                      |     string      | Имя клиента                                             |
| lastName                  |     string      | Фамилия клиента                                         |
| normName                  |     string      | Имя целиком                                             |
| phone                     |     string      | номер телефона клиента                                  |
| address                   |     string      | адрес клиента (также это и адрес доставки)              |
| account                   |      float      | счёт клиента                                            |
| normPhone                 |     string      | номер телефона(только цифры, без спецсимволов)          |
| cardNumber                |     string      | скидочная карта клиента                                 |
| bonusBalance              |      float      | бонусы клиента                                          |
| pointBalance              |      float      | баллы клиента                                           |
| payBalance                |      float      | сумма покупок                                           |
| email                     | string или null | E-mail клиента                                          |
| deletedAt                 | string или null | дата удаления клиента                                   |
| group_id                  |       int       | ???                                                     |
| birth_day_discount_amount |       int       | скидка на день рождения (не используется)               |
| discount_type             |     string      | тип программы лояльности скидочной группы               |
| bonus_max_bill_rate       |       int       | максимальная сумма оплаты заказа бонусами               |
| discount_name             |     string      | имя скидочной группы                                    |
| discount_rate             |      float      | размер бонусов/скидки на чек (0.2 = 20%)                |
| group_float               |      bool       | ???                                                     |
| discount_group            |     object      |                                                         |
| id                        |       int       | ID скидочный группы клиента                             |
| promotionKeeps            |     object      | список акций и накоплений по ним                        |
| id                        |       int       | ???                                                     |
| amount                    |     object      | количество накоплений по акции (ID товара и количество) |
| promotion                 |     object      |                                                         |
| id                        |       int       | ID акции                                                |
| active                    |      bool       | активна ли акция (`true/false`)                         |
| deleted                   |      bool       | удалена ли акция (`true/false`)                         |

## 5. Получение категорий товаров

> Пример запроса

```python
# TODO
```

### HTTP запрос

`GET https://{account_name}.caffesta.coma/v1.0/draft/get_categories`

Получение информации по остаткам товаров происходит при обращении по пути
`a/v1.0/draft/get_categories` c использованием метода **GET**.

В ответ же будет возращён JSON-объект следующего содержания:

```json
{
  "code": "ok",
  "data": [
    {
      "id": 2,
      "name": "Главная",
      "buttonColor": "#D3D3D3",
      "image": null,
      "lft": 2,
      "lvl": 1,
      "availableForPos": true,
      "useMarking": false,
      "type": null,
      "depositId": null,
      "position": 2,
      "parent": null,
      "course": null,
      "deposit_id": null
    }
  ]
}
```

| Название        |  Тип   | Описание                                          |
|-----------------|:------:|---------------------------------------------------|
| code            | string | ???                                               |
| data            | object |                                                   |
| id              |  int   | ID категории                                      |
| name            | string | Название категории                                |
| buttonColor     | string | Цвет кнопки категории                             |
| image           |  int   | Картинка категории                                |
| lft             |  int   | ???                                               |
| lvl             |  int   | Уровень вложенности папки                         |
| availableForPos |  bool  | Отображается ли на точке продаж (`true/false`)    |
| useMarking      |  bool  | Используется ли маркировка товаров (`true/false`) |
| type            |  int   | ???                                               |
| depositId       |  int   | ???                                               |
| position        |  int   | Позиция в категории                               |
| parent          |  int   | ID родительской категории                         |
| course          |  int   | ID курса подачи                                   |
| deposit_id      |  int   | ID товара-депозита                                |

## 6. Получение цехов и отделов

> Пример запроса

```python
# TODO
```

### HTTP запрос

`GET https://{account_name}.caffesta.coma/v1.0/draft/get_workspaces`

Получение информации по остаткам товаров происходит при обращении по пути
`a/v1.0/draft/get_workspaces` c использованием метода **GET**.

В ответ же будет возращён JSON-объект следующего содержания:

```json
{
  "code": "ok",
  "data": [
    {
      "id": 1,
      "group": 1,
      "refCode": 123,
      "name": "Кухня"
    },
    {
      "id": 2,
      "group": 1,
      "refCode": null,
      "name": "Бар"
    }
  ]
}
```

| Название |  Тип   | Описание         |
|----------|:------:|------------------|
| code     | string | ???              |
| data     | object |                  |
| id       |  int   | ID цеха          |
| group    |  int   |                  |
| refCode  |  int   | Внешний код цеха |
| name     | string | Название         |

## 7. Получение налогов

> Пример запроса

```python
# TODO
```

### HTTP запрос

`GET https://{account_name}.caffesta.com/a/v1.0/draft/get_taxes`

Получение информации по остаткам товаров происходит при обращении по пути
`/a/v1.0/draft/get_taxes` c использованием метода **GET**.

В ответ же будет возращён JSON-объект следующего содержания:

```json
{
  "code": "ok",
  "data": [
    {
      "id": 1,
      "refId": null,
      "refCode": null,
      "name": "Без налога",
      "type": "inner",
      "value": 0,
      "fiscal": false,
      "ref_id": 1
    },
    {
      "id": 2,
      "refId": null,
      "refCode": null,
      "name": "НДС",
      "type": "inner",
      "value": 20,
      "fiscal": false,
      "ref_id": 1
    }
  ]
}

```

| Название |     Тип      | Описание                                      |
|----------|:------------:|-----------------------------------------------|
| code     |    string    | ???                                           |
| data     |    object    |                                               |
| id       |     int      | ID налога                                     |
| refId    | int или null | ???                                           |
| refCode  | int или null | Внешний код                                   |
| name     |    string    | Название                                      |
| type     |    string    | Тип налога (`НДС - inner, с продажи - outer`) |
| value    |    float     | Ставка налога                                 |
| fiscal   |     bool     | Фискальный ли (`true/false`)                  |
| ref_id   |     int      | Номер налога в ФР                             |

## 8. Получение групп модификаторов

> Пример запроса

```python
# TODO
```

### HTTP запрос

`GET https://{account_name}.caffesta.com/a/v1.0/draft/get_modifier_groups`

Получение информации по остаткам товаров происходит при обращении по пути
`/a/v1.0/draft/get_taxes` c использованием метода **GET**.

В ответ же будет возращён JSON-объект следующего содержания:

```json
{
  "code": "ok",
  "data": [
    {
      "id": 1,
      "name": "Сиропы",
      "multiple": true,
      "min": 0,
      "max": 10,
      "deleted": false,
      "productModifiers": [
        {
          "id": 100
        },
        {
          "id": 101
        }
      ],
      "items": [
        {
          "id": 2,
          "name": "Клубника",
          "image": null,
          "count": "1.000",
          "price": "1.00",
          "product": {
            "id": 9900,
            "descr": "product"
          }
        },
        {
          "id": 3,
          "name": "Малина",
          "image": null,
          "count": "1.000",
          "price": "1.00",
          "product": {
            "id": 9898,
            "descr": "product"
          }
        }
      ]
    }
  ]
}
```

| Название         |  Тип   | Описание                                              |
|------------------|:------:|-------------------------------------------------------|
| code             | string | ???                                                   |
| data             | object |                                                       |
| id               |  int   | ID группы модификаторов                               |
| name             | string | Наименование                                          |
| multiple         |  bool  | Можно выбрать несколько модификаторов (`true/false`)  |
| min              |  int   | Минимальное количество                                |
| max              |  int   | Максимальное количество                               |
| deleted          |  bool  | Удалена ли группа (не используется; `true/false`)     |
| productModifiers | object | Список техкарт с привязанными группами модификаторов  |
| id               |  int   | ID техкарты, к которой привязана группа модификаторов |
| items            | object | Список товаров-модификаторов в группе                 |
| id               |  int   | ID модификатора                                       |
| name             | string | Наименование                                          |
| image            | string | Картинка, если есть                                   |
| count            | string | Количество (за штуку, грамм, милилитр)                |
| price            | string | Цена за поле **count**                                |
| product          | object |                                                       |
| id               |  int   | ID товара-модификатора                                |
| descr            |  int   | Тип товара                                            |

## 9. Получение изменений с точки продаж

> Пример запроса

```python
# TODO
```

### HTTP запрос

`GET https://{account_name}.caffesta.com/a/v1.0/draft/get_product_shop_data/{posId}/{ts}`

Получение информации по остаткам товаров происходит при обращении по пути
`/a/v1.0/draft/get_taxes` c использованием метода **GET** и передачей уточняющих переменных:

* posId - ID точки продаж в Caffesta, с которой нужно получить изменения;
* ts - timestamp предыдущего запроса из пункта 8.1

В ответ же будет возращён JSON-объект следующего содержания:

```json
{
  "code": "ok",
  "data": [
    {
      "price": "5.00",
      "avgInvoicedSelfCost": "0.000000",
      "availableForSale": true,
      "inStopList": true,
      "shop_id": 3,
      "product_id": 1
    }
  ]
}
```

| Название            |  Тип   | Описание                                        |
|---------------------|:------:|-------------------------------------------------|
| code                | string | ???                                             |
| data                | object |                                                 |
| price               | string | Цена товара на точке продаж                     |
| avgInvoicedSelfCost | string | Себестоимость товара                            |
| availableForSale    |  bool  | Доступен ли для продажи на точке (`true/false`) |
| inStopList          |  bool  | Находится ли в стоп-листе (`true/false`)        |
| shop_id             |  int   | ID точки продажи                                |
| product_id          |  int   | ID товара в Caffesta                            |




