# Reminders

This set of APIs allows the calling application to:

-   [Get a list of bill payment reminder
    models](#get-reminder-models-list)

-   [Get a single reminder model](#get-reminder-model-single)

-   [Add a reminder model](#add-a-reminder-model)

-   [Update a reminder model](#update-a-reminder-model)

-   [Delete a reminder model](#delete-a-reminder-model)

## Get Reminder Models (List)

The Reminders Get API returns a list of a consumer’s bill payment
reminder models. Each model contains the parameters that are used to
generate individual reminder messages. For example, a **reminder model**
might indicate that a **reminder message** (ToDo) is to be generated for
a payment typically due on the 15th of the month, for $15.00, monthly,
and that the consumer wants to be reminded 10 days prior to the 15th.

**When to use**: Use this API if you want to present the ability to a
consumer to set up and manage bill payment reminders. This is typically
called as part of displaying information for a specific payee to present
any bill payment reminder instructions that have previously been set up
for that payee by that consumer.

### Method and Endpoint


| GET | /api/v1/me/Reminders |
|-----|----------------------|

### Response

| Parameter | Req  | Data Type                     | Description                                                                                                                                         |
|------------|-----|----------|-----------------------------------------------|
| data      | Req  | [ReminderList](#reminderlist) | Reminder models. There is an empty array if there is no data to return.                                                                             |
| result    | Cond | [ResultType](#resulttype)     | Result associated with the request. Condition: Only returned when the request fails. No result content returned for success (HTTP status code 200). |

### Sample API Usage

#### Response
```json
{
"data": {
"reminders": [
{
"alertAtLeadTimeBeforePaymentDueDate": true,
"alertIfNoBillPaymentMadeByDueDate": true,
"alertWhenBillPaymentProcesses": true,
"amount": 10.0,
"firstReminderDate": "2022-12-13",
"frequency": "Annually",
"leadTime": "Lead28Days",
"currentDueDate": "2022-12-13",
"payeeUri": "/api/v1/me/payees/dcd2c702d1b84c7ead5497516fbadc0b",
"self": "/api/v1/me/reminders/c458cad50a8a4cec98d8f5e816f4ba99",
"id": "c458cad50a8a4cec98d8f5e816f4ba99"
}
]
},
"result": {
"success": true,
"resultInfo": []
}
}
```
## Get Reminder Model (Single)

The Reminders Get API returns a specific bill payment reminder model for
a consumer.

### Method and Endpoint


| GET | /api/v1/me/Reminders/{id} |
|-----|---------------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description                        |
|-----------|-----|------------|-----------|------------------------------------|
| id        | Req | path       | string    | Identifier for the reminder model. |

### Response

| Parameter | Req | Data Type                 | Description                           |
|---------|----|-----------|-------------------------------------------------|
| data      | Req | [Reminder](#reminder)     | Information about the reminder model. |
| result    | Req | [ResultType](#resulttype) | Result information.                   |

### Sample API Usage

#### Request URL

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/Reminders/21343a099774452e87fbe866cf1eef01 |
|------------------------------------------------------------------------|

#### Response
```json
{
"data": {
"alertAtLeadTimeBeforePaymentDueDate": true,
"alertIfNoBillPaymentMadeByDueDate": true,
"alertWhenBillPaymentProcesses": true,
"amount": 10.0,
"firstReminderDate": "2022-10-30",
"frequency": "Weekly",
"leadTime": "Lead03Days",
"currentDueDate": "2022-10-30",
"payeeUri": "/api/v1/me/payees/186dd42d11f04ba1adef2806a10128b5",
"self": "/api/v1/me/reminders/21343a099774452e87fbe866cf1eef01",
"id": "21343a099774452e87fbe866cf1eef01"
},
"result": {
"success": true,
"resultInfo": []
}
}
```
## Add a Reminder Model

The Reminders Post API enables a consumer to set up a new bill payment
reminder model for one of their payees. Each model contains the
parameters that are used to generate individual reminder messages. For
example, a **reminder model** might indicate that a **reminder**
**message** (ToDo) is to be generated for a payment typically due on the
15th of the month, for $15.00, monthly, and that the consumer wants to
be reminded 10 days prior to the 15th.

**When to use:** Use this API if you want to present the ability to a
consumer to set up and manage bill payment reminder instructions. This
API is typically called as part of displaying information for a specific
payee and allows the consumer to set up a new bill payment reminder for
that payee.

Examples:

• Viewing a payee – Use this operation to enable a new bill payment
reminder for that payee.

• Viewing existing reminders – Use this operation to allow a consumer to
set up new bill payment reminders for other payees.

### Method and Endpoint


| POST | /api/v1/me/Reminders |
|------|----------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| alertAtLeadTimeBeforePaymentDueDate | Req | body | boolean | Indicates if the consumer is reminded at lead time (see leadTime) before a payment is due.<br> Valid values: <br> true – Remind the consumer at lead time before a payment is due. <br> false – Do not remind the consumer at lead time before a payment is due. | 
| alertIfNoBillPaymentMadeByDueDate | Req | body | boolean | Indicates if the consumer is reminded if no payment has been made by the due date. <br> Valid values: <br> true – Remind the consumer that a payment is due. <br> false – Do not remind the consumer that a payment is due. |
| alertWhenBillPaymentProcesses | Req | body | boolean | Indicates if the consumer is notified when a payment is processed. <br> Valid values: <br> true – Notify the consumer when a payment has been marked as processed. <br> false – Do not notify the consumer when a payment has been marked as processed. |
| amount | Req | body | number | The payment amount associated with this reminder. <br> Pattern: ^\\d+(\\.\\d{1,2})?$ |
| firstReminderDate | Req | body | string | Date for the first bill payment reminder in yyyy-MM-dd format. The first date of a reminder should be greater than today plus the number of lead days requested. |
| frequency | Req | body | string | The frequency of how often the reminder is automatically created. Valid values: <br> Weekly (should not be used in combination with leadTime of 10, 14, 21, or 28 days) <br> Every2Weeks (should not be used in combination with leadTime of 14, 21, or 28 days) <br> TwiceAMonth (should not be used in combination with leadTime of 21 or 28 days) <br> Every4Weeks <br> Monthly <br> Every2Months <br> Every3Months <br> Every4Months <br> Every6Months <br> Annually | 
| leadTime | Req | body | string | Number of days (excluding any risk management calculations) before the due date that the reminder will be created. Valid values: <br> Lead03Days <br> Lead05Days <br> Lead10Days (should not be used in combination with frequency Weekly) <br> Lead14Days (should not be used in combination with frequency Weekly or Every2Weeks) <br> Lead21Days (should not be used in combination with frequency Weekly, Every2Weeks, or TwiceAMonth) <br> Lead28Days (should not be used in combination with frequency Weekly, Every2Weeks, or TwiceAMonth) |
| payeeUri | Req | body | string | URI for the payee. This matches the payee URI returned in GET Payees. |

### Response

| Parameter | Req  | Data Type                 | Description                                                        |
|------------|-----|--------|------------------------------------------------|
| data      | Cond | [BaseModel](#basemodel)   | Response data. Condition: Always returned for successful response. |
| result    | Req  | [ResultType](#resulttype) | Result information.                                                |

### Sample API Usage

#### Request Body
```json
{
"alertAtLeadTimeBeforePaymentDueDate": true,
"alertIfNoBillPaymentMadeByDueDate": true,
"alertWhenBillPaymentProcesses": true,
"amount": 10.0,
"firstReminderDate": "2022-11-27",
"frequency": "Weekly",
"leadTime": "Lead03Days",
"payeeUri": "/api/v1/me/payees/bc3e8eab2ccb4d43ad5214ea19463864"
}
```
#### Response
```json
{
"data": {
"self": "/api/v1/me/reminders/07b04270363d4c58beb3040ac3505bcd",
"id": "07b04270363d4c58beb3040ac3505bcd"
},
"result": {
"success": true,
"resultInfo": []
}
}
```
## Update a Reminder Model

The Reminders Patch API enables a consumer to modify an existing bill
payment reminder model for one of their payees. Each model contains the
parameters that are used to generate individual reminder messages. For
example, a **reminder model** might indicate that a **reminder message**
(ToDo) is to be generated for a payment typically due on the 15th of the
month, for $15.00, monthly, and that the consumer wants to be reminded
10 days prior to the 15th.

**When to use:** Use this API if you want to present the ability to a
consumer to set up and manage bill payment reminder instructions. This
operation is typically called as part of displaying information for a
specific payee and allows the consumer to modify an existing bill
payment reminder for that payee.

Examples:

• Viewing a payee – Use this operation to enable modifying an existing
bill payment reminder for that payee.

• Viewing existing reminders – Use this operation to allow a consumer to
modify an existing payment reminder

### Method and Endpoint


| PATCH | /api/v1/me/Reminders/{id}|
|--------|--------------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description | 
|-----------|-----|------------|-----------|-------------|
| id | Req | path | string | Identifier for the reminder model. |
| alertAtLeadTimeBeforePaymentDueDate | Opt | body | boolean | Indicates if the user is reminded at lead time (see LeadTime) before a payment is due. <br> Valid values: <br> true – Remind the user at lead time before a payment is due. <br> false – Do not remind the user at lead time before a payment is due. |
| alertIfNoBillPaymentMadeByDueDate | Opt | body | boolean | Indicates if the user is reminded if no payment has been made by the due date. <br> Valid values: <br> true – Remind the user that a payment is due. <br> false – Do not remind the user that a payment is due. |
| alertWhenBillPaymentProcesses | Opt | body | boolean | Indicates if the user is notified when a payment is processed. <br> Valid values: <br> true – Notify the user when a payment has been marked as processed. <br> false – Do not notify the user when a payment has been marked as processed. |
| amount | Opt | body | number | The payment amount associated with this reminder. <br> Pattern: ^\\d+(\\.\\d{1,2})?$ |
| firstReminderDate | Opt | body | string | First date for the bill payment reminder in yyyy-MM-dd format. The first date of a reminder should be greater than today plus the number of lead days requested. |
| frequency | Opt | body | string | The frequency of how often the reminder is automatically created. Valid values: <br> Weekly (should not be used in combination with leadTime of 10, 14, 21, or 28 days) <br> Every2Weeks (should not be used in combination with leadTime of 14, 21, or 28 days) <br> TwiceAMonth (should not be used in combination with leadTime of 21 or 28 days) <br> Every4Weeks <br> Monthly <br> Every2Months <br> Every3Months <br> Every4Months <br> Every6Months <br> Annually |
| leadTime | Opt | body | string | Number of days (excluding any risk management calculations) before the due date that the reminder will be created. Valid values: <br> Lead03Days <br> Lead05Days <br> Lead10Days (should not be used in combination with frequency Weekly) <br> Lead14Days (should not be used in combination with frequency Weekly or Every2Weeks) <br> Lead21Days (should not be used in combination with frequency Weekly, Every2Weeks, or TwiceAMonth) <br> Lead28Days (should not be used in combination with frequency Weekly, Every2Weeks, or TwiceAMonth) |

### Response

| Parameter | Req  | Data Type                 | Description                                                                                                                                  |
|-------------|-----|-------|-------------------------------------------------|
| result    | Cond | [ResultType](#resulttype) | Result associated with the request. Condition: Only returned when the request fails. No content returned for success (HTTP status code 204). |

### Sample API Usage

#### Request URL

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/Reminders/e5b32963fe774f6bb436b9dc0529c4ee |
|------------------------------------------------------------------------|

#### Request Body
```json
{
"alertAtLeadTimeBeforePaymentDueDate": false,
"alertIfNoBillPaymentMadeByDueDate": false,
"alertWhenBillPaymentProcesses": false,
"amount": 12.21,
"firstReminderDate": "2023-01-17",
"frequency": "Monthly",
"leadTime": "Lead05Days"
}
```
#### Response

| 204 No Content |
|----------------|

## Delete a Reminder Model

The Reminders Delete API enables a consumer to cancel an existing bill
payment reminder model for one of their payees.

**When to use:** Use this API if you want to present the ability to a
consumer to manage bill payment reminder instructions. This operation is
typically called as part of displaying information for a specific payee
and allows the consumer to stop receiving bill payment reminders set up
for that payee.

Examples:

• Viewing a payee – Use this operation to enable canceling an existing
bill payment reminder for that payee.

• Viewing existing reminders – Use this operation to allow a consumer to
cancel an existing payment reminder.

### Method and Endpoint

| DELETE | /api/v1/me/Reminders/{id}|
|--------|--------------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description                                      |
|------------|----|-------|-------|-------------------------------------------|
| id        | Req | path       | string    | Identifier for the reminder model to be deleted. |

### Response

| Parameter | Req  | Data Type                 | Description                                                                                                                                  |
|------------|------|-------|-----------------------------------------------|
| result    | Cond | [ResultType](#resulttype) | Result associated with the request. Condition: Only returned when the request fails. No content returned for success (HTTP status code 204). |

### Sample API Usage

#### Request 

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/Reminders/356685037c1e421fae79aad446c2895f |
|------------------------------------------------------------------------|

#### Response

| 204 No Content |
|----------------|