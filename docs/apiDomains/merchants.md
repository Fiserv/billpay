# Merchants

This API allows the calling application to:

-   [Search for merchants](#search-for-merchants)

## Search for Merchants

The Merchants Search Get API enables a consumer to search for merchants
based on the merchant name. Any exact matches or partial matches based
on what the consumer entered as the search criteria are returned to the
client’s application. If the consumer wants to add a payee from the
merchants returned from this operation, they will need to enter only
their account number and possibly the merchant’s ZIP Code, but not the
full address of the merchant.

**When to use:** A client’s UI application that enables consumers to
manage their payees typically uses the Payee Post API in conjunction
with the Merchants Search Get API. By using these in combination, a UI
application can provide the ability to search for merchants and
ultimately add a payee to a consumer’s payee list from the found
merchants.

Examples:

-   Add a new payee to a consumer’s existing list of payees.

-   Present a special flow to new users that helps them to get started
    using bill pay functionality.

### Method and Endpoint

| GET | /api/v1/merchants/Search |
|-----|--------------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| name | Req | query | string | Length: 3–32 <br> The name of the merchant name to be found. The search characters entered shall be those contained within the merchant name (not a “starts with” search). |
| returnLocalMerchants | Opt | query | boolean | Indicates whether to return merchants that are relevant to the consumer’s ZIP Code. <br> True – If there is no match, return local merchants in the response. If there is a match or partial match, return those matches only. Default is true. <br> False – If no matches are found, do not return any local merchants. | 

### Response

| Parameter | Req | Data Type | Description |
|-----------|-----|-----------|-------------|
| data | Req | Array of [Merchant](./complexObjects.md#merchant) | List of merchants found based on the provided name search criteria and the returnLocalMerchants flag. Merchants matched are returned in the following order: <br> 1. Exact matches <br> 2. Merchants containing the search criteria <br> If no matches are found based upon the entered search criteria, merchants relevant to the consumer’s ZIP Code will be returned if the returnLocalMerchants flag is true. |
| result | Req | [ResultType](./complexObjects.md#resulttype) | Result information. |

### Sample API Usage

#### Request URL – Exact Match

| https://api-checkfreenext-cert.fiservapps.com/api/v1/merchants/Search?name=Bay+State+Gas&returnLocalMerchants=False |
|------------------------------------------------------------------------|

#### Response – Exact Match
```json
{
"data": [
{
"logoUri":
"https://logos-checkfreenext-cert.fiservapps.com/63.png",
"merchantZipRequired": false,
"name": "Bay State Gas",
"self": "/api/v1/merchants/1d7bc8609f424f30864695cb9a92122b",
"id": "1d7bc8609f424f30864695cb9a92122b"
}
],
"result": {
"success": true,
"resultInfo": []
}
}
```
#### Request URL – Partial Match

| https://api-checkfreenext-cert.fiservapps.com/api/v1/merchants/Search?name=Bay&returnLocalMerchants=False |
|------------------------------------------------------------------------|

#### Response – Partial Match
```json
{
"data": [
{
"logoUri":
"https://logos-checkfreenext-cert.fiservapps.com/63.png",
"merchantZipRequired": false,
"name": "Bay State Gas",
"self": "/api/v1/merchants/1d7bc8609f424f30864695cb9a92122b",
"id": "1d7bc8609f424f30864695cb9a92122b"
},
{
"logoUri": "https://logos-checkfreenext-cert.fiservapps.com/70.png",
"merchantZipRequired": false,
"name": "Bombay Company",
"self": "/api/v1/merchants/558fc18488cf42babac32526aa48af81",
"id": "558fc18488cf42babac32526aa48af81"
},
{
"logoUri": "https://logos-checkfreenext-cert.fiservapps.com/250.png",
"merchantZipRequired": false,
"name": "East Bay Carol",
"self": "/api/v1/merchants/86803dd9ed124e3bad31f24cb53b34bb",
"id": "86803dd9ed124e3bad31f24cb53b34bb"
},
{
"logoUri": "https://logos-checkfreenext-cert.fiservapps.com/7142.png",
"merchantZipRequired": false,
"name": "East Bay Mud",
"self": "/api/v1/merchants/5944ae33d9354ca3b87e5ee05dd2a944",
"id": "5944ae33d9354ca3b87e5ee05dd2a944"
}
]
```
#### Request URL – Special Character

| https://api-checkfreenext-cert.fiservapps.com/api/v1/merchants/Search?name=AT%26T |
|------------------------------------------------------------------------|

#### Response – Special Character
```json
{
"data": [
{
"logoUri":
"https://logos-checkfreenext-cert.fiservapps.com/5.png",
"merchantZipRequired": true,
"name": "AT&amp;T",
"self": "/api/v1/merchants/a9265cab5a4d4b24b916b1da17f8057e",
"id": "a9265cab5a4d4b24b916b1da17f8057e"
},
{
"logoUri":
"https://logos-checkfreenext-cert.fiservapps.com/5.png",
"merchantZipRequired": true,
"name": "AT&amp;T (Local and Long Distance)",
"self": "/api/v1/merchants/71aa7ac36a6445ed99efe52aa821fae1",
"id": "71aa7ac36a6445ed99efe52aa821fae1"
},
{
"logoUri":
"https://logos-checkfreenext-cert.fiservapps.com/PayeeDefaultLogo.png",
"merchantZipRequired": false,
"name": "AT&amp;T Bill (BellSouth)",
"self": "/api/v1/merchants/ce2a3ec706a4447d87f9b129f4fcdad1",
"id": "ce2a3ec706a4447d87f9b129f4fcdad1"
},
{
"logoUri":
"https://logos-checkfreenext-cert.fiservapps.com/5.png",
"merchantZipRequired": true,
"name": "AT&amp;T Bill (SBC-AR,KS,MO,OK,TX)",
"self": "/api/v1/merchants/a157d32ef1234acfbf14afafc3e7d625",
"id": "a157d32ef1234acfbf14afafc3e7d625"
}
]
```
#### Request URL – ZIP Code-based

| https://api-checkfreenext-cert.fiservapps.com/api/v1/merchants/Search?name=Bay+Stafdgte+Gdgas |
|------------------------------------------------------------------------|

#### Response – ZIP Code-based
```json
{
"data": [
{
"logoUri":
"https://logos-checkfreenext-cert.fiservapps.com/63.png",
"merchantZipRequired": false,
"name": "Bay State Gas",
"self": "/api/v1/merchants/1d7bc8609f424f30864695cb9a92122b",
"id": "1d7bc8609f424f30864695cb9a92122b"
},
{
"logoUri":
"https://logos-checkfreenext-cert.fiservapps.com/121.png",
"merchantZipRequired": true,
"name": "General Motors Acceptance Corp",
"self": "/api/v1/merchants/cf495d15a0af4cee9424bbe1caf83ee7",
"id": "cf495d15a0af4cee9424bbe1caf83ee7"
},
{
"logoUri":
"https://logos-checkfreenext-cert.fiservapps.com/76.png",
"merchantZipRequired": true,
"name": "KeySpan Energy Delivery",
"self": "/api/v1/merchants/c496363b763e40d9ae7466442724d43e",
"id": "c496363b763e40d9ae7466442724d43e"
},
{
"logoUri":
"https://logos-checkfreenext-cert.fiservapps.com/365.png",
"merchantZipRequired": false,
"name": "Liberty Mutual",
"self": "/api/v1/merchants/eb5d80969f8048e8bb2e52e32d831196",
"id": "eb5d80969f8048e8bb2e52e32d831196"
}
]
```
# 