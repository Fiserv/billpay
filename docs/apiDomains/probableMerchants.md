
# Probable Merchants

This API allows the calling application to:

-   [Get probable merchants](#get-probable-merchants)

## Get Probable Merchants

The ProbableMerchants Get API presents "probable" or "likely" merchants
that a consumer would make payments to. This API returns a list of
merchants that are typically paid by other bill payment users in that
consumer's ZIP Code area (the consumer's ZIP Code is identified using
the subscriber ID). Local utility companies, financial institutions,
service companies, etc. may all be returned.

**When to use:** A client's UI application that enables consumers to
manage their payees can use the ProbableMerchants Get API in conjunction
with the Merchants Search Get API. By using these in combination, a UI
application can present the consumer with the options of selecting from
a list of probable merchants or entering a payee name.

Examples:

-   Add a new payee to a consumer's existing list of payees.

-   Present a special flow to new users that helps them to get started
    using bill pay functionality.

### Method and Endpoint

| GET | /api/v1/me/probableMerchants |
|-----|----------------------------|

### Response

| Parameter | Req | Data Type                               | Description                 |
|------------|-----|----------|-----------------------------------------------|
| data      | Req | [ProbableMerchants](?path=docs/apiDomains/complexObjects.md&branch=develop#probablemerchants) | List of probable merchants. |
| result    | Req | [ResultType](?path=docs/apiDomains/complexObjects.md&branch=develop#resulttype)               | Result information.         |

### Sample API Usage

#### Response
```json
{
"data": {
"probablePayeeList": [
{
"categoryId": 1,
"category": "UTILITIES",
"suggestedDisplayPriority": 1,
"merchantList": [
{
"displayPayeeName": "Bay State Gas",
"logoUri":
"https://payeelogos-dev.apps.fog.onefiserv.net/63.png",
"payeeList": [
{
"billerId": "7186434050001",
"onlinePayeeName": "Bay State Gas (SBC-AR,KS,MO)",
"merchantZipRequired": false,
"self":
"/api/v1/me/probableMerchants/fc15a723d846358b3fa67aad33ddd5",
"id": "fc15a723d846358b3fa67aad33ddd5"
},
{
"billerId": "7186434052131",
"onlinePayeeName": "Bay State Gas (SBC-MI,OH,WI)",
"merchantZipRequired": true,
"self":
"/api/v1/me/probableMerchants/fc15a723d846358b3fa67aad22ddd5",
"id": "fc15a723d846358b3fa67aad22ddd5"
}
]
},
{
"displayPayeeName": "FIRSTAR BANKS",
"logoUri":
"https://payeelogos-dev.apps.fog.onefiserv.net/31.png",
"payeeList": [
{
"billerId": "7123434050001",
"onlinePayeeName": "FIRSTAR BANKS (SBC-AR,KS,TX)",
"merchantZipRequired": true,
"self":
"/api/v1/me/probableMerchants/fc15a733d846358b3fa67aad22ddd5",
"id": "fc15a733d846358b3fa67aad22ddd5"
},
{
"billerId": "7186432341001",
"onlinePayeeName": "FIRSTAR BANKS (SBC-IL,OH,WI)",
"merchantZipRequired": false,
"self":
"/api/v1/me/probableMerchants/fc15a833d846358b3fa67aad22ddd5",
"id": "fc15a833d846358b3fa67aad22ddd5"
}
]
}
]
},
{
"categoryId": 2,
"category": "PHONE",
"suggestedDisplayPriority": 2,
"merchantList": [
{
"displayPayeeName": "AT&amp;T",
"logoUri": "https://payeelogos-dev.apps.fog.onefiserv.net/5.png",
"payeeList": [
{
"billerId": "7186431234001",
"onlinePayeeName": "AT&amp;T Bill (SBC-AR,KS,MO,OK,TX)",
"merchantZipRequired": true,
"self":
"/api/v1/me/probableMerchants/dc15a833d846358b3fa67aad22ddd5",
"id": "dc15a833d846358b3fa67aad22ddd5"
},
{
"billerId": "7186434012001",
"onlinePayeeName": "AT&amp;T (Local and Long Distance)",
"merchantZipRequired": true,
"self":
"/api/v1/me/probableMerchants/wc15a812d846358b3fa67aad22ddd5",
"id": "wc15a812d846358b3fa67aad22ddd5"
}
]
},
{
"displayPayeeName": "AMERICAN EXPRESS",
"logoUri":
"https://payeelogos-dev.apps.fog.onefiserv.net/52.png",
"payeeList": [
{
"billerId": "8671434050001",
"onlinePayeeName": "American Express",
"merchantZipRequired": false,
"self":
"/api/v1/me/probableMerchants/ac15a812d812358b3fa67aad22ddd5",
"id": "ac15a812d812358b3fa67aad22ddd5"
},
{
"billerId": "9186434050001",
"onlinePayeeName": "American Express CC",
"merchantZipRequired": false,
"self":
"/api/v1/me/probableMerchants/rfc15a81d812358b3fa67aad22ddd5",
"id": "rfc15a81d812358b3fa67aad22ddd5"
}
]
}
]
}
]
},
"result": {
"success": true
}
}
```