# E-bill Activation

These APIs are an extension to the Payee APIs. They are only applicable
to the payees that support and can deliver bills electronically. For an
e‑bill capable payee, these APIs can be used to offer and manage e-bill
activation and service.

-   [Begin the process of activating e-bill service for a payee](#get-payee-e-bill-capability)

-   [Complete the process of activating e‑bill service for a payee](#activate-payee-e-bill-service)

-   [Get e-bill service information for a payee](#get-payee-e-bill-service)

-   [Change e-bill service for a payee](#change-payee-e-bill-service)

-   [Cancel e-bill service for a payee](#delete-payee-e-bill-service)

## Get Payee E-bill Capability

The Payee E-bill Capability Get API enables a consumer to begin the
process of requesting e-bill service activation for a payee on that
consumer’s payee list. This API will provide to the calling application
the data required to present e-bill activation to the consumer for an
e-bill capable biller and will enable the UI to begin a flow where the
consumer is prompted to provide information specific to the specified
payee so that e-bills from that payee can be delivered to the consumer.
This operation is specifically for initiating e-bill service for a
single payee on a consumer’s payee list.

**When to use:** This API is available for use when the UI application
wants to enable a consumer to activate e-bill service. This specific API
is typically used as part of a broader flow where the consumer is made
aware by the UI that the payee supports e-bills and is offered to
activate e-bill service as part of that flow.

### Method and Endpoint

| GET | /api/v1/me/payees/{id}/ebillCapability |
|-----|----------------------------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description               |
|-----------|-----|------------|-----------|---------------------------|
| id        | Req | path       | string    | Identifier for the payee. |

### Response

| Parameter | Req | Data Type | Description |
|-----------|-----|-----------|-------------|
| data | Req | [EbillCapability](?path=docs/apiDomains/complexObjects.md&branch=develop#ebillcapability) | Information required for e-bill activation. Empty if there is no data to return. |
| result | Cond | [ResultType](?path=docs/apiDomains/complexObjects.md&branch=develop#resulttype) | Result information. <br> Condition: Only returned when the request fails. No result content returned for success (HTTP status code 200). (Known issue: result is currently being returned in response for success.) |

### Sample API Usage

#### Request URL

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/payees/afa28534bea54a00b23da127380087b4/ebillCapability |
|------------------------------------------------------------------------|

#### Response
```json
{
"data": {
"billerName": "Boston Edison",
"billerSiteUrl": "http://www.nstaronline.com/",
"paperSuppressionOptions": [
{
"option": "PaperSuppression",
"termsText": "Send my Boston Edison bill here and stop delivering paper bill in the mail. I have read and agree to the biller's[@TCURL@].",
"termsUrl":
"https://secure.cmax.americanexpress.com/Render/docs/en/CheckFree/TermsandConditions/TermsandConditions.html",
"isEmbedded": false
},
{
"option": "Trial",
"termsText": "I want to try it first, for 4 days, and I will decide later if I want to stop my paper bill. Send my bill both here and in the mail. I have read and agree to the biller's [@TCURL@].",
"termsUrl": "http://www.yahoo.com",
"isEmbedded": false
}
],
"preAuthTokens": [
{
"name": "Last 4 Digits SSN",
"value": "0000",
"regularExpression": "^[0-9]{4,4}$"
},
{
"name": "Home Phone Number (Digits only)",
"value": "5552351829",
"regularExpression": "^[0-9]{10,10}$"
}
],
"restrictionText": "Sample restriction text",
"serviceAddressRequired": true,
"serviceAddress": {
"address1": "1187 Made Up St.",
"address2": "Mailbox 1034",
"city": "Dublin",
"state": "OH",
"zipCode": "43016"
}
"trialPeriodDays": 4,
"accountNumberEditable": true,
"self":
"/api/v1/me/payees/afa28534bea54a00b23da127380087b4/ebillCapability",
"id": "2587dd43-dd6a-421a-9653-f1691d836749"
},
"result": {
"success": true,
"resultInfo": [
{
"resultCategory": "Info",
"code": "0000",
"description": "Success."
}
]
}
}
```
## Activate Payee E-bill Service

The Payee E-bill Service Post API enables a consumer to submit the
activation request for the e-bill service to an e-bill capable payee on
that consumer’s payee list. This operation enables the consumer to
provide back to Fiserv any information required by the biller to
activate e-bill service. The activation process between Fiserv and the
biller can take anywhere from one day to two billing cycles.

**When to use:** This API is available for use when the UI application
has presented the e-bill activation flow to the consumer and the
consumer has provided the information required to submit the e-bill
activation request.

### Method and Endpoint

| POST | /api/v1/me/payees/{id}/ebillService |
|------|-------------------------------------|

### Request

| Parameter                 | Req | Param Type | Data Type                                               | Description                                |
|------------|----|-------|-----------|---------------------------------------|
| id                        | Req | path       | string                                                  | Identifier for the payee.                  |
| ebillServiceActivateInput | Req | body       | [EbillServiceActivateInput](?path=docs/apiDomains/complexObjects.md&branch=develop#ebillserviceactivateinput) | Information for e-bill service activation. |

### Response

| Parameter | Req | Data Type                                                 | Description                                              |
|---------|----|------------|------------------------------------------------|
| data      | Req | [EbillServiceActivateOutput](?path=docs/apiDomains/complexObjects.md&branch=develop#ebillserviceactivateoutput) | E-bill service information. Empty if the request failed. |
| result    | Req | [ResultType](?path=docs/apiDomains/complexObjects.md&branch=develop#resulttype)                                 | Result information.                                      |

### Sample API Usage

#### Request URL

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/payees/afa28534bea54a00b23da127380087b4/ebillService |
|------------------------------------------------------------------------|

#### Request Body
```json
{
"acceptedTC": true,
"businessName": null,
"emailAddressShare": false,
"name": {
"first": "Jack",
"last": "Userford",
"middle": null,
"nickname": null,
"prefix": null
},
"paperSuppressionOption": "PaperSuppression",
"preAuthTokens": [
{
"name": "Last 4 Digits SSN",
"value": "8178"
},
{
"name": "Home Phone Number (Digits only)",
"value": "9387645137"
}
],
"serviceAddress": {
"address1": "567 User Lane",
"address2": null,
"city": "Alpharetta",
"state": "GA",
"zipCode": "30004"
}
}
```
#### Response
```json
{
"data": {
"activationState": "Pending",
"self":
"/api/v1/me/payees/afa28534bea54a00b23da127380087b4/ebillService",
"id": "afa28534bea54a00b23da127380087b4"
},
"result": {
"success": true,
"resultInfo": []
}
}
```
## Get Payee E-bill Service

The Payee E-bill Service Get API returns information about the e-bill
service from a given payee to the associated consumer. This information
includes identifying if a consumer is currently in an e-bill trial
period, the duration of that trial period, whether or not the first
e-bill has been received, and whether or not paper bills are still being
delivered.

**When to use:** Use this API to identify when messaging should be
presented to a consumer about payees that are in an e-bill trial period.
When an e‑bill account has met all of the necessary business rule
criteria, this operation may also be used to drive the presentation of
e-bill options to the consumer, such as setting up e-bill automatic
transactions (criteria: the payee must not be in a trial period and the
consumer must have already received the first e-bill). Additionally,
this operation is used as part of managing paper suppression.

Examples:

-   Identify payees in a consumer’s payee list that are in a trial
    period, present messaging to the consumer about the duration of that
    trial period, and promote the completion of the e-bill activation
    signup process to the consumer.

-   As part of providing a consumer with options for setting up e-bill
    automatic payments, retrieve the details about the consumer’s e-bill
    service to ensure that all business rule criteria have been met for
    that payee to be eligible for setting up automatic transactions.

### Method and Endpoint

| GET | /api/v1/me/payees/{id}/ebillService |
|-----|-------------------------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description               |
|-----------|-----|------------|-----------|---------------------------|
| id        | Req | path       | string    | Identifier for the payee. |

### Response

| Parameter | Req | Data Type | Description |
|-----------|-----|-----------|-------------|
| data | Req | [EbillAccountDetail](?path=docs/apiDomains/complexObjects.md&branch=develop#ebillaccountdetail) | E-bill service information. |
| result | Cond | [ResultType](?path=docs/apiDomains/complexObjects.md&branch=develop#resulttype) | Result information. <br> Condition: Only returned when the request fails. No result content returned for success. (Known issue: result is currently being returned in response for success.) |

### Sample API Usage

#### Request URL

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/payees/afa28534bea54a00b23da127380087b4/ebillService |
|------------------------------------------------------------------------|

#### Response
```json
{
"data": {
"billTransactionScheduleEligible": true,
"emailAddressShare": false,
"firstEbillReceived": true,
"name": {
"first": "JACK",
"last": "USERFORD"
},
"isTrialPeriod": false,
"paperProcessingStatus": "Unknown",
"paperSuppressionTermsUrl": "http://www.google.com",
"promptPaperSuppression": false,
"serviceAddress": {
"address1": "567 USER LANE",
"city": "ALPHARETTA",
"state": "GA",
"zipCode": "30004"
},
"serviceEveningPhoneNumber": "9387645137",
"serviceHolderType": "Consumer",
"status": "Active",
"transactionScheduleTypesSupported": [
"FixedAmount",
"AmountDue"
],
"self":
"/api/v1/me/payees/afa28534bea54a00b23da127380087b4/ebillService"
},
"result": {
"success": true,
"resultInfo": [
{
"resultCategory": "Info",
"code": "0000",
"description": "Success."
}
]
}
}
```
## Change Payee E-bill Service

This API is used to enable a consumer who is using the e-bill service
and is in a trial period for a payee to convert from the trial to a full
activation.

### Method and Endpoint

| PATCH | /api/v1/me/payees/{id}/ebillService/SuppressPaper|
|--------|-------------------------------------------------|

### Request

| Parameter  | Req | Param Type | Data Type | Description                                                                        |
|------------|----|-------|-------|-------------------------------------------|
| id         | Req | path       | string    | Identifier for the payee.                                                          |
| acceptedTC | Req | body       | boolean   | Indicates if the consumer has accepted the paper suppression terms and conditions. |

### Response

| Parameter | Req | Data Type | Description |
|-----------|-----|-----------|-------------|
| result | Cond | [ResultType](?path=docs/apiDomains/complexObjects.md&branch=develop#resulttype) | Result information. <br> Condition: Only returned when the request fails. No content returned for success (HTTP status code 204). |

### Sample API Usage

#### Request URL

https://api-checkfreenext-cert.fiservapps.com/api/v1/me/payees/c02255bdbac3439caed063d3b9a04b56/ebillService/SuppressPaper

#### Request Body
```json
{
"acceptedTC": true
}
```
#### Response

| 204 No Content |
|----------------|

## Delete Payee E-bill Service

The Payee E-bill Service Delete API enables a consumer to cancel their
e-bill service. Electronic bills will no longer be delivered to the
consumer and the biller must send paper statements to the consumer. Any
automatic payment plan associated with the e-bill service will also be
deleted.

**When to use:** This API is available for use when the UI application
wants to enable a consumer to manage e-bill service for that consumer’s
payees.

### Method and Endpoint

| DELETE | /api/v1/me/payees/{id}/ebillService|
|--------|------------------------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description               |
|-----------|-----|------------|-----------|---------------------------|
| id        | Req | path       | string    | Identifier for the payee. |

### Response

| Parameter | Req  | Data Type                 | Description                                                                                                                                  |
|---------|------|-----------|-----------------------------------------------|
| result    | Cond | [ResultType](?path=docs/apiDomains/complexObjects.md&branch=develop#resulttype) | Result associated with the request. Condition: Only returned when the request fails. No content returned for success (HTTP status code 204). |

### Sample API Usage

#### Request URL

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/payees/c7b699810d0b40178810b0a5d8feb02f/ebillService |
|------------------------------------------------------------------------|

#### Response

| 204 No Content |
|----------------|