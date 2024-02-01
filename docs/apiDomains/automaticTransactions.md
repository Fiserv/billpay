# AutomaticTransactions

This set of APIs allows you to:

-   [Get a list of automatic transactions for a
    consumer](#get-automatic-transactions-list)

-   [Get a specific automatic transaction for a
    consumer](#get-automatic-transaction-single)

-   [Set up a new recurring model for one of a consumer’s
    payees](#set-up-an-automatic-transaction)

-   [Update an automatic transaction](#update-an-automatic-transaction)

-   [Cancel an automatic transaction](#delete-an-automatic-transaction)

## Get Automatic Transactions (List)

The Automatic Transactions Get API returns the list of recurring payment
models and automatic payment models that have been set up for a specific
consumer.

### Method and Endpoint

| GET | /api/v1/me/automaticTransactions|
|-----|----------------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description | 
|-----------|-----|------------|-----------|-------------|
| pageSize | Opt | query | integer | Specifies the number of items per page. |
| pageNumber | Opt | query | integer | Specifies the page number from which results should be returned. | 
| returnInactiveAutomaticTransactions | Opt | query | boolean | Indicates whether to return inactive automatic transactions in the response. <br> True – Return inactive automatic transactions <br> False – Do not return inactive automatic transactions. This is the default. |
| destinationUri | Opt | query | string | The destination of the automatic transaction. This is a URI for a payee. | 

### Response

| Parameter | Req | Data Type | Description | 
|-----------|-----|-----------|-------------|
| automaticTransactions | Req | Array of [AutomaticTransactionOutputDetail](#automatictransactionoutputdetail) | List of found automatic transactions. There is an empty array if there is no data to return. | 
| result | Cond | [ResultType](#resulttype) | Result information. Condition: Only returned when the request fails. No content returned for success (HTTP status code 200). |

### Sample API Usage

#### Response
```json
{
"automaticTransactions": [
{
"modifiableFields": [
"fundingAccountUri",
"amount",
"endDate",
"endTransactionAmount",
"frequency",
"initiationDate",
"maximumTransactionCount",
"memo",
"transactionScheduledAlert",
"transactionSentAlert",
"recurringScheduleExpireAlert"
],
"id": "b3fea21d83d04051a6241e9284812ee6",
"self":
"/api/v1/me/automatictransactions/b3fea21d83d04051a6241e9284812ee6",
"status": "Active",
"fundingAccountUri":
"/api/v1/me/bankAccounts/8af5bc0b652e41ce9352f24b1ab4b553",
"destinationUri":
"/api/v1/me/payees/a9667fb77a3e4dad99f44250267a9aed",
"billTransactionSchedule": null,
"recurringTransactionSchedule": {
"amount": 15.0,
"endDate": "2023-09-19",
"frequency": "Every3Months",
"initialTransactionAmount": 15.0,
"initiationDate": "2022-09-19",
"nextTransactionDate": "2022-09-19",
"maximumTransactionCount": 5,
"remainingTransactionCount": 4,
"memo": "",
"transactionScheduledAlert": true,
"transactionSentAlert": true,
"recurringScheduleExpireAlert": true,
"duration": "XNumberOfPayments",
"nextTransactionGenerationDate": "2022-12-19"
}
},
{
"modifiableFields": [
"fundingAccountUri",
"amount",
"endDate",
"endTransactionAmount",
"frequency",
"initiationDate",
"maximumTransactionCount",
"memo",
"transactionScheduledAlert",
"transactionSentAlert",
"recurringScheduleExpireAlert"
],
"id": "e37ea97a92a341f2bd844e545f087f21",
"self":
"/api/v1/me/automatictransactions/e37ea97a92a341f2bd844e545f087f21",
"status": "Active",
"fundingAccountUri":
"/api/v1/me/bankAccounts/8af5bc0b652e41ce9352f24b1ab4b553",
"destinationUri":
"/api/v1/me/payees/a9667fb77a3e4dad99f44250267a9aed",
"billTransactionSchedule": null,
"recurringTransactionSchedule": {
"amount": 23.33,
"endDate": "2022-09-26",
"frequency": "Weekly",
"initialTransactionAmount": 23.33,
"initiationDate": "2022-09-19",
"nextTransactionDate": "2022-09-19",
"maximumTransactionCount": 2,
"remainingTransactionCount": 1,
"memo": "",
"transactionScheduledAlert": true,
"transactionSentAlert": true,
"recurringScheduleExpireAlert": true,
"duration": "XNumberOfPayments",
"nextTransactionGenerationDate": "2022-09-26"
}
}
]
}
```
## Get Automatic Transaction (Single)

The Automatic Transactions Get API can be invoked with a specific
automatic transaction identifier to get the respective automatic payment
plan.

### Method and Endpoint

| GET | /api/v1/me/automaticTransactions/{automaticTransactionId}|
|-----|----------------------------|

### Request

| Parameter              | Req | Param Type | Data Type | Description                               |
|-----------|-----|-------|-------|--------------------------------------------|
| automaticTransactionId | Req | path       | string    | Identifier for the automatic transaction. |

### Response

| Parameter | Req | Data Type | Description| 
|-----------|-----|-----------|------------|
| automaticTransactions | Req | [AutomaticTransactionOutputDetail](#automatictransactionoutputdetail) | Details for the automatic payment plan. | 
| result | Cond | [ResultType](#resulttype) | Result information. Condition: Only returned when the request fails. No content returned for success (HTTP status code 200). |

### Sample API Usage

#### Response
```json
{
"modifiableFields": [
"fundingAccountUri",
"amount",
"endDate",
"endTransactionAmount",
"frequency",
"initiationDate",
"maximumTransactionCount",
"memo",
"transactionScheduledAlert",
"transactionSentAlert",
"recurringScheduleExpireAlert"
],
"id": "b3fea21d83d04051a6241e9284812ee6",
"self":
"/api/v1/me/automatictransactions/b3fea21d83d04051a6241e9284812ee6",
"status": "Active",
"fundingAccountUri":
"/api/v1/me/bankAccounts/8af5bc0b652e41ce9352f24b1ab4b553",
"destinationUri":
"/api/v1/me/payees/a9667fb77a3e4dad99f44250267a9aed",
"billTransactionSchedule": null,
"recurringTransactionSchedule": {
"amount": 15.0,
"endDate": "2023-09-19",
"frequency": "Every3Months",
"initialTransactionAmount": 15.0,
"initiationDate": "2022-09-19",
"nextTransactionDate": "2022-09-19",
"maximumTransactionCount": 5,
"remainingTransactionCount": 4,
"memo": "",
"transactionScheduledAlert": true,
"transactionSentAlert": true,
"recurringScheduleExpireAlert": true,
"duration": "XNumberOfPayments",
"nextTransactionGenerationDate": "2022-12-19"
}
}
```
## Set Up an Automatic Transaction

The Automatic Transactions Post API enables a consumer to set up a new
recurring model for one of that consumer’s payees. A recurring model is
an automatic payment instruction set up by the consumer to pay a
specified amount to one of the consumer's payees on a regular basis. The
consumer specifies the starting date and frequency of payments.

### Method and Endpoint

| POST | /api/v1/me/automaticTransactions |
|------|------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description | 
|-----------|-----|------------|-----------|-------------|
| fundingAccountUri | Req | body | string | The source funding account URI for the automatic transaction. |
| destinationUri | Req | body | string | The destination of the automatic transaction. This is a URI for a payee. |
| billTransactionSchedule | Cond | body | [BillTransactionSchedule](#billtransactionschedule) | BillTransactionSchedule defines a transaction schedule for an e-bill automatic payment. <br> Condition: Either billTransactionSchedule or recurringTransactionSchedule is required. |
| recurringTransactionSchedule | Cond | body | [RecurringTransactionSchedule](#recurringtransactionschedule) | RecurringTransactionSchedule defines a transaction schedule for a payee. <br> Condition: Either billTransactionSchedule or recurringTransactionSchedule is required. | 
| cavv | Opt | body | string | Placeholder for 3-D Secure. Cardholder Authentication Verification Value. Used if the transaction is funded by a card account and the institution is participating in 3-D Secure. <br> Only applies to the first transaction generated from a recurring transaction schedule (if the transaction is within 90 days). | 

### Response

| Parameter | Req  | Data Type                 | Description                                                        |
|-------------|-----|-------|-------------------------------------------------|
| data      | Cond | [BaseModel](#basemodel)   | Response data. Condition: Always returned for successful response. |
| result    | Req  | [ResultType](#resulttype) | Result information.                                                |

### Sample API Usage

#### Request Body
```json
{
"fundingAccountUri":
"/api/v1/me/bankAccounts/8af5bc0b652e41ce9352f24b1ab4b553",
"destinationUri":
"/api/v1/me/payees/a9667fb77a3e4dad99f44250267a9aed",
"billTransactionSchedule": null,
"recurringTransactionSchedule": {
"amount": 15.0,
"endDate": null,
"endTransactionAmount": null,
"frequency": "Weekly",
"initialTransactionAmount": null,
"initiationDate": "2018-06-29",
"maximumTransactionCount": 15,
"memo": null,
"transactionScheduledAlert": true,
"transactionSentAlert": true,
"recurringScheduleExpireAlert": true
}
}
```
#### Response
```json
{
"data": {
"self":
"/api/v1/me/automatictransactions/b3fea21d83d04051a6241e9284812ee6",
"id": "b3fea21d83d04051a6241e9284812ee6"
},
"result": {
"success": true,
"resultInfo": [
{
"resultCategory": "Info",
"code": "0000",
"field": null,
"fieldPath": null,
"listItemId": null,
"description": "Success."
}
]
}
}
```
## Update an Automatic Transaction

The Automatic Transactions Patch API enables changes to be made to an
automatic transaction that has already been set up.

### Method and Endpoint

| PATCH | /api/v1/me/automaticTransactions/{automaticTransactionId}|
|--------|--------------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| automaticTransactionId | Req | path | string | Identifier for the automatic transaction. |
| fundingAccountUri | Opt | body | string | The source funding account URI for the automatic transaction. |
| billTransactionSchedule | Opt | body | [BillTransactionSchedulePatch](#billtransactionschedulepatch) | BillTransactionSchedule defines a transaction schedule for an e-bill automatic payment. |
| recurringTransactionSchedule | Opt | body | [RecurringTransactionSchedulePatch](#recurringtransactionschedulepatch) | RecurringTransactionSchedule defines a transaction schedule for a payee. |
| cavv | Opt | body | string | Placeholder for 3-D Secure. Cardholder Authentication Verification Value. Used if the transaction is funded by a card account and the institution is participating in 3-D Secure. <br> Only applies to the first transaction generated from a recurring transaction schedule (if the transaction is within 90 days). |

### Response

| Parameter | Req  | Data Type                                                    | Description                                                                                                                                                                        |
|------------|-----|------------|--------------------------------------------|
| data      | Cond | Array of [TransactionsNotModified](#transactionsnotmodified) | List of transactions that could not be modified. Condition: A spawned payment could not be modified. Only applies to transactions generated from a recurring transaction schedule. |
| result    | Cond | [ResultType](#resulttype)                                    | Result information. Condition: Only returned when the request fails. No content returned for success (HTTP status code 204).                                                       |

### Sample API Usage

#### Request URL

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/automaticTransactions/e149c86d52554704b1d000fc4141ae08 |
|------------------------------------------------------------------------|

#### Request Body
```json
{
"fundingAccountUri":
"/api/v1/me/bankAccounts/545a5e3d624743b09c4933a156bad5af"
}
```
#### Response

| 204 No Content |
|----------------|

## Delete an Automatic Transaction

The Automatic Transactions Delete API enables an automatic transaction
that has been set up to be canceled.

### Method and Endpoint

| DELETE | /api/v1/me/automaticTransactions/{automaticTransactionId}|
|--------|----------------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| automaticTransactionId | Req | path | string | Identifier for the automatic transaction. |
| cancelPendingTransactions | Opt | query | boolean | Indicates if pending transactions should be canceled. <br> true – Yes, cancel pending payments. <br> false – No, do not cancel pending payments. This is the default. <br> The option to cancel pending transactions only applies to automatic transactions that have a RecurringTransactionSchedule. This does not apply to e-bill automatic payments (BillTransactionSchedule). |

### Response

| Parameter | Req  | Data Type                                                                    | Description                                                                                                                                                                                                                     |
|-------------|-----|---------------|----------------------------------------|
| data      | Cond | Array of [PendingTransactionsNotCancelled](#pendingtransactionsnotcancelled) | List of transactions that could not be canceled. Condition: cancelPendingTransactions is true in the request and a payment could not be canceled. Only applies to transactions generated from a recurring transaction schedule. |
| result    | Cond | [ResultType](#resulttype)                                                    | Result associated with the request. Condition: Only returned when the request fails. No content returned for success (HTTP status code 204).                                                                                    |

### Sample API Usage

#### Request URL

https://api-checkfreenext-cert.fiservapps.com/api/v1/me/automaticTransactions/e149c86d52554704b1d000fc4141ae08?
cancelPendingTransactions=true

#### Response

| 204 No Content |
|----------------|

#