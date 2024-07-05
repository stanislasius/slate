# 1. Аутентификация

> Пример простейшего запроса

```python
import requests

api_key = "CAFFESTA-X-API-KEY"

url = "https://{account_name}.caffesta.com/a/v1.0/storage/test"
headers = {"X-API_KEY": api_key}

resp = requests.get(url, headers=headers)

print(resp)
```

```shell
curl "https://touchar2.caffesta.com/a/v1.0/storage/test" \
  -H "X-API-KEY: 7c907e873933f46c3d48c5fdffaaa2ac" 
```

```http
GET /a/v1.0/storage/test HTTP/1.1
Host: touchar2.caffesta.com:443
X-API-KEY: 7c907e873933f46c3d48c5fdffaaa2ac
```

Аутентификация происходит путем передачи параметра *X-API-KEY* в заголовке запроса. Проверить прохождение аутентификации
можно, обратившись по тестовому пути `/a/v1.0/storage/test`.

**HTTP запрос**

`GET https://{account_name}.caffesta.com/a/v1.0/storage/test`

<aside class="notice">
Обязательно проверьте, заменили ли вы <u>CAFFESTA-X-API-KEY</u> на <b>нужный X-API-KEY</b>, а также и <u>{account_name}</u> 
в запросе на <b>нужное</b> имя аккаунта.
</aside>

**Структура ответа**

> Ответ на запрос

```json
{
  "success": true,
  "data": {
    "id": "test"
  }
}
```

| Название | Тип    | Описание                   |
|----------|--------|----------------------------|
| status   | bool   | Успешна ли операция        |
| data     | object |                            |
| id       | string | Строка с тестовыми данными |
