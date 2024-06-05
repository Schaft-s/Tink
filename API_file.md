# Payment Block Management API

API предоставляет возможность управления блокировками платежей клиентов, а также проверку состояния блокировки. API позволяет блокировать и разблокировать платежи клиентов, а также различать типы блокировок (например, подозрение на мошенничество и блокировки для выяснения реквизитов).

## Спецификация OpenAPI/Swagger

```yaml
openapi: 3.0.0
info:
  title: Payment Block Management API
  version: 1.0.0
  description: API для управления блокировками платежей клиентов

paths:
  /block:
    post:
      summary: Блокировка платежей клиента
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                client_id:
                  type: string
                  example: "client123"
                reason:
                  type: string
                  enum: ["fraud", "invalid_details"]
                  example: "fraud"
              required:
                - client_id
                - reason
      responses:
        '200':
          description: Клиент успешно заблокирован
        '400':
          description: Неверный запрос
  /unblock:
    post:
      summary: Разблокировка платежей клиента
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                client_id:
                  type: string
                  example: "client123"
              required:
                - client_id
      responses:
        '200':
          description: Клиент успешно разблокирован
        '400':
          description: Неверный запрос
  /check:
    get:
      summary: Проверка статуса блокировки клиента
      parameters:
        - name: client_id
          in: query
          required: true
          schema:
            type: string
            example: "client123"
      responses:
        '200':
          description: Статус блокировки клиента
          content:
            application/json:
              schema:
                type: object
                properties:
                  client_id:
                    type: string
                    example: "client123"
                  blocked:
                    type: boolean
                    example: true
                  reason:
                    type: string
                    enum: ["fraud", "invalid_details", "none"]
                    example: "fraud"
        '400':
          description: Неверный запрос

components:
  schemas:
    ClientBlockStatus:
      type: object
      properties:
        client_id:
          type: string
          example: "client123"
        blocked:
          type: boolean
          example: true
        reason:
          type: string
          enum: ["fraud", "invalid_details", "none"]
          example: "fraud"
