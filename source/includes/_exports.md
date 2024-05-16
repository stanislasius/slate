# 7. Получение данных по реализации позиций

Строка запроса должна быть преобразована с помощью функции
urlencode(). https://www.php.net/manual/ru/function.urlencode.php
Дата отправляется в формате ISO 8601. Временная зона по умолчанию, та которая выставлена в настройках (часовой пояс).
Используются параметры start и end, выборка возможна за любой период. Допустимы следующие варианты установки даты:
2018-11-05 - время подставится 00:00, временная зона возьмется из настроек;
2018-11-05T05:00 - с передачей времени, временная зона возьмется из настроек;
2018-11-05T05:00+02:00 - с передачей времени и своего часового пояса;
2018-11-05 — только с параметром shift_day, выборка за определённый кассовый день.

Есть три варианта получения реализации из Caffesta:

1) С группировкой по точке продаж, цене, ID товара;
2) С группировкой только по точке продаж;
3) Без группировки, за конкретный кассовый день

## Вариант 1. Группировка по точке продаж, цене, ID товара

> Пример запроса

```python
# TODO
```

### HTTP запрос для получения данных по календарным дням

`GET https://{account_name}.caffesta.com/a/v1.0/product/export_sales?start={start_date}&end={end_date}`

Получение данных по реализации _за календарные дни_ происходит при обращении по
пути `/a/v1.0/product/export_sales?start={start_date}&end={end_date}`
c использованием метода **GET** и передачей вместо `{start_date}` начальной даты для формирования реализации,
а вместо `{end_date}` - конечная дата.

### HTTP запрос для получения данных по одному кассовому дню

`GET https://{account_name}.caffesta.com/a/v1.0/product/export_sales?shift_day={shift_day}`

Получение данных по реализации _за кассовые дни_ происходит при обращении по пути
`/a/v1.0/product/export_sales?shift_day={shift_day}` c использованием метода **GET** и передачей вместо
`{shift_day}` дату кассового дня, за который нужно получить реализацию.

В ответ на запрос будет возвращён JSON-объект следующего содержания:

```json
{
  "data": [
    {
      "caf_terminal_id": "1",
      "caf_id": "1002",
      "name": "Салат Мимоза",
      "ref_code": "1002",
      "price": "1.7000",
      "qnt": "10",
      "deleted": "0",
      "cash": "0",
      "card": "0",
      "cashless": "0",
      "bonus": "0",
      "credit": "0",
      "total_pay": "17"
    }
  ],
  "success": true
}
```

| Название        |  Тип   | Описание                           |
|-----------------|:------:|------------------------------------|
| data            | object |                                    |
| caf_terminal_id | string | ID точки продаж                    |
| caf_id          | string | ID товара                          |
| name            | string | Наименование товара                |
| ref_code        | string | Внешний код                        |
| price           | string | Цена за единицу измерения          |
| qnt             | string | Кол-во товара                      |
| deleted         | string | Удалён ли товар (`true/false`)     |
| cash            | string | Наличными                          |
| card            | string | Банковской картой                  |
| cashless        | string | Безналичными (на р/с)              |
| bonus           | string | Бонусами                           |
| credit          | string |                                    |
| total_pay       | string | Итоговая сумма                     |
| success         |  bool  | Успешна ли операция (`true/false`) |

## Вариант 2. Группировка по точке продаж

> Пример запроса

```python
# TODO
```

### HTTP запрос для получения данных по календарным дням

`GET https://{account_name}.caffesta.com/a/v1.0/product/export_sales_totals?start={start_date}&end={end_date}`

Получение данных по реализации _за календарные дни_ происходит при обращении по
пути `/a/v1.0/product/export_sales_totals?start={start_date}&end={end_date}`
c использованием метода **GET** и передачей вместо `{start_date}` начальной даты для формирования реализации,
а вместо `{end_date}` - конечная дата.

### HTTP запрос для получения данных по одному кассовому дню

`GET https://{account_name}.caffesta.com/a/v1.0/product/export_sales_totals?shift_day={shift_day}`

