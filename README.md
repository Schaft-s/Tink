# Tink

1. API предоставляет возможность управления блокировками платежей клиентов, а также проверку состояния блокировки. API позволяет блокировать и разблокировать платежи клиентов, а также различать типы блокировок (например, подозрение на мошенничество и блокировки для выяснения реквизитов).  
Файл с кодом лежит [рядом](API_file.md). 


2. Для хранения информации о блокировках платежей клиентов в базе данных предлагается следующая структура таблиц:

- Таблица clients
  
|Поле |Тип |Описание |
|-|--------|---|
| client_id | VARCHAR | Уникальный идентификатор клиента |
| name | VARCHAR |		Имя клиента |
| email | 	VARCHAR	| Email клиента |
|created_at|	TIMESTAMP|	Дата создания записи |

- Таблица payment_blocks
  
|Поле	|Тип	|Описание|
|-|--------|---|
|block_id	|SERIAL	|Уникальный идентификатор блокировки|
|client_id|	VARCHAR|	Идентификатор клиента|
|reason|	VARCHAR	|Причина блокировки (fraud, invalid_details)|
|blocked_at	|TIMESTAMP	|Дата блокировки|
|unblocked_at|	TIMESTAMP|	Дата разблокировки|

4. Пример кода для таблиц:
```sql
CREATE TABLE clients (
    client_id VARCHAR PRIMARY KEY,
    name VARCHAR NOT NULL,
    email VARCHAR NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE payment_blocks (
    block_id SERIAL PRIMARY KEY,
    client_id VARCHAR NOT NULL,
    reason VARCHAR NOT NULL CHECK (reason IN ('fraud', 'invalid_details')),
    blocked_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    unblocked_at TIMESTAMP,
    FOREIGN KEY (client_id) REFERENCES clients(client_id)
);
```

4. Пример использования API
- Запрос-Ответ
```
POST /block
Content-Type: application/json

{
  "client_id": "client123",
  "reason": "fraud"
}

--- 200 ОК
```

- Запрос-Ответ
```
POST /unblock
Content-Type: application/json

{
  "client_id": "client123"
}

--- 200 ОК
```

- Запрос-Ответ
```
GET /check?client_id=client123


---{
---  "client_id": "client123",
---  "blocked": true,
---  "reason": "fraud"
---}

```
