# TransactionCalendar

This API allows you to:

-   [Get a list of possible transaction dates](#get-transaction-calendar)

## Get Transaction Calendar

The Transaction Calendar Get API enables the retrieval of valid
potential payment dates based on many factors at the tenant/sponsor
level as well as the consumer’s payee level.

**When to use:** Use the Transaction Calendar Get API as the primary
means of retrieving valid potential payment dates for bill payment
transactions. This is often the mechanism for enabling a consumer to see
upcoming dates for which they can schedule a bill payment transaction.

### Method and Endpoint

| GET | /api/v1/me/transactionCalendar|
|-----|----------------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| transactionDestinationUri | Cond | query | Array of string | The destination for a transaction; this is a URI for a payee, bill, or ToDo item. Multiple destinations can be provided. <br> Condition: Either transactionDestinationUri <strong>or</strong> transactionUri must be provided. |
| fundingAccountUri | Opt | query | string | The funding account for the transaction.| 
| amount | Opt | query | double | The amount of the transaction.<br> If the consumer provides the amount and the supplied funding account is a bank account, the amount will be validated against the minimum and maximum value of the sponsor bill pay profile limit. An error will be thrown if the amount is not within the bounds of the sponsor bill pay profile limit. |
| startDate | Opt | query | string | The desired start date for the list of dates. Format: yyyy-MM-dd |
| endDate | Opt | query | string | The desired end date for the list of dates. Format: yyyy-MM-dd |
| transactionUri | Cond | query | string | The URI for an existing transaction. Condition: Either transactionDestinationUri <strong>or</strong> transactionUri must be provided. |

### Response

| Parameter | Req | Data Type                                              | Description                                            |
|----------|-----|----------|------------------------------------------------|
| data      | Req | Array of [TransactionCalendar](?path=docs/apiDomains/complexObjects.md&branch=develop#transactioncalendar) | There is an empty array if there is no data to return. |
| result    | Req | [ResultType](?path=docs/apiDomains/complexObjects.md&branch=develop#resulttype)                              | Result information.                                    |

### Sample API Usage

#### Request URL

https://api-checkfreenext-cert.fiservapps.com/api/v1/me/
transactionCalendar?transactionDestinationUri=/api/v1/me/payees/81c30842af1a42549c09327605fb7b30

#### Response
```json
{
"data": [
{
"deliveryDate": "2018-12-07",
"transactionDestinationUri":
"/api/v1/me/payees/81c30842af1a42549c09327605fb7b30",
"transactionOptions": [
{
"cutOffTime": "2018-12-06T20:00:00Z",
"deliveryMethod": "Electronic",
"fee": 0,
"fundingAccountUri":
"/api/v1/me/bankAccounts/18173075a4ab44ed8765483f4e07bfbe",
"withdrawNow": false,
"instantDelivery": false,
"isPreferredDate": false,
"maxAmount": 99999.99,
"minAmount": 1
}
]
},
{
"deliveryDate": "2018-12-10",
"transactionDestinationUri":
"/api/v1/me/payees/81c30842af1a42549c09327605fb7b30",
"transactionOptions": [
{
"deliveryMethod": "Electronic",
"fee": 0,
"fundingAccountUri":
"/api/v1/me/bankAccounts/18173075a4ab44ed8765483f4e07bfbe",
"withdrawNow": false,
"instantDelivery": false,
"isPreferredDate": false,
"maxAmount": 99999.99,
"minAmount": 1
}
]
},

{
"deliveryDate": "2019-02-06",
"transactionDestinationUri":
"/api/v1/me/payees/81c30842af1a42549c09327605fb7b30",
"transactionOptions": [
{
"deliveryMethod": "Electronic",
"fee": 0,
"fundingAccountUri":
"/api/v1/me/bankAccounts/18173075a4ab44ed8765483f4e07bfbe",
"withdrawNow": false,
"instantDelivery": false,
"isPreferredDate": false,
"maxAmount": 99999.99,
"minAmount": 1
}
]
}
],
"result": {
"success": true,
"resultInfo": []
}
}
```