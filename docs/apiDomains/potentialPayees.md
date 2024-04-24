# Potential Payees

A potential payee is a biller with which a consumer maintains a payment
relationship (biller direct) and has agreed to find using the
information in credit files and/or biller-generated statements. The
discovered biller can potentially be added to CheckFree Next as a payee
and be sent payments via the bill pay services. The custom UI should
inform the consumer about the benefits of finding bills compared to
adding a payee from scratch and then explicitly record the consumer’s
consent before the bills and billers can be found from various sources.

This set of APIs provides access to a potential payee resource:

-   [Determine a consumer's eligibility for bill discovery](#get-consumer-eligibility-for-bill-discovery)

-   [Get a list of potential payees](#get-potential-payees-list)

-   [Get a specific potential payee](#get-potential-payee-single)

-   [Verify a potential payee](#verify-potential-payee)

-   [Dismiss a potential payee](#dismiss-potential-payee)

-   [Get an unmasked potential payee account number](#get-a-potential-payees-unmasked-account-number)

## Get Consumer Eligibility for Bill Discovery

This API indicates whether a consumer is eligible for bill discovery.

### Method and Endpoint

| GET | /api/v1/me/PotentialPayees/eligibility |
|------|------------------|
### Response

| Parameter | Req | Data Type                                                     | Description                       |
|---------|----|------------|------------------------------------------------|
| data      | Req | [BillDiscoveryUserEligibility](?path=docs/apiDomains/complexObjects.md&branch=develop#billdiscoveryusereligibility) | Consumer eligibility information. |
| result    | Req | [ResultType](?path=docs/apiDomains/complexObjects.md&branch=develop#resulttype)                                     | Result information.               |

The values returned in the response are dependent on sponsor
configuration as follows:

  * If a sponsor is not enabled for Bill Discovery or one of the Bill Discovery data sources, the API returns creditReportEligible and billServiceProviderEligible as false. Zero is returned for the potential payee count.

  * When BillDiscovery is configured as true for the sponsor:

    -   If the sponsor is configured with eligibility rules, those rules will be taken into account (based on the number of payments made             over a period of time). If the consumer is not eligible, the outstandingPotentialPayeesCount is still returned, allowing a consumer to act on any previously found payees. The creditReportEligible and billServiceProviderEligible flags are returned based on consumer eligibility and sponsor configuration.

    -   If the sponsor is not configured with eligibility rules, then the creditReportEligible and billServiceProviderEligible flags are returned based on the sponsor configuration for these data sources. The outstandingPotentialPayeesCount reflects the number of potential payees previously found and not acted on for the consumer. In this situation, this call is not necessary.

### Sample API Usage

#### Request URL

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/potentialPayees/eligibility |
|------------------------------------------------------------------------|

#### Response
```json
{
"data": {
"creditReportEligible": true,
"billServiceProviderEligible": true,
"outstandingPotentialPayeesCount": 0
},
"result": {
"success": true,
"resultInfo": []
}
}
```
## Get Potential Payees (List)

The Potential Payees Get API provides a list of all potential payees
found for the consumer through bill discovery that have not been added
yet as payees by the consumer. This provides an easy way for the
consumer to select a merchant they recognize and add that merchant as a
payee. This operation specifically returns the consumer’s account number
with the merchant so that the consumer does not need to enter any
information to add the merchant as a payee.

### Method and Endpoint

| GET | /api/v1/me/PotentialPayees |
|-----|----------------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| excludeDormantAccounts | Opt | query | boolean | Indicates whether to exclude dormant payee accounts from the results. <br> True – Exclude dormant payee accounts from the results. <br> False – Include dormant payee accounts. This is the default. <br> If the search parameter value is false, the excludeDormantAccounts value must be false. |
| search | Opt | query | boolean | Indicates whether to perform a new search for potential payees. <br> True – Perform a new search. This is the default.<br> False – Do not perform a new search (return only potential payees already identified). <br> If the search parameter value is false, the excludeDormantAccounts value must be false. | 

### Response

| Parameter | Req | Data Type                                 | Description               |
|---------|----|-----------|-------------------------------------------------|
| data      | Req | [PotentialPayeeList](?path=docs/apiDomains/complexObjects.md&branch=develop#potentialpayeelist) | List of potential payees. |
| result    | Req | [ResultType](?path=docs/apiDomains/complexObjects.md&branch=develop#resulttype)                 | Result information.       |

### Sample API Usage

#### Response
```json
{
"data": {
"potentialPayees": [
{
"additionalInfoRequired": true,
"merchantData": {
"merchantName": "PROVIDIAN BANK CORP",
"merchantLogoUrl":
"https://logos-checkfreenext-cert.fiservapps.com/7175.png",
"merchantUri":
"/api/v1/merchants/74741f40cd7e4b0c9fba9a60d5561bec"
},
"verificationTokens": [
{
"description": "Account Number",
"type": "String"
},
{
"description": "Zip 5+4 where your payment would be sent",
"type": "String"
}
],
"verificationUri":
"/api/v1/me/potentialPayees/8c1147042bc34507a733826587309fd5/Verify",
"self":
"/api/v1/me/potentialPayees/8c1147042bc34507a733826587309fd5",
"id": "8c1147042bc34507a733826587309fd5"
}
]
}
}
```
## Get Potential Payee (Single)

The Potential Payees Get API provides the details for a specific
potential payee found for the consumer through bill discovery that has
not been added yet as a payee by the consumer. This operation
specifically returns the consumer’s account number with the merchant so
that the consumer does not need to enter any information to add the
merchant as a payee.

### Method and Endpoint

| GET | /api/v1/me/PotentialPayees/{id} |
|-----|---------------------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description                         |
|-----------|-----|------------|-----------|-------------------------------------|
| id        | Req | path       | string    | Identifier for the potential payee. |

### Response

| Parameter | Req | Data Type                         | Description                            |
|---------|----|-----------|-------------------------------------------------|
| data      | Req | [PotentialPayee](?path=docs/apiDomains/complexObjects.md&branch=develop#potentialpayee) | Information about the potential payee. |
| result    | Req | [ResultType](?path=docs/apiDomains/complexObjects.md&branch=develop#resulttype)         | Result information.                    |

### Sample API Usage

#### Request URL

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/PotentialPayees/35616929c7814e83bea042b20285ec88 |
|------------------------------------------------------------------------|

#### Response
```json
{
"data": {
"additionalInfoRequired": true,
"merchantData": {
"merchantName": "PROVIDIAN BANK CORP",
"merchantLogoUrl":
"https://logos-checkfreenext-cert.fiservapps.com/7175.png",
"merchantUri":
"/api/v1/merchants/74741f40cd7e4b0c9fba9a60d5561bec"
},
"verificationTokens": [
{
"description": "Account Number",
"type": "String"
},
{
"description": " Zip 5+4 where your payment would be sent ",
"type": "String"
}
],
"verificationUri":
"/api/v1/me/PotentialPayees/35616929c7814e83bea042b20285ec88/Verify",
"self":
"/api/v1/me/PotentialPayees/35616929c7814e83bea042b20285ec88",
"id": "35616929c7814e83bea042b20285ec88"
}
}
```
## Verify Potential Payee

Verification (such as an account number) may be required for a potential
payee.

### Method and Endpoint

| GET | /api/v1/me/PotentialPayees/{id}/Verify |
|-----|----------------------------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| id | Req | path | string | Identifier for the potential payee. | 
| verificationTokens | Cond | body | Array of [VerificationToken](?path=docs/apiDomains/complexObjects.md&branch=develop#verificationtoken) | Array of verification token information provided by the consumer to verify the merchant relationship. <br> Condition: One or more verification tokens are required for this payee. If no verification tokens are required, this array is not required. |

### Response

| Parameter | Req | Data Type                                     | Description                            |
|---------|----|-----------|-------------------------------------------------|
| data      | Req | [VerificationResponse](?path=docs/apiDomains/complexObjects.md&branch=develop#verificationresponse) | Information about the potential payee. |
| result    | Req | [ResultType](?path=docs/apiDomains/complexObjects.md&branch=develop#resulttype)                     | Result information.                    |

### Sample API Usage

#### Request URL

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/PotentialPayees/8c1147042bc34507a733826587309fd5/Verify |
|------------------------------------------------------------------------|

#### Request Body
```json
{
"verificationTokens": [
{
"description": "ACCOUNTNUMBER",
"value": "4121295555555558",
"type": "String"
},
{
"description": "ZIP5",
"value": "43235",
"type": "String"
}
]
}
```
#### Response
```json
{
"data": {
"payeeUri": "/api/v1/me/Payees/059202055504452da803383409f721ff",
"maskedAccountNumber": "**********5309",
"unmaskedAccountNumberUri":
"/api/v1/me/payees/da32f76b52ce4dbd9545d341f99a8e56/payeeAccountNumber",
"additionalInfoRequired": true,
"merchantData": {
"merchantName": "Test Merchant Name",
"merchantLogoUrl": "/api/v1/me/MerchantLogos/64",
"merchantUri": "/api/v1/me/Merchants/64"
},
"verificationTokens": [
{
"description": "Test Verification Description",
"type": "String"
}
],
"verificationUri":
"/api/v1/me/PotentialPayees/da32f76b52ce4dbd9545d341f99a8e56/Verify",
"self":
"/api/v1/me/PotentialPayees/Verify/da32f76b52ce4dbd9545d341f99a8e56",
"id": "da32f76b52ce4dbd9545d341f99a8e56"
},
"result": {
"success": true
}
}
```
## Dismiss Potential Payee

This API can be used to dismiss a potential payee. When a potential
payee is dismissed, the potential payee will no longer appear in the
ToDo list or potential payee list.

### Method and Endpoint

| PUT | /api/v1/me/PotentialPayees/{id}/dismiss |
|-----|----------------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description                         |
|-----------|-----|------------|-----------|-------------------------------------|
| id        | Req | path       | string    | Identifier for the potential payee. |

### Response

| Parameter | Req  | Data Type                 | Description                                                                                                                  |
|---------|-----|----------|-------------------------------------------------|
| result    | Cond | [ResultType](?path=docs/apiDomains/complexObjects.md&branch=develop#resulttype) | Result information. Condition: Only returned when the request fails. No content returned for success (HTTP status code 204). |

### Sample API Usage

#### Request URL

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/potentialPayees/e421657097d54c6b8fb8d585dd70aed4/dismiss |
|------------------------------------------------------------------------|

#### Response

| 204 No Content |
|----------------|

## Get a Potential Payee’s Unmasked Account Number

This API enables retrieving a potential payee’s unmasked account number.

The parameter **unmaskedAccountNumberUri** (returned for [Get Potential Payees](#get-potential-payees-list), [Get Potential Payee](#get-potential-payee-single), and [Verify Potential Payee](#verify-potential-payee)) returns the endpoint shown below.

### Method and Endpoint

| GET | /api/v1/me/PotentialPayees/{id}/payeeAccountNumber|
|-----|----------------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description                         |
|-----------|-----|------------|-----------|-------------------------------------|
| id        | Req | path       | string    | Identifier for the potential payee. |

### Response

| Parameter | Req  | Data Type                 | Description                                                                                                                                  |
|------------|------|-------|-----------------------------------------------|
| data      | Req  | string                    | Unmasked account number of the potential payee. There is an empty string if there is no data to return.                                      |
| result    | Cond | [ResultType](?path=docs/apiDomains/complexObjects.md&branch=develop#resulttype) | Result associated with the request. Condition: Only returned when the request fails. No content returned for success (HTTP status code 200). |

 #