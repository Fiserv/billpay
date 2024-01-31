# CardAccounts

A card account is the debit card or a credit card (future release)
issued by the sponsor financial institution or another financial
institution. This section is only applicable if the client FI wants to
allow consumers to fund their bill payments using a debit card and/or
credit card (future release). The FI can select to either provide Fiserv
with the consumer’s card information or allow the consumers to add cards
through the client custom UI themselves.

Card accounts can only be used to make one-time payments for the
earliest payment date – not expedited, recurring, auto-pay, or
future-dated payments.

This set of APIs allows you to:

-   [Get a list of card accounts for a
    consumer](#get-card-accounts-list)

-   [Get a specific card account for a
    consumer](#get-a-card-account-single)

-   [Get information to invoke the 3DS SDK](#get-3ds-info)

-   [Get card account limits for a consumer](#get-card-account-limits)

-   [Add a card account for a consumer](#add-a-card-account)

-   [Update a consumer’s card account](#update-a-card-account)

-   [Delete a consumer’s card account](#delete-a-card-account)

## Get Card Accounts (List)

The CardAccounts Get API returns a list of card accounts available to a
consumer for funding a bill payment. Only active card accounts will be
returned. Use this API in conjunction with the BankAccounts Get API to
populate a list of all funding accounts available for a consumer to fund
a bill payment. A consumer must select one funding account from the list
to fund a payment.

### Method and Endpoint

| GET | /api/v1/me/cardAccounts |
|-----|----------------------------|

### Response

| Parameter | Req  | Data Type                                            | Description                                                                                                                  |
|-----------|-----|-----------|-----------------------------------------------|
| data      | Req  | Array of [CardAccountGetModel](#cardaccountgetmodel) | There is an empty array if there is no data to return.                                                                       |
| result    | Cond | [ResultType](#resulttype)                            | Result information. Condition: Only returned when the request fails. No content returned for success (HTTP status code 200). |

### Sample API Usage

#### Response
```json
{"self":
"/api/v1/me/cardAccounts/6d1f621d-267a-4ea1-b5d9-67bd4d4c8ab3",
"id": "6d1f621d-267a-4ea1-b5d9-67bd4d4c8ab3",
"addedBy": "Sponsor",
"cardNumberMasked": "********5672",
"deleteAllowed": false,
"cardType": "Visa",
"isDebit": true,
"billingAddress": {
"address1": "2321 HOUSEMAN",
"address2": "HUNSLOW",
"city": "Madrid Square",
"state": "NY",
"zipCode": "11332"
},
"expirationMonth": 7,
"expirationYear": 2021,
"externalAccountDescription": "ExternalAccountDescription",
"nameOnCard": "Steve Law",
"nickname": "Steve’s Debit Card"
}
```
## Get a Card Account (Single)

The CardAccounts Get API returns the details for a specific card account
available for funding a bill payment.

### Method and Endpoint

| GET | /api/v1/me/cardAccounts/{cardId} |
|-----|----------------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description                      |
|-----------|-----|------------|-----------|----------------------------------|
| cardId    | Req | path       | string    | Identifier for the card account. |

### Response

| Parameter | Req  | Data Type                                   | Description                         |
|-----------|-----|-----------|-----------------------------------------------|
| data      | Req  | [CardAccountGetModel](#cardaccountgetmodel) | Information about the card account. |
| result    | Cond | [ResultType](#resulttype)                   | Result information.                 |

### Sample API Usage

#### Request URL

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/cardAccounts/6d1f621d-267a-4ea1-b5d9-67bd4d4c8ab3 |
|------------------------------------------------------------------------|

#### Response
```json
{
"self":
"/api/v1/me/cardAccounts/6d1f621d-267a-4ea1-b5d9-67bd4d4c8ab3",
"id": "6d1f621d-267a-4ea1-b5d9-67bd4d4c8ab3",
"addedBy": "Sponsor",
"cardNumberMasked": "********5672",
"deleteAllowed": false,
"cardType": "Visa",
"isDebit": true,
"billingAddress": {
"address1": "2321 HOUSEMAN",
"address2": "HUNSLOW",
"city": "Madrid Square",
"state": "NY",
"zipCode": "11332"
},
"expirationMonth": 7,
"expirationYear": 2021,
"externalAccountDescription": "ExternalAccountDescription",
"nameOnCard": "Steve Law",
"nickname": "Steve’s Debit Card"
}
```
## Get 3DS Info

This is a placeholder for an API that will return information needed by
the client to invoke the 3-D Secure (3DS) SDK.

### Method and Endpoint

GET /api/v1/me/cardAccounts/{id}/3DSInfo

### Response

| Parameter | Req  | Data Type                 | Description                                                                                                                  |
|---------|-----|-----------|------------------------------------------------|
| data      | Req  | [3DSInfo](#dsinfo)        | There is an empty array if there is no data to return.                                                                       |
| result    | Cond | [ResultType](#resulttype) | Result information. Condition: Only returned when the request fails. No content returned for success (HTTP status code 200). |

#### 3DSInfo

| Parameter    | Req | Data Type | Description                                                  |
|---------|-----|-----------|-------------------------------------------------|
| CardToken    | Req | string    | Token that should be fed as input to the 3DS SDK.            |
| 3DSServerUrl | Req | string    | Path to the 3-D Secure Server where requests should be sent. |

## Get Card Account Limits

The Card Account Add Limits Get API returns the card limits that apply
to a consumer. This is used to verify whether a consumer is allowed to
add a card.

### Method and Endpoint

| GET | /api/v1/me/cardAccounts/CardAccountAddLimits |
|-----|----------------------------|

### Response

| Parameter | Req | Data Type                                     | Description                            |
|-----------|-----|-----------|-----------------------------------------------|
| data      | Req | [CardAccountAddLimits](#cardaccountaddlimits) | Information about card account limits. |
| result    | Req | [ResultType](#resulttype)                     | Result information.                    |

### Sample API Usage

#### Request URL

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/cardAccounts/CardAccountAddLimits |
|------------------------------------------------------------------------|

#### Response
```json
{
"data": {
"rollingPeriodDaysLimit": 30,
"rollingPeriodDaysLimitModifiable": true,
"rollingPeriodCardsLimit": 1,
"rollingPeriodCardsRemainingCount": 1,
"rollingPeriodCardsLimitModifiable": true,
"sameCardDeleteAddLimit": 3,
"sameCardDeleteAddLimitModifiable": true,
"activeCardLimit": 1,
"activeCardRemainingCount": 1,
"activeCardLimitModifiable": true },
"result": {
"success": true
}
```
## Add a Card Account

The CardAccounts Post API enables adding a card account as a funding
account for a consumer. This should be integrated with a UI that allows
a consumer to enter card account information.

### Method and Endpoint

| POST | /api/v1/me/cardAccounts |
|------|------------------|

###  Request

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| cardNumber | Req | body | string | Consumer card account number. Must be numeric. Length: 15-19 <br> Pattern: ^[0-9]\* |
| cardType | Req | body | string | Indicates the type of card. Valid values: AmericanExpress, Discover, MasterCard, Visa |
| expirationMonth | Req | body | integer | Month in which the card expires. Length: 2. Value: 01–12 <br> Pattern: ^([1-9]\|1[012])$ |
| expirationYear | Req | body | integer | Year in which the card expires. Length: 4. Format: yyyy <br>Pattern: ^[0-9]{4}$ |
| externalAccountDescription | Req | body | string | External account description that is free-form text, such as: “Bank Name” Visa. Length: 1–32 |
| nameOnCard | Req | body | string | Name printed on the card. Length: 1–80 | 
| nickname | Opt | body | string | A description of the card used to help identify it in a list. Length: 1–30 <br> No validations; free-form text. Do not enter any sensitive information such as the account number. |
| cvv | Cond | body | integer | The CVV number on the card. Must be numeric. Length: 3-4. <br> Pattern: ^[0-9]\* <br> This value is not stored or displayed anywhere. <br> Condition: Required for cards added by consumer. (Not required for cards added by FI.) |
| billingAddress | Req | body | [USAddress](#usaddress) | The billing address for the card. |

### Response

| Parameter | Req | Data Type | Description | 
|-----------|-----|-----------|-------------|
| data | Cond | [CardAccountPostResponse](#cardaccountpostresponse) | Response data. Condition: Always returned for successful response. |
| result | Req | [ResultType](#resulttype) | Result Information. |

### Sample API Usage

#### 

#### Request
```json
{
"billingAddress": {
"address1": "6883 Made Up St.",
"address2": "Apt 354",
"city": "Athens",
"state": "GA",
"zipCode": "30306"
},
"cardNumber": "4022014646433778",
"cardType": "VISA",
"expirationMonth": 12,
"expirationYear": 2020,
"externalAccountDescription": " Ext Acct description ",
"nameOnCard": "TESTCARDACCOUNT",
"nickname": "TstCrd",
"cvv": "000"
}
```
#### Response
```json
{
"data": {
"self":
"/api/v1/me/cardAccounts/fdb5a27012ab406eb3690a3186250d42",
"id": "fdb5a27012ab406eb3690a3186250d42"
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
## Update a Card Account 

The CardAccounts Patch API enables modifying a card account that is a
funding account for a consumer. The fields that can be modified are
limited. The calling application should only pass those attributes in
the request that are required to be updated. A consumer can update
consumer-added cards only, and an FI can update FI-added cards.

### Method and Endpoint

| PATCH | /api/v1/me/cardAccounts/{cardId}|
|--------|--------------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| cardId | Req | path | string | Identifier for the card account. |
| billingAddress | Opt | body | [USAddress](#usaddress) | The billing address for the card. |
| expirationMonth | Opt | body | integer | Month in which the card expires. Length: 2. Value: 01–12 <br> Pattern: ^([1-9]\|1[012])$ |
| expirationYear | Opt | body | integer | Year in which the card expires. Length: 4. Format: yyyy <br>Pattern: ^[0-9]{4}$ |
| externalAccountDescription | Opt | body | string | External account description that is free-form text, such as: “Bank Name” Visa. Length: 1–32 |
| nameOnCard | Opt | body | string | Name printed on the card. Length: 1–80 |
| nickname | Opt | body | string | A description of the card used to help identify it in a list. No validations; free-form text. Do not enter any sensitive information such as the account number. Length: 1–30 |
| cvv | Cond | body | integer | The CVV number on the card. Must be numeric. Length: 3-4. <br> Pattern: ^[0-9]\* <br> Condition: For cards added by consumer, required if anything except nickname and/or externalAccountDescription is modified. Not used for FI-added cards. <br> This value is not stored or displayed anywhere. | 

### Response

| Parameter | Req  | Data Type                 | Description                                                                                                                  |
|-------------|-----|-------|-------------------------------------------------|
| result    | Cond | [ResultType](#resulttype) | Result information. Condition: Only returned when the request fails. No content returned for success (HTTP status code 204). |

### Sample API Usage

#### Request URL

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/cardAccounts/f223d18d-e07c-4228-a51f-917935d15818 |
|------------------------------------------------------------------------|

#### Request Body
```json
{
"billingAddress": {
"address1": "6883 Made Up St.",
"address2": "Apt 354",
"city": "Athens",
"state": "GA",
"zipCode": "30306"
},
"expirationMonth": 6,
"expirationYear": 2018,
"externalAccountDescription": "Ext Acct description",
"nameOnCard": "TESTCARDACCOUNT",
"nickname": "Test card",
"cvv": "000"
}
```
#### Response

| 204 No Content |
|----------------|

## Delete a Card Account

The CardAccounts Put API enables deleting a card account that is a
funding account for a consumer. A consumer can delete consumer-added
cards only, and an FI can delete FI-added cards.

### Method and Endpoint

| DELETE | /api/v1/me/cardAccounts/{cardId}|
|--------|----------------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description                      |
|-----------|-----|------------|-----------|----------------------------------|
| cardId    | Req | path       | string    | Identifier for the card account. |

### Response

| Parameter | Req  | Data Type                 | Description                                                                                                                                  |
|------------|------|-------|-----------------------------------------------|
| result    | Cond | [ResultType](#resulttype) | Result associated with the request. Condition: Only returned when the request fails. No content returned for success (HTTP status code 204). |

### Sample API Usage

#### Request URL

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/cardAccounts/f223d18d-e07c-4228-a51f-917935d15818 |
|------------------------------------------------------------------------|

#### Response

| 204 No Content |
|----------------|

# 