# Transactions

This set of APIs allows you to:

-   [Get a list of transactions](#get-transactions)

-   [Create transactions](#create-a-transaction)

-   [Update a transaction](#update-a-transaction)

## Get Transactions

The Transactions Get API enables the retrieval of transactions for a
specific consumer. Fiserv validates each individual transaction and
creates a confirmation number for each one. Fiserv stores the
transactions and processes them on the requested due date.

### Method and Endpoint

| GET | /api/v1/me/Transactions|
|-----|----------------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| startDate | Opt | query | string <br> &lt;date-time&gt; | The desired start date for the list of transactions. Format: yyyy-MM-dd <br> Default is 6 months in the past. |
| endDate | Opt | query | string <br> &lt;date-time&gt; | The desired end date for the list of transactions. Format: yyyy-MM-dd <br> Default is all transactions scheduled in the future. |
| sort | Opt | query | string | Sort the response based on the given input. These are the eligible values with which to sort: amount, deliveryDate, payeeName, deliveryTrackingNumber, confirmationNumber, debitDate, fee, note, memo, deliveryMethod, status, transactionType <br> A negative sign (-) before the parameter indicates that the response should be sorted in descending order. For example, “sort=-amount” |
| start | Opt | query | integer | Specifies the starting record to be fetched. |
| limit | Opt | query | integer | Specifies the number of records to be fetched. |
| payeeUri | Opt | query | string | The URI to the payee that this transaction is associated with. |
| status | Opt | query | string | The status of the payment transaction. Valid values: Pending, Complete, InProcess, Failed Canceled |
| fundingAccountUri | Opt | query | string | The funding account for the transaction. |
| minAmount | Opt | query | double | The minimum transaction amount. |
| maxAmount | Opt | query | double | The maximum transaction amount. | 

### Response

| Parameter | Req | Data Type                                    | Description                                            |
|----------|-----|----------|------------------------------------------------|
| data      | Req | Array of [TransactionList](#transactionlist) | There is an empty array if there is no data to return. |
| result    | Req | [ResultType](#resulttype)                    | Result information.                                    |

### Sample API Usage

#### Request URL

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/Transactions?startDate=2021-04-20&endDate=2021-05-18 |
|------------------------------------------------------------------------|

#### Response
```json
{
"data": {
"transactions": [
{
"confirmationNumber": "N8R8C-6RY2L",
"deliveryMethod": "Electronic",
"status": "Canceled",
"statusUri":
"/api/v1/me/transactions/de3e5a820597428c9a3793a7113b59a3/status",
"deliveryDate": "2021-05-11",
"cancelUri": null,
"amount": 25.5,
"note": null,
"memo": null,
"billItemUri": null,
"payeeUri": "/api/v1/me/payees/78adc4a26bcd4965926abb2c1a194351",
"automaticTransactionUri": null,
"fundingAccountUri":
"/api/v1/me/bankAccounts/a00c767684124b0baa4f641ccb69dd51",
"fee": 0.0,
"debitDate": "2021-05-11",
"modifiableFields": null,
"transactionType": "Standard",
"deliveryTrackingNumber": null,
"legacyTransactionId": "20210421023206036957",
"self":
"/api/v1/me/transactions/de3e5a820597428c9a3793a7113b59a3",
"id": "de3e5a820597428c9a3793a7113b59a3"
},
{
"confirmationNumber": "MJBT6-49Z7G",
"deliveryMethod": "Paper",
"status": "Complete",
"statusUri":
"/api/v1/me/transactions/36e75562b1aa43c7b12e7613a9670822/status",
"deliveryDate": "2021-05-12",
"cancelUri": null,
"amount": 74.4200,
"note": null,
"memo": null,
"billItemUri": null,
"payeeUri": "/api/v1/me/payees/2feed664f6604d43b97c7f457a07caae",
"automaticTransactionUri": null,
"fundingAccountUri":
"/api/v1/me/bankAccounts/923ea9541fa5421fae03025f7a4bc58c",
"fee": 14.0000,
"debitDate": "2021-05-12",
"modifiableFields": null,
"transactionType": "Overnight",
"deliveryTrackingNumber": "UPS2018012416111900002",
"legacyTransactionId": "20210511151325971343",
"checkNumber": 5003,
"self":
"/api/v1/me/transactions/36e75562b1aa43c7b12e7613a9670822",
"id": "36e75562b1aa43c7b12e7613a9670822"
},
{
"confirmationNumber": "NJB4L-KKFVZ",
"deliveryMethod": "Electronic",
"status": "InProcess",
"statusUri":
"/api/v1/me/transactions/780065a5939440a0bd3a6aeb014342f4/status",
"deliveryDate": "2021-05-11",
"cancelUri": null,
"amount": 17.3800,
"note": null,
"memo": null,
"billItemUri": null,
"payeeUri": "/api/v1/me/payees/bea77791fe2743c5bf2fe30c0c53f768",
"automaticTransactionUri": null,
"fundingAccountUri":
"/api/v1/me/bankAccounts/d1b49b05102943f3b8712c76353e7216",
"fee": 11.1100,
"debitDate": "2021-05-12",
"modifiableFields": null,
"transactionType": "Expedited",
"deliveryTrackingNumber": null,
"legacyTransactionId": "20210511025959583812",
"self":
"/api/v1/me/transactions/780065a5939440a0bd3a6aeb014342f4",
"id": "780065a5939440a0bd3a6aeb014342f4"
},
{
"confirmationNumber": "NJB4J-TQRPJ",
"deliveryMethod": "Paper",
"status": "Pending",
"statusUri":
"/api/v1/me/transactions/64a0db63ba5b4cc3b562cceaf0ef4951/status",
"deliveryDate": "2021-05-12",
"cancelUri":
"/api/v1/me/transactions/64a0db63ba5b4cc3b562cceaf0ef4951/cancel",
"amount": 16.33,
"note": null,
"memo": null,
"billItemUri": null,
"payeeUri": "/api/v1/me/payees/a0518a61e9a44763b6937f37212e581e",
"automaticTransactionUri": null,
"fundingAccountUri":
"/api/v1/me/bankAccounts/710ae3ba5f764d6cbe09797c2399b838",
"fee": 0.0,
"debitDate": "2021-05-12",
"modifiableFields": [
"amount",
"note",
"memo",
"deliveryDate",
"fundingAccountUri"
],
"transactionType": "Standard",
"deliveryTrackingNumber": null,
"legacyTransactionId": "20210420090259280850",
"self":
"/api/v1/me/transactions/64a0db63ba5b4cc3b562cceaf0ef4951",
"id": "64a0db63ba5b4cc3b562cceaf0ef4951"
}
]
}
}
```
## Create a Transaction

The Transactions Post API enables adding new transactions in a single
request to Fiserv for a specific consumer. Fiserv validates each
individual transaction and creates a confirmation number for each one.
Fiserv stores the transaction(s) and processes them on the requested due
date. If any transaction fails the validation step, then none of the
supplied transactions are scheduled.

**When to use:** Use this API when you are creating a consumer-facing
application that allows a consumer to schedule transactions. You may
create a consumer interaction that enables scheduling one transaction at
a time or a multiple transaction interaction that enables scheduling
transactions to different payees and submitting them all in a single
request.

Examples:

-   Create a Schedule Transaction interaction within your online bill
    payment application.

-   Schedule a transaction with a mobile device.

-   Schedule a single transaction.

-   Schedule multiple transactions to different payees.

### Method and Endpoint

| POST | /api/v1/me/Transactions|
|------|------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| allowDuplicateTransaction | Opt | query | boolean | Indicates that duplicate transactions are allowed. <br> Valid values: <br> true – Allow duplicate transactions <br> false – Check for duplicate transactions. This is the default.<br> A transaction cannot be a duplicate unless the allowDuplicateTransaction flag is true. A duplicate is defined as having the same payee, transaction amount, and transaction date. <br> This does not apply to SameDay Payments. Duplicate transactions are not allowed for SameDay Payments. |
| transaction | Req | body | Array of [Transaction](#transaction) | List of transactions to be scheduled for the consumer. Limited to 30 or fewer per request. |

### Response

| Parameter | Req | Data Type                                                                              | Description                                                       |
|------------|-----|-----------|----------------------------------------------|
| data      | Req | Array of [TransactionOutputIpsListItemResponse](#transactionoutputipslistitemresponse) | List of responses for the transactions requested to be scheduled. |
| result    | Req | [ResultType](#resulttype)                                                              | Overall result.                                                   |

### Sample API Usage

#### Request URL

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/transactions?allowDuplicateTransaction=true |
|------------------------------------------------------------------------|

#### Request Body 
```json
[
{
"amount": 21.13,
"deliveryDate": "2022-10-11",
"withdrawNow": false,
"note": null,
"destinationUri":
"/api/v1/me/payees/26257eb288ad4ea7bc7d1863a1d1ac2b",
"fundingAccountUri":
"/api/v1/me/bankAccounts/9c7df35c7ea54993a0a16e645bdddbde",
"listItemId": 1,
"memo": null
},
{
"amount": 45.96,
"deliveryDate": "2022-10-11",
"withdrawNow": false,
"note": null,
"destinationUri":
"/api/v1/me/payees/26257eb288ad4ea7bc7d1863a1d1ac2b",
"fundingAccountUri":
"/api/v1/me/bankAccounts/9c7df35c7ea54993a0a16e645bdddbde",
"listItemId": 2,
"memo": null
},
]
```
#### Response
```json
{
"data": [
{
"listItemId": 1,
"data": {
"confirmationNumber": "NK70H-0TRRJ",
"deliveryDate": "2022-10-11",
"debitDate": "2022-10-11",
"legacyTransactionId": "20220930060534080071",
"self":
"/api/v1/me/transactions/43764cb3a0c24a6e8c0894414285b500",
"id": "43764cb3a0c24a6e8c0894414285b500"
},
"result": {
"success": true,
"resultInfo": []
}
},
{
"listItemId": 2,
"data": {
"confirmationNumber": "NK70H-0V7Z1",
"deliveryDate": "2022-10-11",
"debitDate": "2022-10-11",
"legacyTransactionId": "20220930061508725429",
"self":
"/api/v1/me/transactions/38b7acdf649445c3bf0f25b48ffe40ff",
"id": "38b7acdf649445c3bf0f25b48ffe40ff"
},
"result": {
"success": true,
"resultInfo": []
}
},
],
"result": {
"success": true,
"resultInfo": []
}
}
```
## Update a Transaction

The Transactions Patch API enables the modification of one or more
editable fields of a previously scheduled payment that currently has a
status of Pending. This API can also be used to cancel a specific
transaction. After a payment begins processing, it can no longer be
modified or canceled.

**When to use:** If you are presenting a list of payments for a given
consumer, you may choose to present the option to the consumer to modify
and/or cancel payments that are in Pending status (if the values for
modifiableFields and cancelUri in the Transactions Get API response for
this consumer support these actions).

Example:

-   Present a payment list with an option to modify payments that are in
    Pending status.

### Method and Endpoint

To modify a transaction (requires Request Body):

| PATCH | /api/v1/me/Transactions/{transactionId}|
|--------|--------------------------|

To cancel a transaction (does not require Request Body):

| PATCH | /api/v1/me/Transactions/{transactionId}/cancel|
|--------|--------------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| transactionId | Req | path | string | Identifier for the transaction. |
| amount | Opt | body | double | The amount of the transaction. <br> Pattern: ^\\d+(\\.\\d{1,2})?$ |
| deliveryDate | Opt | body | string | The date that the payment transaction is to be delivered to its destination in yyyy-MM-dd format. | 
| fundingAccountUri | Opt | body | string | The funding account URI for the transaction. Account types that are eligible to be used are those enabled for Bill Payment (service) for the tenant/sponsor. |
| memo | Opt | body | string | Transaction memo. Maximum of 34 characters. This text will be printed on the check sent to the payee for this payment. A payment memo is only used for payments that are to be processed via a paper check. |
| note | Opt | body | string | A consumer’s “note to self.” This note is not submitted to the payee. Length: 0–255 <br> Pattern: ^[\\x2A-\\x2E\\x30-\\x39\\x40-\\x5A\\x5F\\x5E\\x61-\\x7A\\x20\\x21\\x23-\\x25]+\$ <br> The note field only allows the following character sets: a-z, A-Z, 0-9, _ @!+#.$,%^\*- | 
| withdrawNow | Opt | body | boolean | Reserved for future use. |
| cavv | Opt | body | string | Placeholder for 3-D Secure. Cardholder Authentication Verification Value. Used if the transaction is funded by a card account and the institution is participating in 3-D Secure. |

### Response

| Parameter | Req      | Data Type                                           | Description                                                                                                     |
|------------|------|------------|-------------------------------------------|
| data      | Req/Cond | [TransactionModifyOutput](#transactionmodifyoutput) | Data about the specific transaction. For a successful cancellation, no data is returned (HTTP status code 204). |
| result    | Req/Cond | [ResultType](#resulttype)                           | Result information. For a cancellation, this is only returned when the request fails.                           |

### Sample API Usage

#### Request URL – Modify Transaction

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/Transactions/d7a3a9d4ccee446dbbdc77cee5cbc |
|------------------------------------------------------------------------|

#### Request Body – Modify Transaction
```json
{
"amount": 6.0,
"note": "test",
"memo": "Modified Memo"
}
```
#### Response – Modify Transaction
```json
{
"data": {
"self":
"/api/v1/me/transactions/d7a3a9d4ccee446dbbdc77cee5cbcd98",
"id": "d7a3a9d4ccee446dbbdc77cee5cbcd98",
"confirmationNumber": "RKBS0-75GWS",
"deliveryMethod": "Electronic",
"status": "Pending",
"statusUri":
"/api/v1/me/transactions/d7a3a9d4ccee446dbbdc77cee5cbcd98/status",
"deliveryDate": "2021-05-19",
"cancelUri":
"/api/v1/me/transactions/d7a3a9d4ccee446dbbdc77cee5cbcd98/cancel",
"transactionType": "Standard",
"legacyTransactionId": "20210518134130189301"
},
"result": {
"success": true,
"resultInfo": []
}
}
```
#### Request URL – Cancel Transaction

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/Transactions/71c0b55fe40d43fc97c9500744bc311a/cancel |
|------------------------------------------------------------------------|

#### Response – Cancel Transaction

| 204 No Content |
|----------------|

# 