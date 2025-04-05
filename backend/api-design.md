# 🌐 API Design – Standard Response Format

A consistent API response structure improves frontend integration, debugging, and uniformity across services.

---

## ✅ Standard Response Structure

```json
{
  "status": "success",
  "code": 200,
  "message": "Request successful",
  "data": { ... },
  "error": null,
  "metadata": {
    "requestId": "abc-123",
    "timestamp": "2025-04-05T09:30:00Z",
    "executionTimeMs": 23
  }
}
```

---

## 🔍 Field Descriptions

| Field       | Type        | Description                                                  |
|-------------|-------------|--------------------------------------------------------------|
| `status`    | string      | "success" / "fail" / "error"                           |
| `code`      | integer     | HTTP status or application-level status code                 |
| `message`   | string      | Human-readable description                                   |
| `data`      | object/null | The actual payload                                           |
| `error`     | object/null | Structured error info (only when status ≠ "success")         |
| `metadata`  | object      | Diagnostic info (timestamps, request IDs, etc.)              |

---

## ❌ Error Object Structure

```json
{
  "code": "ERR-VALIDATION",
  "message": "Validation failed",
  "details": [
    {
      "field": "email",
      "message": "Email must not be blank"
    }
  ],
  "stackTrace": "Optional - visible only in non-prod"
}
```

| Field        | Description                              |
|--------------|------------------------------------------|
| `code`       | App-specific error identifier            |
| `message`    | Summary of the error                     |
| `details`    | Field-specific errors (optional)         |
| `stackTrace` | Stack trace for debugging (optional)     |

---

## 🧪 Success Example

```json
{
  "status": "success",
  "code": 200,
  "message": "User created",
  "data": {
    "id": 101,
    "name": "Santosh"
  },
  "error": null,
  "metadata": {
    "requestId": "req-9876",
    "timestamp": "2025-04-05T10:20:00Z",
    "executionTimeMs": 12
  }
}
```

---

## 🧨 Error Example

```json
{
  "status": "fail",
  "code": 400,
  "message": "Validation error",
  "data": null,
  "error": {
    "code": "ERR-VALIDATION",
    "message": "Missing fields",
    "details": [
      {
        "field": "email",
        "message": "must not be blank"
      }
    ]
  },
  "metadata": {
    "requestId": "req-1234",
    "timestamp": "2025-04-05T10:22:00Z",
    "executionTimeMs": 15
  }
}
```

---

## 📘 OpenAPI Schema Definitions (YAML)

### 🔧 Base Schemas

```yaml
components:
  schemas:
    ApiResponseBase:
      type: object
      properties:
        status:
          type: string
          enum: [success, fail, error]
        code:
          type: integer
        message:
          type: string
        error:
          $ref: '#/components/schemas/ErrorDetail'
        metadata:
          $ref: '#/components/schemas/Metadata'

    ErrorDetail:
      type: object
      properties:
        code:
          type: string
        message:
          type: string
        details:
          type: array
          items:
            $ref: '#/components/schemas/FieldError'
        stackTrace:
          type: string
          nullable: true

    FieldError:
      type: object
      properties:
        field:
          type: string
        message:
          type: string

    Metadata:
      type: object
      properties:
        requestId:
          type: string
        timestamp:
          type: string
          format: date-time
        executionTimeMs:
          type: integer
        traceId:
          type: string
          nullable: true
```

---

## 🔀 Generic Response Wrapping (Simulated)

Because OpenAPI does not support generics, use `allOf` to reuse the response wrapper with different data payloads.

### Example

```yaml
components:
  schemas:
    ApiResponse_User:
      allOf:
        - $ref: '#/components/schemas/ApiResponseBase'
        - type: object
          properties:
            data:
              $ref: '#/components/schemas/User'

    ApiResponse_UserList:
      allOf:
        - $ref: '#/components/schemas/ApiResponseBase'
        - type: object
          properties:
            data:
              type: array
              items:
                $ref: '#/components/schemas/User'
```

Use in a path:

```yaml
paths:
  /users:
    get:
      summary: Get list of users
      responses:
        '200':
          description: Successful response
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ApiResponse_UserList'
```

---

## 🧐 Tips for Spring Boot

- Create a generic `ApiResponse<T>` wrapper class
- Use `@ControllerAdvice` for centralized exception handling
- Use `HandlerInterceptor` to set metadata (timestamp, requestId)
- Filter `stackTrace` from responses in production
- Make `metadata` default through a response builder or response factory

---

