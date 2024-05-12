# Аутентификация

> Пример запроса

```python
import requests

url = "https://{account_name}.caffesta.com/a/v1.0/storage/test"
headers = {"X-API_KEY": "CAFFESTA-X-API-KEY"}
resp = requests.get(url, headers=headers)

print(resp)
```

Аутентификация происходит путем передачи параметра *X-API-KEY* в заголовке запроса. Проверить прохождение аутентификации
можно, обратившись по тестовому пути `/a/v1.0/storage/test`.

**HTTP запрос**

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
