# Payee Groups

This set of APIs enables a consumer to:

-   [Get a list of payee groups](#get-payee-groups)

-   [Add payee group](#add-a-payee-group)

-   [Update payee group](#update-a-payee-group)

-   [Delete payee group](#delete-a-payee-group)

## Get Payee Groups

The PayeeGroups Get API enables a consumer to return a list of payee
group names set up by the user. Payees assigned to the group(s) will
also be returned.

### Method and Endpoint

| GET | /api/v1/me/PayeeGroups |
|-----|------------------------|

### Response

| Parameter | Req | Data Type                                     | Description                                                    |
|---------|----|-----------|-------------------------------------------------|
| data      | Req | [PayeeGroupListOutput](?path=docs/apiDomains/complexObjects.md&branch=develop#payeegrouplistoutput) | List of payee groups. If there are no groups, 204 is returned. |
| result    | Req | [ResultType](?path=docs/apiDomains/complexObjects.md&branch=develop#resulttype)                     | Result information.                                            |

### Sample API Usage

#### Response
```json
{
"data": {
"payeeGroups": [
{
"isVisible": true,
"name": "TestGroup1",
"payeeGroupInfo": [
"payeeUri": "/api/v1/me/payees/3f998588c1784b068e41676de11a36ca",
"payeeUri": "/api/v1/me/payees/f0c603de83af40518fac4b9dfae433f2"
],
"self":
"/api/v1/me/payeeGroups/df1873d54d6a4f14b43ac84b36fb3556",
"id": "df1873d54d6a4f14b43ac84b36fb3556"
}
]
},
"result": {
"success": true,
"resultInfo": []
}
}
```
## Add a Payee Group

The Payee Groups Post API enables a consumer to add a new payee group.
The consumer can assign payees to the group and can specify whether they
want to display the group or not.

### Method and Endpoint

| POST | /api/v1/me/PayeeGroups |
|------|------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| isVisible | Req | body | boolean | Indicates whether whether the payee group will be displayed. <br> True - Payee group will be displayed. This is the default. <br> False - Payee group will not be displayed. |
| name | Req | body | string | Unique name for the payee group to be added. Length: 1-32 <br> Pattern: ^\(\x26\(?\!\x23\)\|\[\x20-\x25\x27\x2A-\x2F\x30-\x39\x3A\x3B\x3F\x40-\x5A\x5C\x5F\x61-\x7E\]\)+$ <br> Note: Cannot contain any of the following cross-site scripting special characters: <>=() and cannot contain the characters 0x26 (&) and 0x23 (#) in the sequence &# |
| payeeGroupInfo | Opt | body | Array of [PayeeGroupPayeeItem](?path=docs/apiDomains/complexObjects.md&branch=develop#payeegrouppayeeitem) | A list of payees to assign to the group. Any payees found to be invalid will not be added to the group. A warning will be returned in this case. |

### Response

| Parameter | Req  | Data Type                             | Description                                                      |
|------------|-----|----------|-----------------------------------------------|
| data      | Cond | [PayeeGroupOutput](?path=docs/apiDomains/complexObjects.md&branch=develop#payeegroupoutput) | Response data. Condition: Required when payeeGroupInfo provided. |
| result    | Req  | [ResultType](?path=docs/apiDomains/complexObjects.md&branch=develop#resulttype)             | Result information.                                              |

### Sample API Usage

#### Request Body
```json
{
"isVisible": true,
"name": "NewGroup",
"payeeGroupInfo": [
{
"payeeUri": "/api/v1/me/payees/3d30666ac2104251913349825d45d1f8",
"listItemId": 1
}
]
}
```
#### Response
```json
{
"data": {
"payeeResults": [
{
"listItemId": 1,
"result": {
"success": true,
"resultInfo": []
}
}
],
"self":
"/api/v1/me/payeeGroups/67cd0da939924accbd233220f24fd9f0",
"id": "67cd0da939924accbd233220f24fd9f0"
},
"result": {
"success": true,
"resultInfo": []
}
}
```
## Update a Payee Group

The Payee Groups Put API enables a consumer to modify a payee group
name, specify whether the payee group is displayed, assign payees to a
payee group, or remove payees from a payee group.

Note

To move a payee from one group to another, the consumer must first
remove the payee from its current group and then make another call to
add the payee to the appropriate group.

### Method and Endpoint

| PUT | /api/v1/me/PayeeGroups/{id} |
|-----|------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| id | Req | path | string | Identifier for the payee group. |
| isVisible | Req | body | boolean | Indicates whether whether the payee group will be displayed. <br> True - Payee group will be displayed. This is the default. <br> False - Payee group will not be displayed. |
| name | Req | body | string | Unique name for the payee group. Length: 1-32 <br> Pattern: ^\(\x26\(?\!\x23\)\|\[\x20-\x25\x27\x2A-\x2F\x30-\x39\x3A\x3B\x3F\x40-\x5A\x5C\x5F\x61-\x7E\]\)+$ <br> Note: Cannot contain any of the following cross-site scripting special characters: <>=() and cannot contain the characters 0x26 (&) and 0x23 (#) in the sequence &# |
| payeeGroupInfo | Opt | body | Array of [PayeeGroupPayeeItem](?path=docs/apiDomains/complexObjects.md&branch=develop#payeegrouppayeeitem) | A list of payees to assign to the group. Any payees to be added to or retained in the group should be provided in this array. <br> Note that the following actions will remove **all** payees from a group: <br> - Submitting an empty array <br> - Submitting an array specifying “null” <br> - Not submitting an array. <br> Any payees found to be invalid will not be added to the group. A warning will be returned in this case. | 

### Response

| Parameter | Req  | Data Type                                   | Description                                                                                                                                  |
|------------|-----|-----------|----------------------------------------------|
| data      | Cond | [PutPayeeGroupOutput](?path=docs/apiDomains/complexObjects.md&branch=develop#putpayeegroupoutput) | Response data. Condition: Required when payeeGroupInfo provided.                                                                             |
| result    | Cond | [ResultType](?path=docs/apiDomains/complexObjects.md&branch=develop#resulttype)                   | Result associated with the request. Condition: Only returned when the request fails. No content returned for success (HTTP status code 204). |

### Sample API Usage

#### Request URL

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/payeeGroups/3fc7a190e69d4fe3ac8e2b56dc75b62e |
|------------------------------------------------------------------------|

#### Request Body
```json
{
"isVisible": false,
"name": "Utilities",
"payeeGroupInfo": [
{
"payeeUri": "/api/v1/me/payees/57235cfe940a481d8ceb5a9157fdbbe2",
"listItemId": 1
},
{
"payeeUri": "/api/v1/me/payees/3e0133c6d6414dc083f00526a6de2a36",
"listItemId": 2
}
],
"self":
"/api/v1/me/payeeGroups/3fc7a190e69d4fe3ac8e2b56dc75b62e",
"id": "3fc7a190e69d4fe3ac8e2b56dc75b62e"
}
```
#### Response
```json
{
"data": {
"payeeResults": [
{
"listItemId": 1,
"result": {
"success": true,
"resultInfo": []
}
},
{
"listItemId": 2,
"result": {
"success": true,
"resultInfo": []
}
}
]
},
"result": {
"success": true,
"resultInfo": []
}
}
```
## Delete a Payee Group

This API enables a consumer to delete an existing payee group name. Any
payees previously assigned to that group will become unassigned (no
longer assigned to any particular group).

### Method and Endpoint

| DELETE | /api/v1/me/PayeeGroups/{id}|
|--------|----------------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description                                   |
|------------|----|-------|-------|-------------------------------------------|
| id        | Req | path       | string    | Identifier for the payee group to be deleted. |

### Response

| Parameter | Req  | Data Type                 | Description                                                                                                                                  |
|------------|------|-------|-----------------------------------------------|
| result    | Cond | [ResultType](?path=docs/apiDomains/complexObjects.md&branch=develop#resulttype) | Result associated with the request. Condition: Only returned when the request fails. No content returned for success (HTTP status code 204). |

### Sample API Usage

#### Request 

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/payeeGroups/d882685743114c5cad7f3cc9e7087506 |
|------------------------------------------------------------------------|

#### Response

| 204 No Content |
|----------------|