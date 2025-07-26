
# API Documentation

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


