
# REST API Development Guidelines with Best Practices and Versioning

## Layered Architecture

To ensure consistency, scalability, and maintainability, we follow a structured layered approach for REST API development:

**Flow:**  
`Controller → AdapterService → BusinessService [→ IntegrationService (if required)] → DaoService → Database`

### Layer Responsibilities

- **Controller**  
  Handles incoming HTTP requests and delegates processing to the `AdapterService`.  
  Responsible for API versioning (`/v1/`, `/v2/`, etc.).

- **AdapterService**  
  Converts incoming **DTO requests** from the controller into **service requests** that are understood by the business layer.  
  Helps maintain the same core business logic across multiple API versions.

- **BusinessService**  
  Contains core business logic.  
  Should be independent of DTOs and controller-specific logic.  
  Can delegate to **IntegrationService** if external service communication is required.

- **IntegrationService (optional)**  
  Handles interactions with third-party systems, external APIs, or other microservices.  
  Keeps external integration logic isolated from core business logic.

- **DaoService**  
  Responsible for persistence logic and database interactions.  
  Abstracts the data access layer.

- **Database**  
  The underlying data store.

### Key Benefits

- **Consistency** – Same business logic can be reused across multiple versions of the API.  
- **Separation of Concerns** – Each layer has a well-defined responsibility.  
- **Maintainability** – Changes in one layer (e.g., DTO changes in Controller) do not affect others.  
- **Extensibility** – Easy to add new versions or integrations without duplicating logic.  

---


# API Documentation for new task

## **Endpoint**

`POST /api/controller/v1/verb`

---

## **Sample Request**

```json
{
  "userId": "123456",
  "action": "CREATE",
  "timestamp": "2025-07-26T10:00:00Z"
}
```

---

## **Request Specification**

| Field Name  | Type     | Mandatory | Allowed Values         | Description                    |
| ----------- | -------- | --------- | ---------------------- | ------------------------------ |
| `userId`    | String   | Yes       | Alphanumeric           | Unique identifier for the user |
| `action`    | String   | Yes       | CREATE, UPDATE, DELETE | Action to be performed         |
| `timestamp` | DateTime | No        | ISO 8601 format        | Time of request execution      |

---

## **Sample Response**

```json
{
  "status": "SUCCESS",
  "message": "Action completed successfully",
  "referenceId": "ABC123XYZ"
}
```

---

## **Response Specification**

| Field Name    | Type   | Mandatory | Allowed Values   | Description                      |
| ------------- | ------ | --------- | ---------------- | -------------------------------- |
| `status`      | String | Yes       | SUCCESS, FAILURE | Status of the API request        |
| `message`     | String | Yes       | Any string       | Descriptive message              |
| `referenceId` | String | No        | Alphanumeric     | Unique reference for the request |

---

## **Error Codes** /http and application specific

| Error Code | Message                 | Description                               |
| ---------- | ----------------------- | ----------------------------------------- |
| 400        | Invalid request payload | Malformed JSON or missing required fields |
| 401        | Unauthorized            | Authentication failed or missing token    |
| 403        | Forbidden               | User doesn't have permission              |
| 404        | Resource not found      | Endpoint or entity doesn't exist          |
| 500        | Internal server error   | Unexpected failure in server-side logic   |

---

## **Assumptions**

* The client sends a valid and authorized token (if required by your API).
* All date-time values follow ISO 8601 format (`YYYY-MM-DDTHH:mm:ssZ`).
* The `action` field drives the internal logic and may have side-effects like database changes.
* The server is configured to respond in `application/json` format.
* Field validations (e.g., userId length, action enums) are performed server-side.


