# ToDos

A ToDo is any item (for example, an unpaid bill) that requires an action
from the consumer.

This set of APIs allows the calling application to:

-   [Get to-do items for a consumer](#get-todo-items-list)

-   [Get a single to-do item for a consumer](#get-todo-item-single)

-   [Dismiss a to-do item for a consumer](#dismiss-a-todo-item)

## Get ToDo Items (List)

The ToDos Get API enables the retrieval of ToDo items for a consumer
that they can take action on.

**When to use**: Use this API if you want to present a list of to-do
items and allow the consumer to take action on those items.

### Method and Endpoint

| GET | /api/v1/me/ToDos |
|-----|------------------|

### Response

| Parameter | Req  | Data Type                 | Description                                                                                                                                         |
|------------|-----|----------|-----------------------------------------------|
| data      | Req  | Array of [ToDo](?path=docs/apiDomains/complexObjects.md&branch=develop#todo)    | ToDo items. There is an empty array if there is no data to return.                                                                                  |
| result    | Cond | [ResultType](?path=docs/apiDomains/complexObjects.md&branch=develop#resulttype) | Result associated with the request. Condition: Only returned when the request fails. No result content returned for success (HTTP status code 200). |

### Sample API Usage

#### Response
```json
{
"toDoType": "UnpaidBill",
"toDoDetailUri":
"/api/v1/me/bills/5372f785-0bbf-47ec-8c08-7952670fcb84",
"payeeUri":
"/api/v1/me/payees/3f4d1e3f-b08e-4f45-9d2b-7c2281e8ccdb",
"actionDate": "2018-11-26",
"amount": 100,
"maskedAccountNumber": "*******1586",
"description": "AT&amp;T",
"self": "/api/v1/me/todos/5372f785-0bbf-47ec-8c08-7952670fcb84",
"id": "5372f785-0bbf-47ec-8c08-7952670fcb84"
}
```
## Get ToDo Item (Single)

The ToDos Get API returns a specific ToDo item for a consumer.

### Method and Endpoint

| GET | /api/v1/me/ToDos/{id} |
|-----|-----------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description                   |
|-----------|-----|------------|-----------|-------------------------------|
| id        | Req | path       | string    | Identifier for the ToDo item. |

### Response

| Parameter | Req | Data Type                 | Description                      |
|---------|----|-----------|-------------------------------------------------|
| data      | Req | [ToDo](?path=docs/apiDomains/complexObjects.md&branch=develop#todo)             | Information about the ToDo item. |
| result    | Req | [ResultType](?path=docs/apiDomains/complexObjects.md&branch=develop#resulttype) | Result information.              |

### Sample API Usage

#### Request URL

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/todos/ea4c60e2f3274a69a9078f5088e0f092 |
|------------------------------------------------------------------------|

#### Response
```json
{
"data": {
"toDoType": "UnpaidBill",
"toDoDetailUri":
"/api/v1/me/bills/ea4c60e2f3274a69a9078f5088e0f092",
"payeeUri": "/api/v1/me/payees/3c321d78a62e40cb908846d2e7d85528",
"actionDate": "2019-09-23",
"amount": 64.69,
"maskedAccountNumber": "*******1248",
"description": "American Eagle",
"self": "/api/v1/me/todos/ea4c60e2f3274a69a9078f5088e0f092",
"id": "ea4c60e2f3274a69a9078f5088e0f092"
},
"result": {
"success": true,
"resultInfo": [
]
}
}
```
## Dismiss a ToDo Item

This API is used when the consumer wants to dismiss a ToDo from the ToDo
list. When a ToDo is dismissed, the source item is removed from the list
as well.

### Method and Endpoint

| PATCH | /api/v1/me/ToDos/{id}/dismiss|
|--------|-----------------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description                   |
|-----------|-----|------------|-----------|-------------------------------|
| id        | Req | path       | string    | Identifier for the ToDo item. |

### Response

| Parameter | Req  | Data Type                 | Description                                                                                                                  |
|------------|-----|----------|-----------------------------------------------|
| result    | Cond | [ResultType](?path=docs/apiDomains/complexObjects.md&branch=develop#resulttype) | Result information. Condition: Only returned when the request fails. No content returned for success (HTTP status code 204). |

### Sample API Usage

#### Request URL

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/ToDos/1c1acf920e48487abada730cc8851640/dismiss |
|------------------------------------------------------------------------|

#### Response

| 204 No Content |
|----------------|