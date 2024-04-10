# BankAccounts

A bank account is the financial account of the consumer at the financial
institution that has been enabled to fund a bill payment. The bank
accounts will be set up for use with bill pay using existing Builder
APIs.

This set of APIs allows the calling application to:

-   [Retrieve a financial institution name](#get-financial-institution-name)

-   Get a list of bank accounts to be used to fund a payment

    - [Managed user](#get-bank-accounts-for-managed-user-list)

    - [Consumer](#get-bank-accounts-for-a-consumer-list)

-   Get a specific bank account

    - [Managed user](#get-a-bank-account-for-a-managed-user-single)

    - [Consumer](#get-a-bank-account-for-a-consumer-single)

-   [Add a bank account for a managed user](#add-a-bank-account-for-a-managed-user)

-   Update a bank account

    - [Managed user](#update-a-bank-account-for-a-managed-user)

    - [Consumer](#update-a-bank-account-for-a-consumer)

-   [Delete a bank account for a managed user](#delete-a-bank-account-for-a-managed-user)

-   Get the unmasked account number for a bank account

    - [Maintenance grant scoped to tenant](#get-an-unmasked-bank-account-number-tenant-scoped)

    - [Maintenance grant scoped to consumer](#get-an-unmasked-bank-account-number-consumer-scoped)

## Get Financial Institution Name

The Financial Institution Get API is used to retrieve the FI name for a
given routing number.

### Method and Endpoint

| GET | /api/v1/financiallnstitution|
|-----|----------------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| routingTransitNumber | Req | query | string | Routing and transit number for a bank account. Length: 9 <br> Pattern: ^[0-9]{9}$ |

### Response

| Parameter | Req  | Data Type                                               | Description                                                        |
|-----------|-----|-----------|----------------------------------------------|
| data      | Cond | [FinancialInstitutionModel](./complexObjects.md#financialinstitutionmodel) | Response data. Condition: Always returned for successful response. |
| result    | Req  | [ResultType](./complexObjects.md#resulttype)                               | Result information.                                                |

### Sample API Usage

#### Request URL

| https://api-checkfreenext-cert.fiservapps.com/api/v1/financialInstitution?routingTransitNumber=xxxxxxxxx |
|------------------------------------------------------------------------|

#### Response
```json
{
"data": {
"financialInstitutionName": "BANK"
},
"result": {
"success": true
}
}
```
## Get Bank Accounts for Managed User (List)

The BankAccounts Get API returns a list of all bank accounts for funding
a bill payment for a managed user.

Note

For the authentication request, the client\_id must be configured for
UserMaintenance. Do not send fiserv.identity.billpay.subscriberId; see
[Sample API Usage: Authentication Request for User
Maintenance](../resourcesAndGuides/authenticate.md#sample-api-usage-authentication-request-for-user-maintenance).

Because the maintenance grant is scoped to a tenant, the user ID for the
consumer must be provided in the BankAccounts Get request.

### Method and Endpoint

| GET | /api/v1/users/{userId}/bankAccounts|
|-----|----------------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| userId | Req | path | string | Identifier for the managed user. |
| returnInactiveBankAccounts | Opt | query | boolean | Indicates whether to return inactive bank accounts in the response. <br> True – Return inactive bank accounts <br> False – Do not return inactive bank accounts. This is the default. |
| idType | Opt | query | string | Identifies the user ID type. Valid values: SubscriberId, ExternalSubscriberId, CheckFreeNextUserId <br> CheckFreeNextUserId is the default. |

### Response

| Parameter | Req | Data Type                                            | Description                                                                                                          |
|-----------|----|-----------|-----------------------------------------------|
| data      | Req | Array of [BankAccountGetModel](./complexObjects.md#bankaccountgetmodel) | Collection of all bank accounts available to fund a payment. Empty if there is no data to return or an error occurs. |
| result    | Req | [ResultType](./complexObjects.md#resulttype)                            | Result information.                                                                                                  |

### Sample API Usage

#### Request URL

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/users/a8ffbc12db4ae911a83400505689ef1f/bankAccounts |
|------------------------------------------------------------------------|

#### Response
```json
{
"data": [
{
"self":
"/api/v1/me/users/a8ffbc12db4ae911a83400505689ef1f/bankAccounts/d457a828a84342b2a6e2088b37f3db34",
"id": "d457a828a84342b2a6e2088b37f3db34",
"isBilling": true,
"modifiableFields": [
"nickname"
],
"accountType": "Checking",
"maskedAccountNumber": "***********6115",
"accountBalance": 198.59,
"isPreferred": true,
"unmaskedAccountNumberUri":
"/api/v1/me/bankAccounts/d457a828a84342b2a6e2088b37f3db34/accountNumber",
"routingTransitNumber": "122235821",
"isBusiness": false,
"primaryAccountOwner": "",
"secondaryAccountOwner": "",
"checkPrintAddress": {
"address1": "1614 Main St.",
"city": "Athens",
"state": "GA",
"zipCode": "30306"
}
}
],
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
## Get Bank Accounts for a Consumer (List)

The BankAccounts Get API returns a list of all bank accounts available
for funding a bill payment for the user associated with the issued
bearer token. The UI should present this list for the consumer to select
one of the bank accounts for funding a payment.

### Method and Endpoint

| GET | /api/v1/me/bankAccounts|
|-----|----------------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| returnInactiveBankAccounts | Opt | query | boolean | Indicates whether to return inactive bank accounts in the response. <br> True – Return inactive bank accounts <br> False – Do not return inactive bank accounts. This is the default.|

### Response

| Parameter | Req | Data Type                                            | Description                                                                                                          |
|-----------|----|-----------|-----------------------------------------------|
| data      | Req | Array of [BankAccountGetModel](./complexObjects.md#bankaccountgetmodel) | Collection of all bank accounts available to fund a payment. Empty if there is no data to return or an error occurs. |
| result    | Req | [ResultType](./complexObjects.md#resulttype)                            | Result information.                                                                                                  |

### Sample API Usage

#### Request URL

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/bankAccounts |
|----------------------------------------------------------------------|

#### Response
```json
{
"data": [
{
"self":
"/api/v1/me/bankAccounts/35ecac78659e48c99e92ff18ec21ca43",
"id": "35ecac78659e48c99e92ff18ec21ca43",
"isBilling": true,
"modifiableFields": [
"nickname"
],
"accountType": "Checking",
"maskedAccountNumber": "***********6115",
"accountBalance": 198.59,
"isPreferred": true,
"unmaskedAccountNumberUri":
"/api/v1/me/bankAccounts/35ecac78659e48c99e92ff18ec21ca43/accountNumber",
"routingTransitNumber": "261070015",
"isBusiness": false,
"primaryAccountOwner": "",
"secondaryAccountOwner": "",
"checkPrintAddress": {
"address1": "1614 Main St.",
"city": "Athens",
"state": "GA",
"zipCode": "30306"
}
}
],
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
## Get a Bank Account for a Managed User (Single)

The BankAccounts Get API returns the details for a specific bank account
available for funding a bill payment for a managed user.

Note

For the authentication request, the client\_id must be configured for
UserMaintenance. Do not send fiserv.identity.billpay.subscriberId; see
[Sample API Usage: Authentication Request for User
Maintenance](../resourcesAndGuides/authenticate.md#sample-api-usage-authentication-request-for-user-maintenance).

Because the maintenance grant is scoped to a tenant, the user ID for the
consumer must be provided in the BankAccounts Get request.

### Method and Endpoint

| GET | /api/v1/users/{userId}/bankAccounts/{bankAccountCommonId}|
|-----|----------------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| userId | Req | path | string | Identifier for the managed user. |
| bankAccountCommonId | Req | path | string | Identifier for the bank account. |
| idType | Opt | query | string | Identifies the user ID type. Valid values: SubscriberId, ExternalSubscriberId, CheckFreeNextUserId <br> CheckFreeNextUserId is the default. |

### Response

| Parameter | Req | Data Type                                   | Description                         |
|------------|-----|-----------|---------------------------------------------|
| data      | Req | [BankAccountGetModel](./complexObjects.md#bankaccountgetmodel) | Information about the bank account. |
| result    | Req | [ResultType](./complexObjects.md#resulttype)                   | Result information.                 |

### Sample API Usage

#### Request URL

https://api-checkfreenext-cert.fiservapps.com/api/v1/me/users/
1480e4e304e8e811a83000505689ef1f/bankAccounts/57694efd08e548dfb552e639269af6eb

#### Response
```json
{
"data": {
"self":
"/api/v1/me/users/1480e4e304e8e811a83000505689ef1f/bankAccounts/57694efd08e548dfb552e639269af6eb",
"id": "57694efd08e548dfb552e639269af6eb",
"isBilling": true,
"modifiableFields": [
"nickname"
],
"accountType": "Checking",
"maskedAccountNumber": "***********6115",
"accountBalance": 198.59,
"isPreferred": true,
"unmaskedAccountNumberUri":
"/api/v1/me/bankAccounts/57694efd08e548dfb552e639269af6eb/accountNumber",
"routingTransitNumber": "122235821",
"isBusiness": false,
"primaryAccountOwner": "",
"secondaryAccountOwner": "",
"checkPrintAddress": {
"address1": "1614 Main St.",
"city": "Athens",
"state": "GA",
"zipCode": "30306"
}
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
## Get a Bank Account for a Consumer (Single)

The BankAccounts Get API returns the details for a specific bank account
available for funding a bill payment for the user associated with the
issued bearer token.

### Method and Endpoint

| GET | /api/v1/me/bankAccounts/{bankAccountCommonId}|
|-----|----------------------------|

### Request

| Parameter           | Req | Param Type | Data Type | Description                      |
|-----------|-----|--------|-------|-------------------------------------------|
| bankAccountCommonId | Req | path       | string    | Identifier for the bank account. |

### Response

| Parameter | Req | Data Type                                   | Description                         |
|------------|-----|-----------|---------------------------------------------|
| data      | Req | [BankAccountGetModel](./complexObjects.md#bankaccountgetmodel) | Information about the bank account. |
| result    | Req | [ResultType](./complexObjects.md#resulttype)                   | Result information.                 |

### Sample API Usage

#### Request URL

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/bankAccounts/35ecac78659e48c99e92ff18ec21ca43 |
|------------------------------------------------------------------------|

#### Response
```json
{
"data": {
"self":
"/api/v1/me/bankAccounts/35ecac78659e48c99e92ff18ec21ca43",
"id": "35ecac78659e48c99e92ff18ec21ca43",
"isBilling": true,
"modifiableFields": [
"nickname"
],
"accountType": "Checking",
"maskedAccountNumber": "***********6115",
"accountBalance": 198.59
"isPreferred": true,
"unmaskedAccountNumberUri":
"/api/v1/me/bankAccounts/35ecac78659e48c99e92ff18ec21ca43/accountNumber",
"routingTransitNumber": "261070015",
"isBusiness": false,
"primaryAccountOwner": "",
"secondaryAccountOwner": "",
"checkPrintAddress": {
"address1": "1614 Main St.",
"city": "Athens",
"state": "GA",
"zipCode": "30306"
}
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
## Add a Bank Account for a Managed User

The BankAccounts Post API enables adding a bank account as a funding
account for a managed user.

Note

For the authentication request, the client\_id must be configured for
UserMaintenance. Do not send fiserv.identity.billpay.subscriberId; see
[Sample API Usage: Authentication Request for User
Maintenance](../resourcesAndGuides/authenticate.md#sample-api-usage-authentication-request-for-user-maintenance).

Because the maintenance grant is scoped to a tenant, the user ID for the
consumer must be provided in the BankAccounts Post request.

### Method and Endpoint

| POST | /api/v1/me/users/{userId}/bankAccounts|
|------|---------------------------------------|

###  Request

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| userId | Req | path | string | Identifier for the managed user. |
| idType | Opt | query | string | Identifies the user ID type. Valid values: SubscriberId, ExternalSubscriberId, CheckFreeNextUserId <br> CheckFreeNextUserId is the default. |
| routingTransitNumber | Req | body | string | Routing and transit number for the bank account. Length: 9 <br> Must be numeric <br> Pattern: ^[0-9]{9}$ |
| accountNumber | Req | body | string | Consumer’s bank account number. Length: 1–22 <br> Pattern: ^[a-zA-Z0-9]{1,22}$ <br> Must contain uppercase characters when alphabetic characters are part of the account number. |
| accountType | Req | body | string | The type of bank account. Valid values: "Checking", "Savings", "InstallmentLoan", "IndividualRetirement", "CommercialLoan", "MoneyMarket", "LineOfCredit", "Brokerage", "SpecialDeposit" |
| nickname | Opt | body | string | A description of the bank account used to help identify it in a list. Length: 1–30 <br> Pattern: ^[a-zA-Z0-9_(){}&amp;@!+#.'$,%^ \*-]\* <br> Do not enter any sensitive information such as the account number. |
| startingCheckNumber | Opt | body | integer | Starting check number for paper drafts. Maximum value: 9999 <br> Valid values: 1-9999 |
| isPreferred | Opt | body | boolean | Indicates if the account is the preferred account. The preferred account flag indicates the consumer’s preferred choice of bank account used to fund bill payment transactions. True if it is a preferred account. |
| isBusiness | Req | body | boolean | Indicates if the account is a business account. True if it is a business account. |
| isBilling | Cond | body | boolean | Establishes whether this account is the account from which user/subscriber billing fees (if billed by Fiserv) will be debited. When moving the billing identifier from one bank account to another, set the IsBilling flag to “true” on the desired account. <br> Valid values: <br> true (set this account to be the account from which billing fees are debited) <br> false (do not debit billing fees from this account) <br> Condition: Required if Fiserv is billing the user on behalf of the Sponsor/Tenant. |
| businessName | Cond | body | string | When isBusiness is true, this is the name of the business. Max length: 40 <br> Pattern: ^[a-zA-Z0-9_(){}&amp;@!+#.'$,%^ \*-]\* |
| primaryAccountOwner | Opt | body | string | Name of the primary owner of this account. If not provided, this value defaults to the consumer’s name. <br> Length: 1-35 <br> Pattern: ^[a-zA-Z0-9_(){}&amp;@!+#.'$,%^ \*-]\* |
| secondaryAccountOwner | Opt | body | string | Name of the secondary owner of this account, if applicable. <br> Length: 1-35 <br> Pattern: ^[a-zA-Z0-9_(){}&amp;@!+#.'$,%^ \*-]\* |
| checkPrintAddress | Opt | body | [USAddress](./complexObjects.md#usaddress) | The U.S. address printed on the checks. If provided, this is the consumer’s address to be printed in the upper left corner of the check. |
| bankingOptions | Opt | body | [BankingOptions](./complexObjects.md#bankingoptions) | Banking options when banking service is enabled. |

### Response

| Parameter | Req  | Data Type                 | Description                                                        |
|------------|-----|--------|------------------------------------------------|
| data      | Cond | [BaseModel](./complexObjects.md#basemodel)   | Response data. Condition: Always returned for successful response. |
| result    | Req  | [ResultType](./complexObjects.md#resulttype) | Result Information.                                                |

### Sample API Usage

#### Request URL

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/users/b188b172fb7fe911a83600505689ef1f/bankAccounts |
|------------------------------------------------------------------------|

#### Request
```json
{
"routingTransitNumber": "122235821",
"accountNumber": "569944550459",
"accountType": "Checking",
"nickname": "arj",
"startingCheckNumber": "014",
"isPreferred": true,
"isBusiness": false,
"isBilling": true,
"businessName": null,
"primaryAccountOwner": "arjun",
"secondaryAccountOwner": "ar",
"checkPrintAddress": {
"address1": "6883 Made Up St",
"address2": "123 n central",
"address3": null,
"city": "Dublin",
"state": "CA",
"zipCode": "45456"
}
}
```
#### Response
```json
{
"data": {
"self":
"/api/v1/me/users/b188b172fb7fe911a83600505689ef1f/bankAccounts/5edc4d812d5d4ecbbacf73a1e0a465c2",
"id": "5edc4d812d5d4ecbbacf73a1e0a465c2"
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
## Update a Bank Account for a Managed User

The BankAccounts Patch API enables modifying a bank account that is a
funding account for a managed user. The fields that can be modified are
limited. The calling application should only pass those attributes in
the request that are required to be updated.

Note

For the authentication request, the client\_id must be configured for
UserMaintenance. Do not send fiserv.identity.billpay.subscriberId; see
[Sample API Usage: Authentication Request for User
Maintenance](../resourcesAndGuides/authenticate.md#sample-api-usage-authentication-request-for-user-maintenance).

Because the maintenance grant is scoped to a tenant, the user ID for the
consumer must be provided in the BankAccounts Patch request.

### Method and Endpoint

| PATCH | /api/v1/me/users/{userId}/bankAccounts/{bankAccountCommonId}|
|--------|--------------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| userId | Req | path | string | Identifier for the managed user. |
| bankAccountCommonId | Req | path | string | Identifier for the bank account. |
| idType | Opt | query | string | Identifies the user ID type. Valid values: SubscriberId, ExternalSubscriberId, CheckFreeNextUserId <br> CheckFreeNextUserId is the default. | 
| nickname | Opt | body | string | A description of the bank account used to help identify it in a list. Length: 1–30 <br> Pattern: ^[a-zA-Z0-9_(){}&amp;@!+#.'$,%^ \*-]\* <br>Do not enter any sensitive information such as the account number. |
| startingCheckNumber | Opt | body | string | Starting check number for paper drafts. Length: 1-9 <br> Pattern: ^[0-9]\* |
| isPreferred | Opt | body | boolean | Indicates if the account is the preferred account. The preferred account flag indicates the consumer’s preferred choice of bank account used to fund bill payment transactions. True if it is a preferred account. <br> Cannot be changed to “false”. By making another account the preferred account, isPreferred will become false for this account. |
| isBilling | Cond | body | boolean | Establishes whether this account is the account from which user/subscriber billing fees (if billed by Fiserv) will be debited. When moving the billing identifier from one bank account to another, set the IsBilling flag to “true” on the desired account. <br> Valid values: <br> true (set this account to be the account from which billing fees are debited) <br> false (do not debit billing fees from this account) <br> Condition: Required if Fiserv is billing the user on behalf of the Sponsor/Tenant. | 
| isBusiness | Opt | body | boolean | Indicates if the account is a business account. True if it is a business account. |
| businessName | Cond | body | string | When isBusiness is true, this is the name of the business. Cannot have a value when isBusiness is “false”. Max length: 40 <br> Pattern: ^[a-zA-Z0-9_(){}&amp;@!+#.'$,%^ \*-]\* |
| primaryAccountOwner | Opt | body | string | Name of the primary owner of this account. Length: 1-35 <br> Pattern: ^[a-zA-Z0-9_(){}&amp;@!+#.'$,%^ \*-]\* <br>
| secondaryAccountOwner | Opt | body | string | Name of the secondary owner of this account, if applicable. Length: 1-35 <br> Pattern: ^[a-zA-Z0-9_(){}&amp;@!+#.'$,%^ \*-]\* |
| checkPrintAddress | Opt | body | [USAddress](./complexObjects.md#usaddress) | The U.S. address printed on the checks. If provided, this is the consumer’s address to be printed in the upper left corner of the check. |
| bankingOptions | Opt | body | [BankingOptions](./complexObjects.md#bankingoptions) | Banking options when banking service is enabled. |

### Response

| Parameter | Req  | Data Type                 | Description                                                                                                                  |
|-------------|-----|-------|-------------------------------------------------|
| result    | Cond | [ResultType](./complexObjects.md#resulttype) | Result information. Condition: Only returned when the request fails. No content returned for success (HTTP status code 204). |

### Sample API Usage

#### Request URL

https://api-checkfreenext-cert.fiservapps.com/api/v1/me/users/
f16c5320ff7fe911a83600505689ef1f/bankAccounts/ef8017e00843437b8848511979d8071f

#### Request Body
```json
{
"nickname": "bankaccount",
"startingCheckNumber": "778",
"isBusiness": true,
"isBilling": true,
"businessName": "new business",
"primaryAccountOwner": "arjkkkk",
"secondaryAccountOwner": "noone" }
```
#### Response

| 204 No Content |
|----------------|

## Update a Bank Account for a Consumer

The BankAccounts Patch API enables modifying a bank account that is a
funding account for the user associated with the issued bearer token.
The fields that can be modified are limited. The calling application
should only pass those attributes in the request that are required to be
updated.

### Method and Endpoint

| PATCH | /api/v1/me/bankAccounts/{bankAccountCommonId}|
|--------|--------------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| bankAccountCommonId | Req | path | string | Identifier for the bank account. |
| nickname | Opt | body | string | A description of the bank account used to help identify it in a list. Length: 1–30 <br> Pattern: ^[a-zA-Z0-9_(){}&amp;@!+#.'$,%^ \*-]\* <br> Do not enter any sensitive information such as the account number. |
| startingCheckNumber | Opt | body | string | Starting check number for paper drafts. Length: 1-9 <br> Pattern: ^[0-9]\* |
| isPreferred | Opt | body | boolean | Indicates if the account is the preferred account. The preferred account flag indicates the consumer’s preferred choice of bank account used to fund bill payment transactions. True if it is a preferred account. <br> Cannot be changed to “false”. By making another account the preferred account, isPreferred will become false for this account. |
| isBilling | Cond | body | boolean | Establishes whether this account is the account from which user/subscriber billing fees (if billed by Fiserv) will be debited. When moving the billing identifier from one bank account to another, set the IsBilling flag to “true” on the desired account. <br> Valid values: <br> true (set this account to be the account from which billing fees are debited) <br> false (do not debit billing fees from this account) <br> Condition: Required if Fiserv is billing the user on behalf of the Sponsor/Tenant. |
| isBusiness | Opt | body | boolean | Indicates if the account is a business account. True if it is a business account. |
| businessName | Cond | body | string | When isBusiness is true, this is the name of the business. Cannot have a value when isBusiness is “false”. Max length: 40 <br> Pattern: ^[a-zA-Z0-9_(){}&amp;@!+#.'$,%^ \*-]\* |
| primaryAccountOwner | Opt | body | string | Name of the primary owner of this account. Length: 1-35 <br> Pattern: ^[a-zA-Z0-9_(){}&amp;@!+#.'$,%^ \*-]\* |
| secondaryAccountOwner | Opt | body | string | Name of the secondary owner of this account, if applicable. Length: 1-35 <br> Pattern: ^[a-zA-Z0-9_(){}&amp;@!+#.'$,%^ \*-]\* |
| checkPrintAddress | Opt | body | [USAddress](./complexObjects.md#usaddress) | The U.S. address printed on the checks. If provided, this is the consumer’s address to be printed in the upper left corner of the check. |
| bankingOptions | Opt | body | [BankingOptions](./complexObjects.md#bankingoptions) | Banking options when banking service is enabled. |

### Response

| Parameter | Req  | Data Type                 | Description                                                                                                                  |
|-------------|-----|-------|-------------------------------------------------|
| result    | Cond | [ResultType](./complexObjects.md#resulttype) | Result information. Condition: Only returned when the request fails. No content returned for success (HTTP status code 204). |

### Sample API Usage

#### Request URL

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/bankAccounts/743ce5de4b01468fab0c518cc301f2c4 |
|------------------------------------------------------------------------|

#### Request Body
```json
{
"nickname": "bankaccount",
"startingCheckNumber": "778",
"isBusiness": true,
"isBilling": true,
"businessName": "new business",
"primaryAccountOwner": "arjkkkk",
"secondaryAccountOwner": "noone" }
```
#### Response

| 204 No Content |
|----------------|

## Delete a Bank Account for a Managed User

The BankAccounts Delete API enables deleting a bank account that is a
funding account for a managed user.

Note

For the authentication request, the client\_id must be configured for
UserMaintenance. Do not send fiserv.identity.billpay.subscriberId; see
[Sample API Usage: Authentication Request for User
Maintenance](../resourcesAndGuides/authenticate.md#sample-api-usage-authentication-request-for-user-maintenance).

Because the maintenance grant is scoped to a tenant, the user ID for the
consumer must be provided in the BankAccounts Delete request.

### Method and Endpoint

| DELETE | /api/v1/me/users/{userId}/bankAccounts/{bankAccountCommonId}|
|--------|----------------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| userId | Req | path | string | Identifier for the managed user. |
| bankAccountCommonId | Req | path | string | Identifier for the bank account. |
| idType | Opt | query | string | Identifies the user ID type. Valid values: SubscriberId, ExternalSubscriberId, CheckFreeNextUserId <br> CheckFreeNextUserId is the default. |

### Response

| Parameter | Req  | Data Type                 | Description                                                                                                                                  |
|------------|------|-------|-----------------------------------------------|
| result    | Cond | [ResultType](./complexObjects.md#resulttype) | Result associated with the request. Condition: Only returned when the request fails. No content returned for success (HTTP status code 204). |

### Sample API Usage

#### Request URL

https://api-checkfreenext-cert.fiservapps.com/api/v1/me/users/e31cb7a57466e911a83500505689ef1f/bankAccounts/
60411aefbaff493d8a89b86fd1cee178

#### Response

| 204 No Content |
|----------------|

## Get an Unmasked Bank Account Number (tenant scoped)

This API enables retrieving the unmasked account number for a bank account for a given tenant/user combination.

### Method and Endpoint

| GET | /api/v1/me/users/{userId}/bankAccounts/{bankCommonId}/accountNumber |
|-----|----------------------------|

### Request

| Parameter    | Req | Param Type | Data Type | Description                      |
|--------------|-----|------------|-----------|----------------------------------|
| userId       | Req | path       | string    | Identifier for the managed user. |
| bankCommonId | Req | path       | string    | Identifier for the bank account. |
| idType       | Opt | query      | string    | Identifies the user ID type. Valid values: SubscriberId, ExternalSubscriberId, CheckFreeNextUserId <br> CheckFreeNextUserId is the default. |


### Response

| Parameter | Req  | Data Type                 | Description                                                                                                                                  |
|------------|------|-------|-----------------------------------------------|
| data      | Req  | string                    | Unmasked account number of the bank account. There is an empty string if there is no data to return.                                         |
| result    | Cond | [ResultType](./complexObjects.md#resulttype) | Result associated with the request. |

### Sample API Usage

#### Request URL

https://api-checkfreenext-cert.fiservapps.com/api/v1/me/users/
f16c5320ff7fe911a83600505689ef1f/bankAccounts/ef8017e00843437b8848511979d8071f/accountNumber


#### Response
```json
{
"data": "834756568556245",
"result": {
"success": true,
"resultInfo": []
}
}
```

## Get an Unmasked Bank Account Number (consumer scoped)

This API enables retrieving the unmasked account number for a bank
account.

The parameter **unmaskedAccountNumberUri** (returned for [Get Bank
Accounts for a Managed User](#get-bank-accounts-for-managed-user-list),
[Get Bank Accounts for a
Consumer](#get-bank-accounts-for-a-consumer-list), [Get Bank Account for
a Managed User](#get-a-bank-account-for-a-managed-user-single), and [Get
Bank Account for a Consumer](#get-a-bank-account-for-a-consumer-single))
returns the endpoint shown below.

### Method and Endpoint

| GET | /api/v1/me/bankAccounts/{bankCommonId}/accountNumber |
|-----|----------------------------|

### Request

| Parameter    | Req | Param Type | Data Type | Description                      |
|--------------|-----|------------|-----------|----------------------------------|
| bankCommonId | Req | path       | string    | Identifier for the bank account. |

### Response

| Parameter | Req  | Data Type                 | Description                                                                                                                                  |
|------------|------|-------|-----------------------------------------------|
| data      | Req  | string                    | Unmasked account number of the bank account. There is an empty string if there is no data to return.                                         |
| result    | Cond | [ResultType](./complexObjects.md#resulttype) | Result associated with the request. Condition: Only returned when the request fails. No content returned for success (HTTP status code 200). |
