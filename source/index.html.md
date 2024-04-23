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

# Введение

```
Выберите язык, пример кода для которого хотите увидеть.
```

Данный протокол взаимодействия по API с Caffesta предназначен для получения или передачи данных по разным разделам
программы.

Весь обмен данными происходит с использованием JSON-объектов.

Кодировка передаваемого текста - **UTF-8**.

### Важно

Все объекты, у которых есть `ref_code` создаются на основе данных из интегрирующейся программы. Caffesta будет сначала искать объект с переданным значением в `ref_code`, и если не найдёт совпадений, создаст новый объект.
Примеры таких объектов: 
  * Внешний код категории - category_ref_code
  * Внешний код цеха/отдела - workspace_ref_code
  * Внешний код товара - product_code 


Также при реализации некоторых возможностей через API могут встречаться объекты (формата `caf_*_id`), которые должны уже быть созданы. Например
  * caf_*_id - эти объекты должны быть созданы заранее. Это могут быть склады (можно создать вручную в административной части программы) или точки продаж(нужно связаться с поддержкой Caffesta для создания). 
Пример объектов: caf_storage_id (ID склада), caf_shop_id (ID точки продаж);



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
можно,
обратившись по тестовому пути `/a/v1.0/storage/test`.


<aside class="notice">
Обязательно проверьте, заменили ли вы <b>CAFFESTA-X-API-KEY</b> на <u>нужный X-API-KEY</u>, а также и <b>{account_name}</b> на <u>нужное</u> имя аккаунта.
</aside>

### Структура ответа

> 200 Ответ

```json
{
  "success": true,
  "data": {
    "id": "test"
  }
}
```

| Название | Тип                     | Описание                           |
|----------|-------------------------|------------------------------------|
| status   | обязательный<br/>bool   | Булево значение true               |
| data     | Обязательный<br/>object | Объект, содержащий тестовые данные |
| id       | обязательный<br/>string | Строка "test"                      |

# Kittens

## Get All Kittens

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens" \
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

#### HTTP Request

`GET http://example.com/api/kittens`

#### Query Parameters

 Parameter    | Default | Description                                                                      
--------------|---------|----------------------------------------------------------------------------------
 include_cats | false   | If set to true, the result will also include cats.                               
 available    | true    | If set to false, the result will include kittens that have already been adopted. 

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

### Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2" \
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

#### HTTP Request

`GET http://example.com/kittens/<ID>`

#### URL Parameters

 Parameter | Description                      
-----------|----------------------------------
 ID        | The ID of the kitten to retrieve 

### Delete a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.delete(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.delete(2)
```

```shell
curl "http://example.com/api/kittens/2" \
  -X DELETE \
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.delete(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted": ":("
}
```

This endpoint deletes a specific kitten.

#### HTTP Request

`DELETE http://example.com/kittens/<ID>`

#### URL Parameters

 Parameter | Description                    
-----------|--------------------------------
 ID        | The ID of the kitten to delete 