Получение данных по реализации _за кассовые дни_ происходит при обращении по пути
`/a/v1.0/product/export_sales_totals?shift_day={shift_day}` c использованием метода **GET** и передачей вместо
`{shift_day}` дату кассового дня, за который нужно получить реализацию.

В ответ на запрос будет возвращён JSON-объект следующего содержания:

```json
{
  "data": [
    {
      "qnt": "16306",
      "caf_terminal_id": null,
      "cafe_workspace_id": "1",
      "cash": "5519.67",
      "card": "2737.40",
      "cashless": "0.00",
      "credit": "0.00",
      "discount": "-35.35",
      "total_pay": "8257.07"
    }
  ],
  "success": true
}
```

| Название          |     Тип      | Описание                           |
|-------------------|:------------:|------------------------------------|
| data              |    object    |                                    |
| qnt               |    string    |                                    |
| caf_terminal_id   | int или null | ID точки продаж                    |
| cafe_workspace_id |    string    | ID цеха                            |
| cash              |    string    | Сумма наличными                    |
| card              |    string    | Банковская карта                   |
| cashless          |    string    | Безналичными (на р/с)              |
| credit            |    string    |                                    |
| discount          |    string    | Сумма скидки                       |
| total_pay         |    string    | Итоговая сумма                     | 
| success           |     bool     | Успешна ли операция (`true/false`) |

## Вариант 3. Получение реализации за кассовый день без группировок

> Пример запроса

```python
# TODO
```

### HTTP запрос

`GET https://{account_name}.caffesta.com/a/v1.0/draft/receipts_by_shift_day/{terminal_ID}/{Date}`

Получение данных по реализации происходит при обращении по
пути `/a/v1.0/draft/receipts_by_shift_day/{terminal_ID}/{Date}`
c использованием метода **GET** и передачей вместо `{terminal_ID}`ID точки продаж для выгрузки,
а вместо `{Date}` - кассовый день, за который нужно получить данные.

В ответ на запрос будет возвращён JSON-объект следующего содержания:

```json
[
  {
    "id": "4f52125c-58ea-touc-0301-650467649790",
    "type": "sale",
    "income": 1,
    "total": 8,
    "total_sum": 8,
    "discount_sum": 0,
    "client": {
      "id": 2
    },
    "created_at": "2022-04-20T18:14:14+03:00",
    "updated_at": "2022-04-20T18:14:48+03:00",
    "bill_dishes": [
      {
        "count": 2,
        "price": 4,
        "self_cost_sum": 0,
        "total": 8,
        "discount_sum": 0,
        "total_sum": 8,
        "dish": {
          "id": 491
        }
      }
    ],
    "count_clients": 1,
    "cash_pay": 8,
    "card_pay": 0,
    "back_pay": 0
  }
]   
```

| Название      |  Тип   | Описание                                                   |
|---------------|:------:|------------------------------------------------------------|
| id            | string | ID чека в Caffesta                                         |
| type          | string | Тип транзакции (`sale — продажа, prepayment — предоплата`) |
| income        |  int   | Тип операции (`приход средств — '1', расход — '-1'`)       |
| total         | float  | Подытог по чеку                                            |
| total_sum     | float  | ИТОГО с учётом скидок по чеку                              |
| discount_sum  | float  | Сумма скидок по чеку                                       |
| client        | object | Сумма наличными                                            |
| id            |  int   | ID клиента                                                 |
| created_at    | string | Время создания чека                                        |
| updated_at    | string | Время его оплаты                                           |
| bill_dishes   | string | Массив заказанных позиций                                  |
| count         | float  | Количество                                                 |
| price         | float  | Цена за единицу измерения                                  |
| self_cost_sum | float  | Себестоимость позиции                                      |
| total         | float  | Подытог по позиции                                         |
| discount_sum  | float  | Сумма скидки на позицию                                    |
| total_sum     | float  | Итого по позиции                                           |
| dish          | object |                                                            |
| id            |  int   | ID товара в чеке                                           |
| count_clients |  int   | Количество клиентов                                        |
| cash_pay      | float  | Наличными                                                  |
| card_pay      | float  | Банковской картой                                          |
| back_pay      | float  | Выдано сдачи                                               |
