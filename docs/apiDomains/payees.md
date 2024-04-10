# Payees

Any business or biller the consumer intends to pay through Bill Pay must
be added as a payee before the payment can be scheduled. This set of
APIs provides access to a payee resource and enables the UI to implement
payee add, modify, delete, and view.

-   [Get a list of payees](#get-payees-list)

-   [Get a specific payee](#get-payee-single)

-   [Add a payee](#add-a-payee)

-   [Update a payee](#update-a-payee)

-   [Delete a payee](#delete-a-payee)

-   [Get an unmasked payee account number](#get-a-payees-unmasked-account-number)

-   [Get a list of automatic transaction options for a payee](#get-automatic-transaction-options)

## Get Payees (List)

The Payees Get API returns the list of payees added by the consumer.

### Method and Endpoint

| GET | /api/v1/me/payees |
|-----|-------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| returnInactivePayees | Opt | query | boolean | Indicates whether to return inactive payees in the response. <br> True - Return inactive payees <br> False - Do not return inactive payees. This is the default. |

### Response

| Parameter | Req | Data Type                 | Description         |
|-----------|-----|---------------------------|---------------------|
| data      | Req | [PayeeList](./complexObjects.md#payeelist)   | List of payees.     |
| result    | Req | [ResultType](./complexObjects.md#resulttype) | Result information. |

### Sample API Usage

#### Response - Freeform Payee
```json
{
"data": {
"payees": [
{
"name": "Johnny Exampleman",
"category": "Uncategorized",
"contactPhoneNumber": "4024375544",
"address": {
"address1": "123 Easy St.",
"city": "Athens",
"state": "GA",
"zipCode": "30601"
},
"lastUsedFundingAccountUri":
"/api/v1/me/bankAccounts/e16324ed66d04f1ea9fd27f391a138b5",
"ebillServiceActivationType": "None",
"payeeLogoUrl":
"https://logos-checkfreenext-cert.fiservapps.com/DefaultPerson.png",
"payeeLogoGeneric": true,
"modifiableFields": [
"accountNumber",
"nickname",
"contactPhoneNumber",
"name",
"address",
"emailAddress",
"mobilePhone",
"overnightAddress",
"bankAccount"
],
"status": "Active",
"earliestStandardTransactionDate":"2022-09-26",
"legacyPayeeId": "2",
"ebillServiceActivationStatus": "NotApplicable",
"billTransactionScheduleActive": false,
"billTransactionScheduleCapable": false,
"recurringTransactionScheduleActive": false,
"isPaperTransactionsEnabled": true,
"isRushDeliveryAvailable": false,
"self": "/api/v1/me/payees/be8d81188f26449697068f572a62eced",
"id": "be8d81188f26449697068f572a62eced"
}
]
},
"result": {
"success": true,
"resultInfo": []
}
}
```
#### Response - Overnight Payee
```json
{
"data": {
"payees": [
{
"name": "Johnny Freeform",
"category": "Uncategorized",
"contactPhoneNumber": "9124367112",
"address": {
"address1": "123 Easy St.",
"city": "Athens",
"state": "GA",
"zipCode": "30601"
},
"overnightAddress": {
"address1": "2900 Westside Pkwy",
"city": "Alpharetta",
"state": "GA",
"zipCode": "30004"
},
"lastUsedFundingAccountUri":
"/api/v1/me/bankAccounts/e16324ed66d04f1ea9fd27f391a138b5",
"ebillServiceActivationType": "None",
"payeeLogoUrl": "https://logos-checkfreenext-cert.fiservapps.com/DefaultPerson.png",
"payeeLogoGeneric": true,
"modifiableFields": [
"accountNumber",
"nickname",
"contactPhoneNumber",
"name",
"address",
"emailAddress",
"mobilePhone",
"overnightAddress",
"bankAccount"
],
"status": "Active",
"earliestStandardTransactionDate": "2022-09-26",
"legacyPayeeId": "3",
"ebillServiceActivationStatus": "NotApplicable",
"billTransactionScheduleActive": false,
"billTransactionScheduleCapable": false,
"recurringTransactionScheduleActive": false,
"isPaperTransactionsEnabled": true,
"isRushDeliveryAvailable": true,
"self": "/api/v1/me/payees/2df8081131694034a9237855e4f5961d",
"id": "2df8081131694034a9237855e4f5961d"
}
]
},
"result": {
"success": true,
"resultInfo": []
}
}
```
#### Response - E-bill Capable Merchant

This type of response will have an ebillServiceActivationType of Lite or
Full as well as having the ebillServiceActivationUri to get any
additional details the biller requires of the user for activation. The
presence of the ebillServiceActivationUri means that the user has not
yet activated e-bills.
```json
{
"data": {
"payees": [
{
"name": "Boston Edison",
"maskedAccountNumber": "********3111",
"unmaskedAccountNumberUri":
"/api/v1/me/payees/afa28534bea54a00b23da127380087b4/payeeAccountNumber",
"category": "Utilities",
"contactPhoneNumber": "800.592.2000",
"merchantUri":
"/api/v1/merchants/8543d3a22a5e49b9977ba9cd546fcd1b",
"lastUsedFundingAccountUri":
"/api/v1/me/bankAccounts/696cd89ebc564981bea7f148f3628fed",
"ebillServiceActivationUri":
"/api/v1/me/payees/afa28534bea54a00b23da127380087b4/EbillCapability",
"ebillServiceActivationType": "Lite",
"payeeLogoUrl":
"https://logos-checkfreenext-cert.fiservapps.com/7163.png",
"payeeLogoGeneric": false,
"modifiableFields": [
"accountNumber",
"nickname",
"contactPhoneNumber"
],
"status": "Active",
"earliestStandardTransactionDate": "2022-09-20",
"legacyPayeeId": "4",
"ebillServiceActivationStatus": "Capable",
"billTransactionScheduleActive": false,
"billTransactionScheduleCapable": false,
"recurringTransactionScheduleActive": false,
"isPaperTransactionsEnabled": false,
"isRushDeliveryAvailable": false,
"self": "/api/v1/me/payees/afa28534bea54a00b23da127380087b4",
"id": "afa28534bea54a00b23da127380087b4"
}
]
},
"result": {
"success": true,
"resultInfo": []
}
}
```
#### Response - E-bill Activated Merchant

Here is the same payee (shown in the previous example) after e-bill
activation. The ebillServiceActivationUri is no longer present and
instead the ebillServiceUri is returned.
```json
{
"data": {
"payees": [
{
"name": "Boston Edison",
"maskedAccountNumber": "********3111",
"unmaskedAccountNumberUri":
"/api/v1/me/payees/afa28534bea54a00b23da127380087b4/payeeAccountNumber",
"category": "Utilities",
"contactPhoneNumber": "800.592.2000",
"merchantUri":
"/api/v1/merchants/8543d3a22a5e49b9977ba9cd546fcd1b",
"lastUsedFundingAccountUri":
"/api/v1/me/bankAccounts/696cd89ebc564981bea7f148f3628fed",
"ebillServiceActivationType": "Lite",
"ebillServiceUri":
"/api/v1/me/payees/afa28534bea54a00b23da127380087b4/EbillService",
"payeeLogoUrl":
"https://logos-checkfreenext-cert.fiservapps.com/7163.png",
"payeeLogoGeneric": false,
"modifiableFields": [
"nickname"
],
"status": "Active",
"earliestStandardTransactionDate": "2022-09-20",
"legacyPayeeId": "4",
"ebillServiceActivationStatus": "Active",
"billTransactionScheduleActive": false,
"billTransactionScheduleCapable": true,
"recurringTransactionScheduleActive": false,
"isPaperTransactionsEnabled": false,
"isRushDeliveryAvailable": false,
"self": "/api/v1/me/payees/afa28534bea54a00b23da127380087b4",
"id": "afa28534bea54a00b23da127380087b4"
}
]
},
"result": {
"success": true,
"resultInfo": []
}
}
```
#### Response - AutoPay Activated Merchant

Here is the same payee (shown in the previous two examples) after
AutoPay activation. The response now includes an array of automatic
transaction URIs that the client can call to get more information about
the AutoPay setup.
```json
{
"data": {
"payees": [
{
"name": "Boston Edison",
"maskedAccountNumber": "********3111",
"unmaskedAccountNumberUri":
"/api/v1/me/payees/afa28534bea54a00b23da127380087b4/payeeAccountNumber",
"category": "Utilities",
"contactPhoneNumber": "800.592.2000",
"merchantUri":
"/api/v1/merchants/8543d3a22a5e49b9977ba9cd546fcd1b",
"automaticTransactionUris": [
"/api/v1/me/automaticTransactions/25e08b3868d14c6298e67cb75a32c6cd"
],
"lastUsedFundingAccountUri":
"/api/v1/me/bankAccounts/696cd89ebc564981bea7f148f3628fed",
"ebillServiceActivationType": "Lite",
"ebillServiceUri":
"/api/v1/me/payees/afa28534bea54a00b23da127380087b4/EbillService",
"payeeLogoUrl":
"https://logos-checkfreenext-cert.fiservapps.com/7163.png",
"payeeLogoGeneric": false,
"modifiableFields": [
"nickname"
],
"status": "Active",
"earliestStandardTransactionDate": "2022-09-20",
"legacyPayeeId": "4",
"ebillServiceActivationStatus": "Active",
"billTransactionScheduleActive": true,
"billTransactionScheduleCapable": true,
"recurringTransactionScheduleActive": false,
"isPaperTransactionsEnabled": false,
"isRushDeliveryAvailable": false,
"self": "/api/v1/me/payees/afa28534bea54a00b23da127380087b4",
"id": "afa28534bea54a00b23da127380087b4"
}
]
},
"result": {
"success": true,
"resultInfo": []
}
}
```
#### Request URL - Inactive Payees

A returnInactivePayees query parameter can be provided to also see
inactive payees in the list.

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/payees?returnInactivePayees=True |
|------------------------------------------------------------------------|

#### Response - Inactive Payees
```json
{
"data": {
"payees": [
{
"name": "Boston Edison",
"maskedAccountNumber": "********3111",
"unmaskedAccountNumberUri":
"/api/v1/me/payees/afa28534bea54a00b23da127380087b4/payeeAccountNumber",
"category": "Utilities",
"contactPhoneNumber": "800.592.2000",
"merchantUri":
"/api/v1/merchants/8543d3a22a5e49b9977ba9cd546fcd1b",
"lastUsedFundingAccountUri":
"/api/v1/me/bankAccounts/696cd89ebc564981bea7f148f3628fed",
"ebillServiceActivationType": "Lite",
"ebillServiceUri":
"/api/v1/me/payees/afa28534bea54a00b23da127380087b4/EbillService",
"payeeLogoUrl":
"https://logos-checkfreenext-cert.fiservapps.com/7163.png",
"payeeLogoGeneric": false,
"modifiableFields": [
"nickname"
],
"status": "Active",
"earliestStandardTransactionDate": "2022-09-20",
"legacyPayeeId": "4",
"ebillServiceActivationStatus": "Active",
"billTransactionScheduleActive": false,
"billTransactionScheduleCapable": false,
"recurringTransactionScheduleActive": false,
"isPaperTransactionsEnabled": false,
"isRushDeliveryAvailable": false,
"self": "/api/v1/me/payees/afa28534bea54a00b23da127380087b4",
"id": "afa28534bea54a00b23da127380087b4"
},
{
"name": "ALLSTATE INSURANCE S&amp;P",
"maskedAccountNumber": "*********4252",
"unmaskedAccountNumberUri":
"/api/v1/me/payees/b2a884c85d8c41489ee3c55368b47ff9/payeeAccountNumber",
"category": "Insurance",
"contactPhoneNumber": "5558675309",
"address": {
"address1": "75 EXECUTIVE PKWY",
"city": "HUDSON",
"state": "OH",
"zipCode": "44237"
},
"lastUsedFundingAccountUri":
"/api/v1/me/bankAccounts/696cd89ebc564981bea7f148f3628fed",
"ebillServiceActivationType": "None",
"payeeLogoUrl":
"https://logos-checkfreenext-cert.fiservapps.com/DefaultCompany.png",
"payeeLogoGeneric": true,
"modifiableFields": [
"accountNumber",
"nickname",
"contactPhoneNumber",
"name",
"address",
"emailAddress",
"mobilePhone",
"overnightAddress",
"bankAccount"
],
"status": "Inactive",
"earliestStandardTransactionDate": "2022-09-26",
"legacyPayeeId": "5",
"ebillServiceActivationStatus": "NotApplicable",
"billTransactionScheduleActive": false,
"billTransactionScheduleCapable": false,
"recurringTransactionScheduleActive": false,
"isPaperTransactionsEnabled": true,
"isRushDeliveryAvailable": true,
"self": "/api/v1/me/payees/b2a884c85d8c41489ee3c55368b47ff9",
"id": "b2a884c85d8c41489ee3c55368b47ff9"
}
]
},
"result": {
"success": true,
"resultInfo": []
}
}
```
## Get Payee (Single)

The Payees Get API returns a specific payee added by the consumer.

### Method and Endpoint

| GET | /api/v1/me/payees/{payeeId} |
|-----|-----------------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description                         |
|-----------|-----|------------|-----------|-------------------------------------|
| payeeId   | Req | path       | string    | Identifier for the potential payee. |

### Response

| Parameter | Req | Data Type                 | Description                  |
|-----------|-----|---------------------------|------------------------------|
| data      | Req | [Payee](./complexObjects.md#payee)           | Information about the payee. |
| result    | Req | [ResultType](./complexObjects.md#resulttype) | Result information.          |

### Sample API Usage

#### Request URL

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/payees/afa28534bea54a00b23da127380087b4 |
|------------------------------------------------------------------------|

#### Response
```json
{
"data": {
"name": "Boston Edison",
"maskedAccountNumber": "********3111",
"unmaskedAccountNumberUri":
"/api/v1/me/payees/afa28534bea54a00b23da127380087b4/payeeAccountNumber",
"category": "Utilities",
"contactPhoneNumber": "800.592.2000",
"merchantUri":
"/api/v1/merchants/8543d3a22a5e49b9977ba9cd546fcd1b",
"automaticTransactionUris": [
"/api/v1/me/automaticTransactions/25e08b3868d14c6298e67cb75a32c6cd"
],
"lastUsedFundingAccountUri":
"/api/v1/me/bankAccounts/696cd89ebc564981bea7f148f3628fed",
"ebillServiceActivationType": "Lite",
"ebillServiceUri":
"/api/v1/me/payees/afa28534bea54a00b23da127380087b4/EbillService",
"payeeLogoUrl":
"https://logos-checkfreenext-cert.fiservapps.com/7163.png",
"payeeLogoGeneric": false,
"modifiableFields": [
"nickname"
],
"status": "Active",
"earliestStandardTransactionDate": "2022-09-20",
"legacyPayeeId": "4",
"ebillServiceActivationStatus": "Active",
"billTransactionScheduleActive": true,
"billTransactionScheduleCapable": true,
"recurringTransactionScheduleActive": false,
"isPaperTransactionsEnabled": false,
"isRushDeliveryAvailable": false,
"self": "/api/v1/me/payees/afa28534bea54a00b23da127380087b4",
"id": "afa28534bea54a00b23da127380087b4"
},
"result": {
"success": true,
"resultInfo": []
}
}
```
## Add a Payee

The Payee Post API enables adding a new payee to a consumer's payee
list, so that payment transactions can be made to that payee.

### Method and Endpoint

| POST | /api/v1/me/payees |
|------|-------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| overrideAddressValidation | Opt | query | boolean | Indicates whether address validation (city, state, ZIP Code, address combination) must be done or can be skipped. <br> True - Address validation will be skipped. <br> False - Address validation will be done. This is the default. |
| payeeInfo | Req | body | [PayeeInfo](./complexObjects.md#payeeinfo) | Payee information. |
| sourceUri | Cond | body | string | The URI from Potential Payees/ Merchant Search/ Payees / ProbableMerchants. <br> Blank for an unmanaged merchant.<br> Condition: If adding a payee by providing only name, account number, and sourceUri, this is required. (Fiserv must already have a relationship with the merchant.) |
| sourceUriPayeeZipCode | Cond | body | string | The payee ZIP Code. This is the ZIP Code that the consumer sees on their bill for the merchant. <br> Conditions: If the /api/v1/merchants/Search API returns a value of true for merchantZipRequired, this is required. If the /api/v1/me/probableMerchants API returns a value of true for merchantZipRequired, this is required. <br> Pattern: ^(\d{5}\|\d{9}\|\d{11})$ <br> Valid characters: 0-9 <br> Parsed: Chars 1-5 = Zip5, Chars 6-9 = Zip4, Chars 10-11 = Zip2 |

### Response

| Parameter | Req  | Data Type                 | Description                                                        |
|------------|-----|--------|------------------------------------------------|
| data      | Cond | [BaseModel](./complexObjects.md#basemodel)   | Response data. Condition: Always returned for successful response. |
| result    | Req  | [ResultType](./complexObjects.md#resulttype) | Result information.                                                |

### Sample API Usage - Freeform Payee

This type of request will be used when the consumer is paying an entity
that Fiserv has no established relationship with. Name, address, and
contact phone number are required.

#### Request Body
```json
{
"payeeInfo": {
"name": "Johnny Freeform",
"address": {
"address1": "123 Easy St.",
"address2": null,
"city": "Athens",
"state": "GA",
"zipCode": "30601"
},
"contactPhoneNumber": "7634633167"
},
"sourceUri": null,
"sourceUriPayeeZipcode": null
}
```
#### Response
```json
{
"data": {
"self": "/api/v1/me/payees/fb4abe7b19ad4fbea0ed76689844d581",
"id": "fb4abe7b19ad4fbea0ed76689844d581"
},
"result": {
"success": true,
"resultInfo": []
}
}
```
### Sample API Usage - Overnight Payee

Some payees use a separate overnight address that can mail the payment
faster. Name, address, and contact phone number are still required.

#### Request Body
```json
{
"payeeInfo": {
"name": "Johnny Freeform",
"address": {
"address1": "123 Easy St.",
"address2": null,
"city": "Athens",
"state": "GA",
"zipCode": "30601"
},
"overnightAddress": {
"address1": "2900 Westside Pkwy",
"address2": null,
"city": "Alpharetta",
"state": "GA",
"zipCode": "30004"
},
"contactPhoneNumber": "9124367112"
},
"sourceUri": null,
"sourceUriPayeeZipcode": null
}
```
#### Response
```json
{
"data": {
"self": "/api/v1/me/payees/2df8081131694034a9237855e4f5961d",
"id": "2df8081131694034a9237855e4f5961d"
},
"result": {
"success": true,
"resultInfo": []
}
}
```
### Sample API Usage - Merchant Payee

When Fiserv has a relationship with a business that a consumer intends
to pay, the POST request for the payee can be simplified. In this
scenario, only name, accountNumber, and sourceUri are required. The
sourceUriPayeeZipcode is conditionally required. The value for sourceUri
and the value of merchantZipRequired (which indicates if
sourceUriPayeeZipcode is required) are obtained from the
/api/v1/merchants/Search API. The account number must be a valid schemed
number for the merchant.

#### Request Body
```json
{
"payeeInfo": {
"name": "Bay State Gas",
"accountNumber": "262436773325"
},
"sourceUri":
"/api/v1/merchants/1d7bc8609f424f30864695cb9a92122b",
"sourceUriPayeeZipcode": null
}
```
#### Response
```json
{
"data": {
"self": "/api/v1/me/payees/776c099ffdf340e28c4e2f7f0bddb73a",
"id": "776c099ffdf340e28c4e2f7f0bddb73a"
},
"result": {
"success": true,
"resultInfo": []
}
}
```
## Update a Payee

The Payee Patch API enables modifying an existing payee on a consumer's
payee list. This is typically done to update information (such as
address) for a payee if that information has changed. Future payments to
that payee are processed using the modified information. It is
recommended to send only those attributes in the request body that
require updates.

### Method and Endpoint

| PATCH | /api/v1/me/payees/{payeeId} |
|-------|----------------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| payeeId | Req | path | string | Identifier for the payee. |
| modifyPendingPayments | Opt | query | boolean | Indicates if the payee modification should be propagated to pending payments. Valid values: <br> true - Yes, propagate payee modification to pending payments. <br> false - No, do not propagate payee modification to pending payments. This is the default. |
| name | Opt | body | string | The name of the payee. <br> Pattern: ^\[\x20-\x5A\x5C\x5F-\x7E\]+$ <br> Length: 2-32 |
| nickname | Opt | body | string | The nickname of the payee. Length: 0-30 <br> Pattern: ^\[\x20\x2C-\x2E\x30-\x39\x41-\x5A\x61-\x7A\r\n\]+$ |
| accountNumber | Opt | body | string | The consumer's account number with the payee. Length: 1-32 <br> Pattern: ^\[a-zA-Z0-9 !"#\$%&amp;-\]{1,32}$ |
| contactPhoneNumber | Opt | body | string | Phone number used to contact the payee if there are issues posting the payment. Length: 10-12 <br> Pattern:(^\[0-9\]{10}\$)\|(^\(?\[0-9\]{3}\\)?-\[0-9\]{3}-\[0-9\]{4}\$)\|(^\\(?\[0-9\]{3}\\)?\\s?\[0-9\]{3}-\[0-9\]{4}\$) <br> Must be numeric and may contain a dash or space between the numbers at the appropriate placement. The phone number must be valid based on the North American Numbering Plan (for example, the area code cannot begin with a 0 or 1). Example: 234-555-1212 | 
| address | Opt | body | [USAddress](./complexObjects.md#usaddress) | Payee address information. | 
| overnightAddress | Opt | body | [USAddress](./complexObjects.md#usaddress) | Address for overnight payments if different from the address. | 
| socialTokens | Opt | body | Array of SocialToken | Reserved for future use. |
| accountTokens | Opt | body | Array of [AccountTokenAddInfo](./complexObjects.md#accounttokenaddinfo) | Reserved for future use. | 

### Response

| Parameter | Req  | Data Type                 | Description                                                                                                                                  |
|-------------|-----|-------|-------------------------------------------------|
| result    | Cond | [ResultType](./complexObjects.md#resulttype) | Result associated with the request. Condition: Only returned when the request fails. No content returned for success (HTTP status code 204). |

### Sample API Usage

#### Request URL

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/payees/a955d07eb5ce4bc98f0e1981979dd7a1 |
|------------------------------------------------------------------------|

#### Request Body
```json
{
"name": "Ohio water",
"nickname": "water",
"accountNumber": "98735422698",
"contactPhoneNumber": "6146986345",
"address": {
"address1": "6000 Perimeter Drive",
"address2": null,
"city": "Dublin",
"state": "OH",
"zipCode": "43017"
},
"overnightAddress": {
"address1": "6000 Perimeter Drive",
"address2": null,
"city": "Dublin",
"state": "OH",
"zipCode": "43017"
}
}
```
#### Response

| 204 No Content |
|----------------|

#### Request URL - Propagate to Pending Payments

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>https://api-checkfreenext-cert.fiservapps.com/api/v1/me/payees/a955d07eb5ce4bc98f0e1981979dd7a1?<br />
modifyPendingPayments=true</th>
</tr>
</thead>
<tbody>
</tbody>
</table>

#### Request Body - Propagate to Pending Payments
```json
{
"name": "Ohio water",
"nickname": "water",
"accountNumber": "98735422698",
"contactPhoneNumber": "6146986345",
"address": {
"address1": "6000 Perimeter Drive",
"address2": null,
"city": "Dublin",
"state": "OH",
"zipCode": "43017"
},
"overnightAddress": {
"address1": "6000 Perimeter Drive",
"address2": null,
"city": "Dublin",
"state": "OH",
"zipCode": "43017"
}
}
```
#### Response - Propagate to Pending Payments

| 204 No Content |
|----------------|

## Delete a Payee

This API enables deleting a payee from a consumer's existing list of
payees. Deleting a payee will cancel any e-bill service and any
automatic payment plans associated with the payee.

### Method and Endpoint

| DELETE | /api/v1/me/payees/{payeeId}|
|--------|----------------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| payeeId | Req | path | string | Identifier for the payee. | 
| cancelPendingTransactions | Opt | query | boolean | Indicates if pending transactions to the payee should be canceled. Valid values: <br> true - Yes, cancel pending transactions. <br> false - No, do not cancel pending transactions. This is the default. |

### Response

| Parameter | Req  | Data Type                 | Description                                                                                                                                  |
|------------|------|-------|-----------------------------------------------|
| result    | Cond | [ResultType](./complexObjects.md#resulttype) | Result associated with the request. Condition: Only returned when the request fails. No content returned for success (HTTP status code 204). |

### Sample API Usage

#### Request URL - Keep Pending Payments

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/payees/c31c83340195458488da45c5e4b83085 |
|------------------------------------------------------------------------|

#### Response - Keep Pending Payments

| 204 No Content |
|----------------|

#### Request URL - Cancel Pending Payments

https://api-checkfreenext-cert.fiservapps.com/api/v1/me/payees/10715add606a48b7a1dfd77fae70e0e4?cancelPendingTransactions=true

#### Response - Cancel Pending Payments

| 204 No Content |
|----------------|

## Get a Payee's Unmasked Account Number

This API enables retrieving a payee's unmasked account number.

The parameter **unmaskedAccountNumberUri** (returned for [Get
Payees](#get-payees-list) and [Get Payee](#get-payee-single)) returns
the endpoint shown below.

### Method and Endpoint

| GET | /api/v1/me/payees/{payeeId}/payeeAccountNumber|
|-----|----------------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description               |
|-----------|-----|------------|-----------|---------------------------|
| payeeId   | Req | path       | string    | Identifier for the payee. |

### Response

| Parameter | Req  | Data Type                 | Description                                                                                                                                  |
|------------|------|-------|-----------------------------------------------|
| data      | Req  | string                    | Unmasked account number of the payee. There is an empty string if there is no data to return.                                                |
| result    | Cond | [ResultType](./complexObjects.md#resulttype) | Result associated with the request. Condition: Only returned when the request fails. No content returned for success (HTTP status code 200). |

## Get Automatic Transaction Options

This API returns a list of automatic transaction options for a payee.

### Method and Endpoint

| GET | /api/v1/me/payees/{Id}/automaticTransactionOptions|
|-----|----------------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description               |
|-----------|-----|------------|-----------|---------------------------|
| id        | Req | path       | string    | Identifier for the payee. |

### Response

| Parameter | Req  | Data Type                                                   | Description                                                                                                                                  |
|------------|------|-------------|------------------------------------------|
| data      | Req  | [AutomaticTransactionOptions](./complexObjects.md#automatictransactionoptions) | Automatic transaction options for the payee.                                                                                                 |
| result    | Cond | [ResultType](./complexObjects.md#resulttype)                                   | Result associated with the request. Condition: Only returned when the request fails. No content returned for success (HTTP status code 200). |

### Sample API Usage

#### Request URL

https://api-checkfreenext-cert.fiservapps.com/api/v1/me/payees/0f425526dab84f2493588dcef2855db3/automaticTransactionOptions

#### Response
```json
{
"data": {
"billTransactionScheduleActive": true,
"billTransactionScheduleCapable": true,
"billTransactionScheduleTypesSupported": {
"transactionTypes": [
"FixedAmount",
"AccountBalance",
"AmountDue",
"MinimumAmountDue"
],
"transactionInitiationTypes": [
"DueDate",
"UponReceipt",
"DaysBeforeDueDate"
],
"eligibleFundingAccounts": [
{
"fundingAccountUri":
"/api/v1/me/bankAccounts/1c1c101ccd9548d8a1bdd45b565efcf1",
"maxAmount": 99999.99,
"minAmount": 1.00
},
{
"fundingAccountUri":
"/api/v1/me/bankAccounts/ce850f2a2bc8415a93cee242bf3ac9ce",
"maxAmount": 99999.99,
"minAmount": 1.00
}
]
},
"recurringTransactionScheduleActive": false,
"recurringTransactionScheduleCapable": true,
"recurringTransactionScheduleTypesSupported": {
"frequency": [
"Weekly",
"Every2Weeks",
"Every4Weeks",
"TwiceAMonth",
"Monthly",
"Every2Months",
"Every3Months",
"Every4Months",
"Every6Months",
"Annually"
],
"duration": [
"UntilSubscriberCancels",
"XNumberOfPayments",
"SpecificEndDate"
],
"eligibleFundingAccounts": [
{
"earliestInitiationDate": "2019-05-14",
"fundingAccountUri":
"/api/v1/me/bankAccounts/1c1c101ccd9548d8a1bdd45b565efcf1",
"maxAmount": 99999.99,
"minAmount": 1.00
},
{
"earliestInitiationDate": "2019-05-14",
"fundingAccountUri":
"/api/v1/me/bankAccounts/ce850f2a2bc8415a93cee242bf3ac9ce",
"maxAmount": 99999.99,
"minAmount": 1.00
}
]
}
}
}
```