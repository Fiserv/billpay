# Bills

This set of APIs allows you to:

-   [Get a list of bills or a specific bill](#get-bills)

-   [Get bill detail](#get-bill-detail)

-   [Update a bill](#update-a-bill)

## Get Bills

The Bills Get API returns the list of all due bills for a given
consumer. The list of bills will contain each bill’s current status 
(Paid, Unpaid, PaymentFailed, PaymentCanceled, Filed).

The list will include any unpaid e-bills (with the exception of e-bills 
that are scheduled to be paid via AutoPay) and bill due alerts
for any active payees associated with the consumer. E-bills are
delivered for payees with active e-bill service. Bill due alerts are
delivered for payees that were found through bill discovery and are not
yet activated for e-bills.

If there are multiple unpaid bills for a payee, only the most recent
bill is returned in the list. After the most recent bill is paid, the
payee will no longer be returned in the list until the next e-bill or
bill due alert arrives.

**When to use:** There are two business models for this API. The first
one is getting all bills for the consumer. The UI can present all unpaid
bills in a single screen to provide the consumer easy access.

The second business model is one in which the UI presents a list of
to-do items that contain e-bills that need to be paid or filed. The
to-do item will contain the bill ID, so the consumer can then select the
to-do item and go directly to the bill to pay or file it.

Optionally, the UI can invoke the API with a bill identifier to get a
bill.

### Method and Endpoint

| GET | /api/v1/me/bills |
|-----|------------------|

| GET | /api/v1/me/bills/{id} |
|-----|-----------------------|

### Request

When query parameters are not passed, bills from the last 45 days and 
future bills for a consumer are returned. If a calling application requires 
bills more than 45 days old to be displayed to their users, call this API 
with the parameters numberOfDays and startingDate.

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| id | Opt | path | string | Identifier for the bill. |
| numberOfDays | Opt | query | string | Number of days from the StartingDate that the list of bills returned will encompass. If numberOfDays and startingDate are not supplied, bills from the last 45 days and future bills for a consumer are returned. |
| startingDate | Opt | query | string | Date that is the starting point for the list of bills to be returned. <br> Must contain a valid date in the format yyyy-MM-dd <br> If numberOfDays and startingDate are not supplied, bills from the last 45 days and future bills for a consumer are returned. |

### Response

| Parameter | Req | Data Type                 | Description                                            |
|----------|-----|----------|------------------------------------------------|
| data      | Req | Array of [Bill](?path=docs/apiDomains/complexObjects.md&branch=develop#bill)    | There is an empty array if there is no data to return. |
| result    | Req | [ResultType](?path=docs/apiDomains/complexObjects.md&branch=develop#resulttype) | Result information.                                    |

### Sample API Usage

#### Response
```json
"data": [
{
"amountDue": 99.0,
"balanceAmountDue": 99.0,
"billerInfo": {
"billerLogoUrl": "https://path/images/cflogo4.gif",
"billReferenceImageUrl": "https://path/image3.jpg",
"billReferenceLinkUrl": "https://path/ajdfkhaskf.jpg",
"billReferenceText": "Text"
},
"billType": "FromBiller",
"dateFirstViewed": "2019-03-06",
"destinationUrl":
"/api/v1/me/payees/13fb99d7fb844d21803c8100ac5360bb",
"dueDate": "2019-03-07",
"actionByDate": "2019-03-06T20:00:00Z",
"detailUrl":
"https://billdetail-test.getbills.com/eBillCentralSSO/EBAS/ViewDetailDirect",
"isDetailUrlIframeSupported": false,
"minimumAmountDue": 20.0,
"status": "Unpaid",
"useDueDateText": false,
"self": "/api/v1/me/bills/bd9ea4b7f5d54ee7af7282d752ea8062",
"id": "bd9ea4b7f5d54ee7af7282d752ea8062"
},
],
"result": {
"success": true,
"resultInfo": []
}
```
## Get Bill Detail

The Bills Detail Get API returns bill detail information for a specific
bill.

### Method and Endpoint

| GET | /api/v1/me/bills/{id}/detail |
|-----|------------------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| id | Req | path | string | Identifier for the bill. |
| redirect | Opt | query | boolean | Indicates if the bill detail URL should be returned in a redirect URL. <br> True – Return HTTP response 307 (Temporary Redirect), which contains the bill detail URL in the Location header. This is the default if not provided in the request. <br> False – Return the bill detail URL in the response body. |

### Response

If the value of the redirect parameter in the request is:

-   **true** or null, HTTP response 307 is returned for successful
    requests. The Location header contains the bill detail URL.

-   **false**, the bill detail URL is returned in the response.

| Parameter | Req | Data Type                 | Description                                                              |
|----------|-----|----------|------------------------------------------------|
| data      | Req | string                    | Bill detail URL. There is an empty string if there is no data to return. |
| result    | Req | [ResultType](?path=docs/apiDomains/complexObjects.md&branch=develop#resulttype) | Result information.                                                      |

### Sample API Usage

#### Request URL – redirect is true

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/bills/c135bc0d381a4d7ba552690d31ee5f8e/detail?redirect=True |
|------------------------------------------------------------------------|

#### Response – redirect is true

307 Temporary Redirect
Location:
https://somesite.com/ViewDetailDirect?payloadEbas=ZnSc61JmWcCv+/
b9Wfb5jGcJCgHcwQ692Odog55okH03Cdq2h4dEjZUr+xe7jqU6arzN6lRcK87SzeBEUoAjdYHW7cWjX5bbpD8FfyXOc0d75cvG21svm/P8Eg+NB/
0avnsmtG2axK6tOMBHoC2UeQ==

#### Request URL – redirect is false

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/bills/c135bc0d381a4d7ba552690d31ee5f8e/detail?redirect=False |
|------------------------------------------------------------------------|

#### Response – redirect is false
```json
"data": {
"billDetailUri":
"https://somesite.com/ViewDetailDirect?payloadEbas=zJqJpKPaZVZorCSr7DOeu39OvYWlME7TIHVqB8o27W2lDtQZP6UJlIk5SjnRC4ThzRC1sddLspvPHEm5IBcfm6aEMHVwqMajoIWwscmc1Nu9Swo0raQf4z5b4OLFisyEf6TUcU31s6nRHycofTZg=="
},
"result": {
"success": true,
"resultInfo": []
}
```
## Update a Bill

The Bills Patch API allows a consumer to file a bill.

### Method and Endpoint

| PATCH | /api/v1/me/bills/{id}/File|
|-------|--------------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| id | Req | path | string | Identifier for the bill. |
| note | Opt | body | string | Optional note entered by the consumer with information about the bill and its resolution. Length: 1-80 |
| reason | Req | body | string | Indicates the method of the bill’s resolution. Valid values: <br> NoneSpecified <br> Bank <br> Check <br> Cash <br> NotPaid <br> Other <br> BillerWebSite <br> Phone <br> Mail <br> Office <br> ZeroBalanceBill <br> ContestedBill |

### Response

| Parameter | Req  | Data Type                 | Description                                                                                                                                  |
|------------|-----|-----------|---------------------------------------------|
| result    | Cond | [ResultType](?path=docs/apiDomains/complexObjects.md&branch=develop#resulttype) | Result associated with the request. Condition: Only returned when the request fails. No content returned for success (HTTP status code 204). |

### Sample API Usage

#### Request URL

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/bills/87fe980db32146b8a7cc50e597673ff0/File |
|------------------------------------------------------------------------|

#### Request Body
```json
{
"note": "Note",
"reason": "ZeroBalanceBill"
}
```
#### Response

| 204 No Content |
|----------------|

 