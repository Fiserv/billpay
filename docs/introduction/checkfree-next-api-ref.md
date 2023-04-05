# CheckFree<sup>®</sup> Next<sup>℠</sup>

API Reference

Dec. 15, 2022 

#####  

# Contents

[Overview](#overview)

[Getting Started](#getting-started)

[REST API Reference](#rest-api-reference)

[Health](#health)

[Users](#users)

[Potential Payees](#potential-payees)

[Payees](#payees)

[Payee Groups](#payee-groups)

[Merchants](#merchants)

[Probable Merchants](#probable-merchants)

[Reminders](#reminders)

[ToDos](#todos)

[Bills](#bills)

[E-bill Activation](#e-bill-activation)

[BankAccounts](#bankaccounts)

[CardAccounts](#cardaccounts)

[TransactionCalendar](#transactioncalendar)

[Transactions](#transactions)

[AutomaticTransactions](#automatictransactions)

[Complex Objects](#complex-objects)

[Appendix A: Sample Use Case
Implementation](#appendix-a-sample-use-case-implementation)

[Appendix B: Response Codes](#appendix-b-response-codes)

# 

# Overview 

CheckFree Next is the next generation of bill pay services from Fiserv
enabling financial institutions to build a customized bill pay
experience that is simpler, smarter, and faster, and that positions them
to be first-in-line for their account holders’ digital payment needs. It
is a collection of simple, powerful, secure, and reliable RESTful APIs
that provide access to Fiserv’s powerful and industry leading payment
processing capabilities.

Integrate with CheckFree Next APIs and enable your consumers to:

-   Find bills by using information from credit report and biller
    statements with the consumer’s consent and select to add payees
    conveniently.

-   Receive alerts for due bills without giving up paper.

-   Make a bill payment to a managed or unmanaged business.

-   Sign up for eBills from eBill capable billers to receive a full
    eBill statement.

-   Automate payments and never be late with a payment again.

# 

# Getting Started

To get started with the CheckFree Next APIs, follow these steps:

## Register

Once agreements have been signed, a financial institution will work with
Fiserv Professional services to set up certificates and a client ID.

## Authenticate

Authentication to the CheckFree Next APIs is achieved using OAuth 2.0
via a custom grant.To obtain an access token, a JSON Web Token (JWT)
must be constructed with this minimum information. All claims are
required/conditionally required except for acctbal.

<table>
<colgroup>
<col style="width: 23%" />
<col style="width: 76%" />
</colgroup>
<thead>
<tr class="header">
<th>Claim</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>iss</td>
<td>Issuer of the JWT</td>
</tr>
<tr class="even">
<td>aud</td>
<td>Audience for the JWT</td>
</tr>
<tr class="odd">
<td>iat</td>
<td>Issue time for the JWT</td>
</tr>
<tr class="even">
<td>exp</td>
<td>Time that the JWT is set to expire</td>
</tr>
<tr class="odd">
<td>jti</td>
<td>Nonce (unique identifier for the JWT)</td>
</tr>
<tr class="even">
<td>fiserv.identity.billpay.sponsorId</td>
<td>Sponsor ID. Required if subscriberId or externalSubscriberId is
provided.</td>
</tr>
<tr class="odd">
<td>fiserv.identity.billpay.subscriberId</td>
<td><p>Subscriber ID. Provide one of the following for a consumer:</p>
<ul>
<li><p>sponsorId AND subscriberId</p></li>
<li><p>sponsorId AND externalSubscriberId</p></li>
<li><p>checkfreeNext.userId</p></li>
</ul></td>
</tr>
<tr class="even">
<td>fiserv.identity.billpay.externalSubscriberId</td>
<td><p>External Subscriber ID. Provide one of the following for a
consumer:</p>
<ul>
<li><p>sponsorId AND subscriberId</p></li>
<li><p>sponsorId AND externalSubscriberId</p></li>
<li><p>checkfreeNext.userId</p></li>
</ul></td>
</tr>
<tr class="odd">
<td>fiserv.identity.billpay.originator</td>
<td>Originator of the request. Valid values: Subscriber, Sponsor</td>
</tr>
<tr class="even">
<td>fiserv.identity.billpay.channel</td>
<td>Channel for the request. Valid values: Desktop, Mobile</td>
</tr>
<tr class="odd">
<td>fiserv.identity.checkfreeNext.userId</td>
<td><p>CheckFree Next user ID. Provide one of the following for a
consumer:</p>
<ul>
<li><p>sponsorId AND subscriberId</p></li>
<li><p>sponsorId AND externalSubscriberId</p></li>
<li><p>checkfreeNext.userId</p></li>
</ul></td>
</tr>
<tr class="even">
<td>fiserv.identity.billpay.acctbal</td>
<td><p>Last 4 of account number and corresponding account balance. This
claim is optional.</p>
<ul>
<li><p>The account number (last 4) is hashed with SHA-512 using
subscriber ID as the salt.</p></li>
<li><p>The account balance can be positive, negative, or zero.</p></li>
<li><p>The account balance will not contain a decimal point or a dollar
sign.</p></li>
<li><p>The hashed account number (last 4) and account balance will be
separated by a period (dot).</p></li>
<li><p>If multiple bank account balances are sent, they will be
separated by commas.</p></li>
<li><p>In the following example, the account balance is $200.03</p>
<p>Example:<br />
D9FB0F4404D8F91D1A249527A198EE9D7B33F088CE75FCBEE6951EEEE3D98E264A3B6B60B92E19771FF07CDE1C<br />
B277AE505463A89C84BA16809195CC720A322E.20003</p></li>
<li><p>If the account balance is negative, a negative sign will be in
the far-left position of the amount field. In the following example, the
account balance is -$123.45<br />
Example:<br />
0C933649A125E2C06524AE024FA1122D754C274AF45F66CFA5024B71AC697FED5E5443DDC63793F8FB70C7C861<br />
6BBFD876B3C1C29031F35C2AA0F9AE7E448A1D.-12345</p></li>
<li><p>In the following example, the account balance is $0<br />
Example:<br />
0C933649A125E2C06524AE024FA1122D754C274AF45F66CFA5024B71AC697FED5E5443DDC63793F8FB70C7C861<br />
6BBFD876B3C1C29031F35C2AA0F9AE7E448A1D.0</p></li>
<li><p>In the following example, multiple account balances are sent. The
two accounts are separated by a comma. The account balance for the first
account is $999.99. The account balance for the second account is
$5211.45.</p></li>
<li><p>Example:</p>
<p>7D419B0B8A5A9D0D865C3DDB84227758B55909BCF23698589E772E9288E24F707EA1EE2238DA59BF26177FB4AE<br />
0326906CE5A8ADE3AB5843FD5AA15F7CC13E1B.99999,51518D25F15B1C9D3B87073EC2949AC138BF3B95E833A<br />
3299509DA3AA011B3F9AFBDB4EDE7EB157D44DAF3E76F0AFF3105DEDC2F227327904B691BAB22A01ADB.521145</p></li>
</ul></td>
</tr>
</tbody>
</table>

Here is a sample JWT payload:
```json
{  
"iss":"Second National Bank",  
"aud":"Fiserv",  
"iat":"1543861896",  
"jti":"b5d2bcef-0a8b-4218-9f09-cc9c8a24d98c",  
"exp":"1543862196",  
"fiserv.identity.billpay.sponsorId":"45678",  
"fiserv.identity.billpay.subscriberId":"98796234567",  
"fiserv.identity.billpay.originator":"Subscriber",  
"fiserv.identity.billpay.channel":"Desktop",  
"fiserv.identity.billpay.acctbal":"D9FB0F4404D8F91D1A249527A198EE9D7B33F088CE75FCBEE6951EEEE3D98E264A3B6B60B92E19771FF07CDE1CB277AE505463A89C84BA16809195CC720A322E.20003"  
}
```
The JWT must be signed with your certificate and encrypted with Fiserv’s
public key. You will need to provide the public key of your signing
certificate to Fiserv. Please work with your project manager for full
requirements and installation. Your project manager will provide
Fiserv’s public key. This JWT then must be signed and encrypted with a
certificate that the FI provides. You will need to provide the public
key of your signing certificate to Fiserv. Please work with your project
manager for full requirements and installation. Fiserv will provide its
public key for encryption.

The signature should be created via the RS256 algorithm. The payload
then must be encrypted using JSON Web Encryption (JWE)
(<https://tools.ietf.org/html/rfc7516>) via RSA-OAEP-256 with
A256CBC-HS512 (<https://tools.ietf.org/html/rfc7518>).

Fiserv will provide the full endpoint to send the request to.

### Method and Endpoint

|POST | /connect/token  |
|------|----------------|




### Request

| Parameter   | Req | Param Type | Data Type         | Description                                                 |
|-----------|-----|-------|---------|-----------------------------------------|
| client\_id  | Req | body       | string            | Fiserv-provided ID; will be assigned during implementation. |
| grant\_type | Req | body       | “ChallengerGrant” |                                                             |
| JWT         | Req | body       | string            | JWT created above                                           |
| tenantId    | Req | body       | string            | Sponsor ID; will be assigned during implementation.         |

### Response

| Parameter      | Req | Data Type | Description                                                                                                                                                                                                                  |
|------------|----|----------|-----------------------------------------------|
| access\_token  | Req | string    | The access token string as issued by the authorization server.                                                                                                                                                               |
| expires\_in    | Req | integer   | The duration of time the access token is granted for.                                                                                                                                                                        |
| token\_type    | Req | string    | The type of token.                                                                                                                                                                                                           |
| refresh\_token | Req | string    | A refresh token that can be used to obtain another access token.                                                                                                                                                             |
| user\_status   | Req | string    | The status of the user. Valid values: Active, CancelledFraud, CancelledOther, FrozenFraud, FrozenOther, Inactive, VerificationNeeded, Unspecified. Not used in maintenance grants or for CheckFree Next user authentication. |

### Sample API Usage: Authentication Request for Subscriber

This subscriber-specific grant is scoped to a consumer.

#### JWT Payload
```json
{  
"iss":"Second National Bank",  
"aud":"Fiserv",  
"iat":"1543861896",  
"jti":"b5d2bcef-0a8b-4218-9f09-cc9c8a24d98c",  
"exp":"1543862196",  
"fiserv.identity.billpay.sponsorId":"45678",  
"fiserv.identity.billpay.subscriberId":"98796234567",  
"fiserv.identity.billpay.originator":"Subscriber",  
"fiserv.identity.billpay.channel":"Desktop"
}
```
#### Request Form Data

client_id: yourSpecificClientId  
grant_type: ChallengerGrant  
JWT: eyJhb…  
tenantId: 45678

#### Response (Success)

```json
{
"access_token":"ee5612220a264169bbb4d42bd88413fa92327a894734031f7f619e8e12dba7d0",
"expires_in":600,
"token_type":"Bearer",
"refresh_token":"d09234ab17c16af6d86cee5355cc91ca7fc4a4070bc80631a8fd7e857ade837f",
"user_status":"Active"
}
```

#### Response (Failure)

```json
{
"error": "invalid_grant"
}
```

### Sample API Usage: Authentication Request for User Maintenance

This maintenance grant is scoped to a tenant.

#### JWT Payload
```json
{  
"iss":"Second National Bank",  
"aud":"Fiserv",  
"iat":"1543861896",  
"jti":"b5d2bcef-0a8b-4218-9f09-cc9c8a24d98c",  
"exp":"1543862196",  
"fiserv.identity.billpay.sponsorId":"45678",  
"fiserv.identity.billpay.originator":"Sponsor",  
"fiserv.identity.billpay.channel":"Desktop"
}
```
#### Request Form Data

client\_id: yourSpecificUserMaintenanceClientId  
grant\_type: ChallengerGrant  
JWT: eyJhb…  
tenantId: 45678

#### Response

```json
{
"access_token":"ee5612220a264169bbb4d42bd88413fa92327a894734031f7f619e8e12dba7d0",
"expires_in":600,
"token_type":"Bearer",
"refresh_token":"d09234ab17c16af6d86cee5355cc91ca7fc4a4070bc80631a8fd7e857ade837f",
}
```

### Sample API Usage: Authentication Request for CheckFree Next User

This grant is scoped to a CheckFree Next user.

#### JWT Payload
```json
{  
"iss":"Second National Bank",  
"aud":"Fiserv",  
"iat":"1543861896",  
"jti":"b5d2bcef-0a8b-4218-9f09-cc9c8a24d98c",  
"exp":"1543862196",  
"fiserv.identity.billpay.sponsorId":"45678",  
"fiserv.identity.billpay.originator":"Sponsor",  
"fiserv.identity.billpay.channel":"Desktop",  
"fiserv.identity.checkfreeNext.userId":"b5d2bcef0a8b42189f09cc9c8a24d98c"  
}
```
#### Request Form Data

client\_id: yourSpecificClientId  
grant\_type: ChallengerGrant  
JWT: eyJhb…  
tenantId: 45678

#### Response

```json
{
"access_token":"ee5612220a264169bbb4d42bd88413fa92327a894734031f7f619e8e12dba7d0",
"expires_in":600,
"token_type":"Bearer",
"refresh_token":"d09234ab17c16af6d86cee5355cc91ca7fc4a4070bc80631a8fd7e857ade837f",
}
```

## Start Making Requests

After authentication you will be given an access token and a refresh
token. The access token should be used in the authorization header:
“Authorization: Bearer &lt;access\_token&gt;”.

The refresh token should be used within the configurable refresh
interval (set during client implementation, default 10 minutes).

## Refresh an Access Token

To renew the access token the standard OAuth refresh token process
should be used.

### Method and Endpoint

| POST | /connect/token |
|------|----------------|

### Request

| Parameter      | Req | Param Type | Data Type        | Description                                                 |
|-----------|-----|-------|--------|------------------------------------------|
| client\_id     | Req | body       | string           | Fiserv-provided ID; will be assigned during implementation. |
| refresh\_token | Req | body       | string           | Token received from initial grant or previous refresh.      |
| grant\_type    | Req | body       | "refresh\_token" |                                                             |

### Response

| Parameter      | Req | Data Type | Description                                                      |
|------------|----|----------|-----------------------------------------------|
| access\_token  | Req | string    | The access token string as issued by the authorization server.   |
| expires\_in    | Req | integer   | The duration of time the access token is granted for.            |
| token\_type    | Req | string    | The type of token.                                               |
| refresh\_token | Req | string    | A refresh token that can be used to obtain another access token. |

### Sample API Usage

#### Request Form Data

client\_id: yourSpecificClientId  
refresh\_token:55dc7e5…  
grant\_type:refresh\_token

#### Response

```json
{
"access_token": "5402665…",
"expires_in": 600,
"token_type": "Bearer",
"refresh_token": "26346ca9d926a792…"
}
```

## Session Termination

To end a session before the token expires, initiate a token revocation
request:

### Method and Endpoint

| POST | /connect/revocation |
|------|---------------------|

### Request

| Parameter         | Req | Param Type | Data Type       | Description                                                 |
|-----------|-----|-------|--------|------------------------------------------|
| client\_id        | Req | body       | string          | Fiserv-provided ID; will be assigned during implementation. |
| token             | Req | body       | string          | Token to revoke.                                            |
| token\_type\_hint | Req | body       | "access\_token" |                                                             |

### Response

HTTP response 200 – OK returned for successful requests.

### Sample API Usage

#### Request Form Data

client\_id:yourSpecificClientId  
token:ee5612220a264169bbb4d42bd88413fa92327a894734031f7f619e8e12dba7d0  
token\_type\_hint:access\_token

#### Response

| 200 OK |
|--------|

## Requirements and Limitations

-   The minimum TLS version is 1.2.

-   All API requests must be made over HTTPS. Calls made over plain HTTP
    will fail. API requests without authentication will also fail.

For more information, see *CheckFree Next Design Requirements, Best
Practices, and Performance Considerations*.

### Caching

In order to deliver an efficient implementation, clients must implement
caching. Data returned from GET operations must be cached within the
client’s implementation.

When data is changed, clients should invalidate all appropriate
resources that have been cached. As an example, after a POST
transaction, Payees, Transactions, Bank Accounts, Bills, and Todos may
all be invalidated.

### FraudNet Data Requirements

Clients using FraudNet must provide the following for each session:
user's IP address, user-agent, and channel.

#### IP Address and User-Agent

If calls are originating directly from an end-user device (via native
app or JavaScript calls), the IP address and user-agent data will be
populated automatically.

For calls originating from a client server, the following HTTP headers
should be set:

-   Source-Ip-Address: This should be the IP address of the device that
    the user was originating bill pay transactions from.

-   User-Agent: This is the standard user-agent field and should be
    overridden with the user agent of the device/client the user was
    originating bill pay transactions from.

#### Channel

In the JWT used to obtain an access token, the client must include
fiserv.identity.billpay.channel in the payload. The valid values are
Desktop and Mobile.

## Versioning and Backwards Compatibility

All APIs will work as released until a breaking change is required, at
which time a new version, or alternative resource, will be created. The
existing resource will continue to be maintained until such time as
Fiserv announces that the API is deprecated and subsequently retired
with sufficient notice to clients that they can move to the new version.

### Backwards Compatibility

In order to limit change to consumers of the API, Fiserv will make every
effort to ensure backwards compatibility when new features are added.
This will be achieved by returning new properties and optional
inputs/query parameters. Clients need to understand the following
guidelines:

-   If a property is a primitive type and Fiserv documentation provides
    no value restrictions, a client MUST NOT assume any restrictions in
    values.

-   If a property is not declared as required to be returned, a client
    MUST NOT depend on it being present.

-   New properties may be added, but these new properties will not
    change how existing properties are used.

-   If new properties are added, the client MUST assume they can be
    returned, though the client need not use them.

-   Clients MUST provide default “fallback” error handling for
    unexpected error codes in all cases. Fiserv may return new error
    codes to allow for unexpected error conditions.

### Versioning

The APIs will be versioned with semantic versioning (semver.org). Fiserv
will increment a version number under the following rules:

-   MAJOR version when incompatible API changes are made.

-   MINOR version when functionality is added in a backwards-compatible
    manner.

-   PATCH version when backwards-compatible bug fixes are made.

Only major version changes will eventually require depreciation of
previous versions.

## Response Codes

CheckFree Next APIs use standard HTTP response codes to indicate the
success or failure of an API request.

| HTTP Status Code            | Description                                                                                                                                                                                |
|------------|------------------------------------------------------------|
| 200 - OK                    | Standard response for successful HTTP requests.                                                                                                                                            |
| 201 - Created               | The request has been fulfilled, resulting in the creation of a new resource.                                                                                                               |
| 204 - No content            | The server successfully processed the request, but is not returning any content.                                                                                                           |
| 307 - Redirect              | The requested resource resides temporarily under a different URI.                                                                                                                          |
| 400 - Bad Request           | The server cannot process the request due to an apparent client error. We will also return the specific reason why you received this error based on the action you were trying to perform. |
| 401 - Unauthorized          | Authentication is required and has failed or has not yet been provided (for example, an invalid key).                                                                                      |
| 403 - Forbidden             | The request was valid, but the server is refusing action.                                                                                                                                  |
| 404 – Not found             | The server could not find what was requested.                                                                                                                                              |
| 500 - Internal server error | Internal server error                                                                                                                                                                      |

## Important to Know

-   In general, CheckFree Next APIs accept alphanumeric input for
    freeform fields. In some cases, input is restricted to prevent
    malicious code from being inserted into the system.

-   The format for all requests and responses is JSON.

-   A request for a list returns an array. There is an empty array if
    there is no data to return.

-   Transaction dates - Transactions are currently processed by Fiserv’s
    payment processing systems located in the U.S. Eastern Time Zone.
    Transaction dates reflect where the payment processing system is
    located.

-   Field encoding – Any consumer of CheckFree Next must take proper
    care to encode and sanitize any user-entered fields that are
    returned.

-   Resource modification – When a field needs to be explicitly removed,
    an explicit null value must be sent in the request. In accordance
    with PATCH resources, if a field is not included, it is not
    modified.

# 

# REST API Reference

CheckFree
Next offers categories of APIs as shown in the following table.

>**Tip:** After you follow a link, press Alt + Left Arrow (Windows) to go
back to the previous location.

<table>
<colgroup>
<col style="width: 18%" />
<col style="width: 48%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>API Name</strong></th>
<th><strong>Description</strong></th>
<th><strong>Supported Operations</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><a href="#health">Health</a></td>
<td>Perform health checks.</td>
<td>GET</td>
</tr>
<tr class="even">
<td><a href="#users">Users</a></td>
<td>Provide access to a consumer’s details.</td>
<td><p>GET, POST, PATCH, DELETE for managed users</p>
<p>GET, PATCH</p>
<p>GET, POST for Consents sub-resource</p>
<p>GET for features</p>
<p>GET for sensitiveInformation</p>
<p>POST for billDeliveryFailure sub-resource</p></td>
</tr>
<tr class="odd">
<td><a href="#potential-payees">PotentialPayees</a></td>
<td>Determine Bill Discovery eligibility and provide access to all
potential payees found for a consumer.</td>
<td><p>GET, PUT</p>
<p>GET for eligibility sub-resource</p>
<p>GET for payeeAccountNumber sub-resource</p></td>
</tr>
<tr class="even">
<td><a href="#payees">Payees</a></td>
<td>Provide access to all payees added by a consumer within bill
pay.</td>
<td><p>GET, POST, PATCH, DELETE</p>
<p>GET for payeeAccountNumber sub-resource</p>
<p>GET for automaticTransactionOptions sub‑resource</p></td>
</tr>
<tr class="odd">
<td><a href="#payee-groups">PayeeGroups</a></td>
<td>Provide access to payee groups for a consumer.</td>
<td>GET, POST, PUT, DELETE</td>
</tr>
<tr class="even">
<td><a href="#merchants">Merchants</a></td>
<td>Provide access to all merchants in the managed merchant
directory.</td>
<td>GET</td>
</tr>
<tr class="odd">
<td><a href="#probable-merchants">ProbableMerchants</a></td>
<td>Provide access to probable merchants for a consumer.</td>
<td>GET</td>
</tr>
<tr class="even">
<td><a href="#reminders">Reminders</a></td>
<td>Provide access to reminder models for a consumer.</td>
<td>GET, POST, PATCH, DELETE</td>
</tr>
<tr class="odd">
<td><a href="#todos">ToDos</a></td>
<td>Provide access to all to-do items for a consumer.</td>
<td>GET, PATCH</td>
</tr>
<tr class="even">
<td><a href="#bills">Bills</a></td>
<td>Provide access to all bills due for a consumer.</td>
<td><p>GET, PATCH</p>
<p>GET for detail sub-resource</p></td>
</tr>
<tr class="odd">
<td><a href="#e-bill-activation">eBill Activation</a></td>
<td>Provide access to e-bill capability for a payee.</td>
<td><p>GET, POST, DELETE</p>
<p>PATCH for SuppressPaper sub-resource</p>
<p>GET for ebillCapability</p></td>
</tr>
<tr class="even">
<td><a href="#bankaccounts">BankAccounts</a></td>
<td>Provide access to all bank accounts set up for use with bill pay for
a consumer.</td>
<td><p>GET, POST, PATCH, DELETE for managed users</p>
<p>GET, PATCH for consumers</p>
<p>GET for accountNumber sub-resource</p>
<p>GET for financialInstitution</p></td>
</tr>
<tr class="odd">
<td><a href="#cardaccounts">Card Accounts</a></td>
<td>Provide access to all card accounts set up for use with bill pay for
a consumer.</td>
<td><p>GET, POST, PATCH, DELETE</p>
<p>GET for CardAccountAddLimits sub-resource</p></td>
</tr>
<tr class="even">
<td><a href="#transactioncalendar">TransactionCalendar</a></td>
<td>Provide access to the transaction calendar applicable to a
payee.</td>
<td>GET</td>
</tr>
<tr class="odd">
<td><a href="#transactions">Transactions</a></td>
<td>Provide access to all transactions for a consumer.</td>
<td>GET, POST, PATCH</td>
</tr>
<tr class="even">
<td><a href="#automatictransactions">AutomaticTransactions</a></td>
<td>Provide access to all automatic payment plans for a payee.</td>
<td>GET, POST, PATCH, DELETE</td>
</tr>
</tbody>
</table>

# 

# Health

This API allows the calling application to perform a health check. This
can be used to test connectivity and ensure availability of the
application.

## Get Health Check Status

The health check pings an endpoint. If successful, a 200 OK response is
returned with the text “Application Up”.

### Method and Endpoint

| GET | /api/Health |
|------|------------|

### Response

HTTP response 200 – OK returned for successful requests.

### Sample API Usage

#### Request URL

| https://api-checkfreenext-cert.fiservapps.com/api/Health |
|----------------------------------------------------------|

#### Response

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th><p>200 OK</p>
<p>Application Up</p></th>
</tr>
</thead>
<tbody>
</tbody>
</table>

# 

# Users

A user is a consumer who is subscribed to use bill pay services.

Note

Fiserv recommends an API-only approach for enrolling or updating
consumers. CheckFree Next APIs should not be used in combination with
SIS for user management.

This set of APIs allows the calling application to:

-   [Enroll a managed user](#enroll-a-user-managed-user)

-   [Update a managed user’s profile](#update-a-user-managed-user)

-   [Update a consumer's profile](#update-a-user)

-   [Delete a managed user](#delete-a-user-managed-user)

-   [Get a managed user’s profile
    information](#get-user-information-managed-user)

-   [Get consumer information](#get-user-information)

-   [Record consumer’s consent
    information](#record-users-consent-information)

-   [Return consumer's consent
    information](#return-users-consent-information)

-   [Get feature capability information](#get-features)

-   [Get sensitive information](#get-sensitive-information)

-   [Post eBill Notification Email Delivery
    Failure](#post-ebill-notification-email-delivery-failure)

>**Tip:**
After you follow a link, press Alt + Left Arrow (Windows) to go back to
the previous location.

## Enroll a User (Managed User)

The User Post API enables the financial institution (FI) to enroll new
users in the Fiserv system for bill payment. The enrollment API is a
single call to add a consumer’s profile information. This API is
constructed such that the consumer being added is “trusted” and the
assumption is that the client FI adding the consumer has verified the
consumer’s identity. As such, this API performs no identity
verification.

This API may also be used to reactivate an existing user who is
currently inactive.

Note

For the authentication request, the client\_id must be configured for
UserMaintenance. Do not send fiserv.identity.billpay.subscriberId; see
[Sample API Usage: Authentication Request for User
Maintenance](#sample-api-usage-authentication-request-for-user-maintenance).

**When to use:** The FI determines the criteria for enrolling consumers
in bill payment. This may be done as part of enabling online banking
with the institution, or may require a consumer request for initiation,
typically through the online banking application. When the criteria has
been met, the FI should initiate the User Post request to Fiserv, and
the consumer will be enabled to access the bill payment application.

Examples:

-   As part of enabling online banking, enable the account for bill
    payment.

-   From within online banking, enable the consumer to elect to use bill
    payment.

### Method and Endpoint

| POST | /api/v2/me/Users |
|------|------------------|

### Request

Note

Work with Fiserv Professional Services to make sure that the values of
any optional parameters in the user profile are consistent with your
sponsor setup.

<table>
<colgroup>
<col style="width: 19%" />
<col style="width: 4%" />
<col style="width: 8%" />
<col style="width: 10%" />
<col style="width: 56%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Param Type</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>billingClass</td>
<td>Cond</td>
<td>body</td>
<td>string</td>
<td><p>Length: 1-9</p>
<p>Pattern: ^[a-zA-Z0-9]+$</p>
<p>If supplied, this field must contain a valid alphanumeric billing
class as supplied by Fiserv during implementation. Condition: This field
is required if Fiserv collects consumer fees on behalf of the
client.</p></td>
</tr>
<tr class="even">
<td>businessName</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td><p>Length: 1-40</p>
<p>Name of the business. Provide businessName if this is a
business,</p></td>
</tr>
<tr class="odd">
<td>consumerTier</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td><p>Tier level of consumer. Valid values:</p>
<p>1 (default)<br />
2<br />
3</p>
<p>Pattern: (1|2|3){1}$</p></td>
</tr>
<tr class="even">
<td>isAllowedToSolicit</td>
<td>Opt</td>
<td>body</td>
<td>boolean</td>
<td><p>Indicates whether or not the consumer may be solicited by Fiserv.
This field allows a client to comply with state laws permitting a
resident to prohibit solicitation in writing or by telephone. Valid
values:</p>
<p>true – Yes, Fiserv can send information about additional products or
services to this consumer.</p>
<p>false – No, Fiserv may not send solicitation information. This is the
default.</p></td>
</tr>
<tr class="odd">
<td>isEmployee</td>
<td>Opt</td>
<td>body</td>
<td>boolean</td>
<td><p>Identifies whether a consumer is an employee of the client. Valid
values:</p>
<p>true – Yes, an employee.</p>
<p>false – No, not an employee. This is the default.</p></td>
</tr>
<tr class="even">
<td>sponsorBillingCategory</td>
<td>Cond</td>
<td>body</td>
<td>string</td>
<td><p>Length: 1–2 (only alphanumeric characters allowed)</p>
<p>Pattern: ^[a-zA-Z0-9]+$</p>
<p>Clients that are doing their own billing but want to differentiate
between consumers can use this field to communicate the consumer
category to Fiserv. Condition: Based on sponsor setup.</p></td>
</tr>
<tr class="odd">
<td>securityQuestion</td>
<td>Cond</td>
<td>body</td>
<td>string</td>
<td><p>Security question prompt.</p>
<p>Valid values: YourHighSchoolName, YourMothersMaidenName,
YourFathersMiddleName, YourCityOfBirth</p>
<p>Condition: If Fiserv is providing first-tier Customer Care support
for the client, securityQuestion and securityAnswer are required at
enrollment.</p></td>
</tr>
<tr class="even">
<td>securityAnswer</td>
<td>Cond</td>
<td>body</td>
<td>string</td>
<td><p>Length: 4-32</p>
<p>An identifier used by the consumer for security purposes.</p>
<p>Condition: If Fiserv is providing first-tier Customer Care support
for the client, securityQuestion and securityAnswer are required at
enrollment.</p></td>
</tr>
<tr class="odd">
<td>password</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td><p>Length: 8</p>
<p>Consumer's password.</p>

`Pattern:((?=.*[@#$&amp;*])(?=.*[a-zA-Z\d])|(?=.*\d)(?=.*[a-zA-Z])(?=.*[a-zA-Z0-9@#$&amp;*])|(?=.*[a-z])(?=.*[A-Z])(?=.*[a-zA-Z0-9@#$&amp;*]))[a-zA-Z0-9@#$&amp;*]+`
<p>Character types allowed: Uppercase letters, lowercase letters,
numbers, and special/punctuation characters (i.e., @#$&amp;*)</p>
<p>Character type required combinations: Must contain at least two of
the four character types. For example, the password "a4fs2jkl" contains
two character types (lowercase letters and numbers), and thus meets the
standard.</p>
<p>Specific Restrictions: Password CANNOT contain the user ID anywhere
within it.</p></td>
</tr>
<tr class="even">
<td>fiservBillpaySubscriberId</td>
<td>Req</td>
<td>body</td>
<td>string</td>
<td><p>Used by the user to authenticate with the user’s product(s).
Length: 2–32 (no spaces allowed)</p>
<p>Pattern: ^[A-Z0-9_(){}&amp;@!+#.'$,%^*-]*</p>
<p>Best practices: This ID should contain at least nine characters. Do
not include special characters (such as # or &amp;). Do not use a Social
Security number.</p></td>
</tr>
<tr class="odd">
<td>fiservBillpayExternalSubscriberId</td>
<td>Req</td>
<td>body</td>
<td>string</td>
<td><p>A unique customer ID, different from the
fiservBillpaySubscriberId, that is used for logging the user in to a
Fiserv product. Length: 2–48 (no spaces allowed)</p>
<p>Pattern: ^[a-zA-Z0-9_(){}&amp;@!+#.'$,%^*-]*</p>
<p>Condition: Required if any of the consumer’s products require it.</p>
<p>Best practices: This ID should contain at least nine characters. Do
not include special characters (such as # and &amp;). Do not use a
Social Security number.</p></td>
</tr>
<tr class="even">
<td>name</td>
<td>Req</td>
<td>body</td>
<td><a href="#name">Name</a></td>
<td>Consumer’s name.</td>
</tr>
<tr class="odd">
<td>contactEndPoints</td>
<td>Req</td>
<td>body</td>
<td><a href="#contactendpoint">Contact<br />
Endpoint</a></td>
<td>Contact details of the consumer, such as the consumer’s email
address, phone numbers, and address.</td>
</tr>
<tr class="even">
<td>identityValidationInformation</td>
<td>Opt</td>
<td>body</td>
<td><a href="#identityvalidationinformation">IdentityValidation<br />
Information</a></td>
<td>Information that may be used to help confirm the consumer’s
identity.</td>
</tr>
<tr class="odd">
<td>birthDate</td>
<td>Req</td>
<td>body</td>
<td>string</td>
<td>The date of birth of the consumer. The user must be 18 years of age
or greater.<br />
Format: yyyy-MM-dd</td>
</tr>
<tr class="even">
<td>category</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td><p>Client-assigned free-form category that identifies the group the
consumer belongs to. Length: 1-32</p>
<p>Pattern: ^[a-zA-Z0-9_(){}&amp;@!+#.'$,%^ *-]*</p></td>
</tr>
<tr class="odd">
<td>taxId</td>
<td>Req</td>
<td>body</td>
<td>string</td>
<td><p>This is the consumer’s ID for tax purposes, either the user’s
Social Security number or the Federal Tax Identification number for
their business. Max length: 9</p>
<p>Pattern:
^(?!111111111|222222222|333333333|444444444|555555555|777777777|888888888|123456789|<br />
012345678)(?!666|000|9\d{2})\d{3}(?!00)\d{2}(?!0{4})\d{4}$</p>
<p>Cannot contain special characters, spaces, or hyphens.</p>
<p>Must never contain all zeros in any group (000######, ###00####,
#####0000).</p>
<p>Must never contain 666 or 900–999 in the first digit group
(666######, 900######, 901######, etc.).</p>
<p>Must not be all repeated digits or a descending sequence.</p></td>
</tr>
<tr class="even">
<td>locale</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td><p>The language code and associated language location in one
combined field. Format: XX-XX (language code and language country
separated by hyphen).</p>
<p>Valid values for language code: “EN”,”ES”</p>
<p>Pattern: [A-Za-z]{2}-[A-Za-z]{2}$</p>
<p>“US” is the default value for language country.</p></td>
</tr>
<tr class="odd">
<td>occupation</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td><p>The activity a user spends time performing to earn a living.
Length: 1-50</p>
<p>Pattern: ^[a-zA-Z0-9_(){}&amp;@!+#.'$,%^ *-]*</p></td>
</tr>
<tr class="even">
<td>userTimeZone</td>
<td>Cond</td>
<td>body</td>
<td>integer</td>
<td><p>The time zone for the consumer. Defaults to “-05” for eastern
standard time (EST) unless a value is provided.</p>
<p>Condition: If Fiserv provides first-tier Customer Care for the
client, this field is required.</p></td>
</tr>
</tbody>
</table>

### Response

| Parameter | Req  | Data Type                 | Description                                                                                                                                  |
|------------|-----|----------|-----------------------------------------------|
| data      | Cond | [BaseModel](#basemodel)   | Response data. Condition: Always returned for successful response.                                                                           |
| result    | Cond | [ResultType](#resulttype) | Result associated with the request. Condition: Only returned when the request fails. No content returned for success (HTTP status code 201). |

### Sample API Usage

#### Request Body

```json
{
"fiservBillpaySubscriberId": "20190116094838423869",
"fiservBillpayExternalSubscriberId": "20190116094838423869EXT",
"name": {
"firstName": "Jeremiah",
"lastName": "Hands",
"middleName": "Rem",
"nickname": null,
"prefix": null
},
"contactEndPoints": [
{
"status": "Active",
"endPointType": "Address",
"address": {
"addressType": "USAddress",
"usAddress": {
"address1": "6000 Perimeter Dr",
"address2": "Ste 3",
"address3": "Mailbox 10",
"city": "Dublin",
"state": "OH",
"zipCode": "43016"
}
},
"phone": null,
"email": null,
"defaultEndpoint": true
},
{
"status": "Active",
"endPointType": "Phone",
"address": null,
"phone": {
"phoneType": "Mobile",
"value": "5558675309"
},
"email": null,
"defaultEndpoint": false
},
{
"status": "Active",
"endPointType": "Phone",
"address": null,
"phone": {
"phoneType": "Day",
"value": "5058675309"
},
"email": null,
"defaultEndpoint": true
},
{
"status": "Active",
"endPointType": "Email",
"address": null,
"phone": null,
"email": {
"value": "test@test-mail.com"
},
"defaultEndpoint": true
}
],
"identityValidationInformation": {
"idType": "StateIssuedDriverLicense",
"idNumber": "OH1234567",
"idIssuingState": "OH",
"idIssuingCountry": "US"
},
"birthDate": "1980-01-01",
"category": "BaseUser",
"taxId": "789324698",
"locale": "en-US",
"userTimeZone": null,
"occupation": "Developer"
"billingClass": null,
"businessName": "CheckFreeNext",
"consumerTier": null,
"isAllowedToSolicit": true,
"isEmployee": true,
"sponsorBillingCategory": null,
"password": null,
"securityQuestion": "YourHighSchoolName",
"securityAnswer": "OxfordSchool"
}
```

#### Response

```json
{
"data": {
"self": "/api/v2/me/users/95da4acf9d19e911a83200505689ef1f",
"id": "95da4acf9d19e911a83200505689ef1f"
},
"result": {
"success": true
}
}
```

## Update a User (Managed User)

The User Patch API enables the financial institution (FI) to update
users in the Fiserv system for bill payment. This enables Fiserv to stay
in synch with the FI's system of record for that consumer’s profile, and
payments made will use the consumer’s most current profile information.

Note

For the authentication request, the client\_id must be configured for
UserMaintenance. Do not send fiserv.identity.billpay.subscriberId; see
[Sample API Usage: Authentication Request for User
Maintenance](#sample-api-usage-authentication-request-for-user-maintenance).

**When to use:** The FI's system of record for consumer profile data
should trigger a call to User Patch when an update to a consumer’s
profile occurs for a consumer who has been enrolled at Fiserv.

Examples:

-   User updates their profile information with the FI, through any of
    the FI channels (such as online or mobile banking).

-   User updates their primary email address to be used for bill payment
    email correspondence.

### Method and Endpoint

| PATCH | /api/v2/me/Users/{userId}|
|------|---------------------------|

### Request

<table>
<colgroup>
<col style="width: 17%" />
<col style="width: 4%" />
<col style="width: 6%" />
<col style="width: 10%" />
<col style="width: 60%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Param Type</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>userId</td>
<td>Req</td>
<td>path</td>
<td>string</td>
<td>Identifier for the managed user.</td>
</tr>
<tr class="even">
<td>idType</td>
<td>Opt</td>
<td>query</td>
<td>string</td>
<td>Identifies the user ID type. Valid values: SubscriberId,
ExternalSubscriberId, CheckFreeNextUserId<br />
CheckFreeNextUserId is the default.</td>
</tr>
<tr class="odd">
<td>password</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td><p>Length: 8</p>
<p>Consumer's password.</p>

`Pattern:((?=.*[@#$&;*])(?=.*[a-zA-Z\d])|(?=.*\d)(?=.*[a-zA-Z])(?=.*[a-zA-Z0-9@#$&;*])|(?=.*[a-z])(?=.*[A-Z])(?=.*[a-zA-Z0-9@#$&;*]))[a-zA-Z0-9@#$&;*]+`
<p>Character types allowed: Uppercase letters, lowercase letters,
numbers, and special/punctuation characters (i.e., @#$&;*)</p>
<p>Character type required combinations: Must contain at least two of
the four character types. For example, the password "a4fs2jkl" contains
two character types (lowercase letters and numbers), and thus meets the
standard.</p>
<p>Specific Restrictions: Password CANNOT contain the user ID anywhere
within it.</p></td>
</tr>
<tr class="even">
<td>billingClass</td>
<td>Cond</td>
<td>body</td>
<td>string</td>
<td><p>Length: 1-9</p>
<p>Pattern: ^[a-zA-Z0-9]+$</p>
<p>If supplied, this field must contain a valid alphanumeric billing
class as supplied by Fiserv during implementation. Condition: This field
is required if Fiserv collects consumer fees on behalf of the
client.</p></td>
</tr>
<tr class="odd">
<td>businessName</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td><p>Length: 1-40</p>
<p>Name of the business. Provide businessName if this is a
business,</p></td>
</tr>
<tr class="even">
<td>consumerTier</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td><p>Tier level of consumer. Valid values:</p>
<p>1<br />
2<br />
3</p>
<p>Pattern: (1|2|3){1}$</p></td>
</tr>
<tr class="odd">
<td>isAllowedToSolicit</td>
<td>Opt</td>
<td>body</td>
<td>boolean</td>
<td><p>Indicates whether or not the consumer may be solicited by Fiserv.
This field allows a client to comply with state laws permitting a
resident to prohibit solicitation in writing or by telephone. Valid
values:</p>
<p>true – Yes, Fiserv can send information about additional products or
services to this consumer.</p>
<p>false – No, Fiserv may not send solicitation information. This is the
default.</p></td>
</tr>
<tr class="even">
<td>isEmployee</td>
<td>Opt</td>
<td>body</td>
<td>boolean</td>
<td><p>Identifies whether a consumer is an employee of the client. Valid
values:</p>
<p>true – Yes, an employee.</p>
<p>false – No, not an employee.</p></td>
</tr>
<tr class="odd">
<td>sponsorBillingCategory</td>
<td>Cond</td>
<td>body</td>
<td>string</td>
<td><p>Length: 1–2 (only alphanumeric characters allowed)</p>
<p>Pattern: ^[a-zA-Z0-9]+$</p>
<p>Clients that are doing their own billing but want to differentiate
between consumers can use this field to communicate the consumer
category to Fiserv. Based on client setup.</p></td>
</tr>
<tr class="even">
<td>name</td>
<td>Opt</td>
<td>body</td>
<td><a href="#name">Name</a></td>
<td>Consumer’s name.</td>
</tr>
<tr class="odd">
<td>contactEndPoints</td>
<td>Opt</td>
<td>body</td>
<td><a href="#contactendpoint">Contact<br />
Endpoint</a></td>
<td>Contact details of the consumer, such as the consumer’s email
address, phone numbers, and address.</td>
</tr>
<tr class="even">
<td>birthDate</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td>The date of birth of the consumer. The user must be 18 years of age
or greater.<br />
Format: yyyy-MM-dd</td>
</tr>
<tr class="odd">
<td>category</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td><p>Client-assigned free-form category that identifies the group the
consumer belongs to. Length: 1-32</p>
<p>Pattern: ^[a-zA-Z0-9_(){}&amp;@!+#.'$,%^ *-]*</p></td>
</tr>
<tr class="even">
<td>identityValidationInformation</td>
<td>Opt</td>
<td>body</td>
<td><a href="#identityvalidationinformation">IdentityValidation<br />
Information</a></td>
<td>Information that may be used to help confirm the consumer’s
identity.</td>
</tr>
<tr class="odd">
<td>locale</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td><p>The language code and associated language location in one
combined field. Format: XX-XX (language code and language country
separated by hyphen).</p>
<p>Valid values for language code: “EN”,”ES”</p>
<p>Pattern: [A-Za-z]{2}-[A-Za-z]{2}$</p>
<p>“US” is the default value for language country.</p></td>
</tr>
<tr class="even">
<td>occupation</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td><p>The activity a user spends time performing to earn a living.
Length: 1-50</p>
<p>Pattern: ^[a-zA-Z0-9_(){}&amp;@!+#.'$,%^ *-]*</p></td>
</tr>
<tr class="odd">
<td>taxId</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td><p>This is the consumer’s ID for tax purposes, either the user’s
Social Security number or the Federal Tax Identification number for
their business. Max length: 9</p>
<p>Pattern:
^(?!111111111|222222222|333333333|444444444|555555555|777777777|888888888|123456789|012345678)<br />
(?!666|000|9\d{2})\d{3}(?!00)\d{2}(?!0{4})\d{4}$</p>
<p>Cannot contain special characters, spaces, or hyphens.</p>
<p>Must never contain all zeros in any group (000######, ###00####,
#####0000).</p>
<p>Must never contain 666 or 900–999 in the first digit group
(666######, 900######, 901######, etc.).</p>
<p>Must not be all repeated digits or a descending sequence.</p></td>
</tr>
<tr class="even">
<td>timeZone</td>
<td>Opt</td>
<td>body</td>
<td>integer</td>
<td><p>The time zone for the consumer. Defaults to “-05” for eastern
standard time (EST) unless a value is provided.</p>
<p>If Fiserv provides first-tier Customer Care for the client, this
field is required.</p></td>
</tr>
</tbody>
</table>

### Response

| Parameter | Req  | Data Type                 | Description                                                                                                                  |
|------------|-----|----------|-----------------------------------------------|
| result    | Cond | [ResultType](#resulttype) | Result information. Condition: Only returned when the request fails. No content returned for success (HTTP status code 204). |

### Sample API Usage

#### Request Body

```json
{
"name": {
"firstName": "Deandre",
"lastName": "Davis",
"middleName": "kf",
"nickname": "zigzag Zuri"
},
"contactEndPoints": [
{
"status": "Active",
"endPointType": "Address",
"address": {
"addressType": "USAddress",
"usAddress": {
"address1": "6000 Perimeter Dr",
"address2": "Ste 3",
"address3": "Mailbox 1034",
"city": "Dublin",
"state": "OH",
"zipCode": "43016"
}
},
"defaultEndpoint": true
},
{
"status": "Active",
"endPointType": "Phone",
"phone": {
"phoneType": "Evening",
"value": "3314856634"
},
"defaultEndpoint": true
},
{
"status": "Active",
"endPointType": "Email",
"email": {
"value": "heftygear@meager.gov"
},
"defaultEndpoint": false
}
],
"birthDate": "1996-09-30",
"category": "qkmwtv",
"identityValidationInformation": {
"idType": "StateIssuedDriverLicense",
"idNumber": "4173351777",
"idIssuingState": "OH",
"idIssuingCountry": "US"
},
"locale": "fr-CA",
"occupation": "ekumpefnyzaontw",
"taxId": "138282116",
"timeZone": -4
"billingClass": null,
"businessName": "CheckFreeNext",
"consumerTier": null,
"isAllowedToSolicit": true,
"isEmployee": true,
"sponsorBillingCategory": null,
"password": "R@nDoM21"
}
```

#### Response

| 204 No Content |
|----------------|

## Update a User

The User Patch API updates the consumer profile in the Fiserv system for
the user associated with the issued bearer token.

Note

For the authentication request, see [Sample API Usage: Authentication
Request for
Subscriber](#sample-api-usage-authentication-request-for-subscriber).

### Method and Endpoint

| PATCH | /api/v1/me |
|------|-------------|

### Request

<table>
<colgroup>
<col style="width: 17%" />
<col style="width: 4%" />
<col style="width: 6%" />
<col style="width: 10%" />
<col style="width: 60%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Param Type</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>name</td>
<td>Opt</td>
<td>body</td>
<td><a href="#name">Name</a></td>
<td>Consumer’s name.</td>
</tr>
<tr class="even">
<td>contactEndPoints</td>
<td>Opt</td>
<td>body</td>
<td><a href="#contactendpoint">Contact<br />
Endpoint</a></td>
<td>Contact details of the consumer, such as the consumer’s email
address, phone numbers, and address.</td>
</tr>
<tr class="odd">
<td>birthDate</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td>The date of birth of the consumer. The user must be 18 years of age
or greater.<br />
Format: yyyy-MM-dd</td>
</tr>
<tr class="even">
<td>category</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td><p>Client-assigned free-form category that identifies the group the
consumer belongs to. Length: 1-32</p>
<p>Pattern: ^[a-zA-Z0-9_(){}&amp;@!+#.'$,%^ *-]*</p></td>
</tr>
<tr class="odd">
<td>identityValidationInformation</td>
<td>Opt</td>
<td>body</td>
<td><a href="#identityvalidationinformation">IdentityValidation<br />
Information</a></td>
<td>Information that may be used to help confirm the consumer’s
identity.</td>
</tr>
<tr class="even">
<td>locale</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td><p>The language code and associated language location in one
combined field. Format: XX-XX (language code and language country
separated by hyphen).</p>
<p>Valid values for language code: “EN”,”ES”</p>
<p>Pattern: [A-Za-z]{2}-[A-Za-z]{2}$</p>
<p>“US” is the default value for language country.</p></td>
</tr>
<tr class="odd">
<td>occupation</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td><p>The activity a user spends time performing to earn a living.
Length: 1-50</p>
<p>Pattern: ^[a-zA-Z0-9_(){}&amp;@!+#.'$,%^ *-]*</p></td>
</tr>
<tr class="even">
<td>taxId</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td><p>This is the consumer’s ID for tax purposes, either the user’s
Social Security number or the Federal Tax Identification number for
their business. Max length: 9</p>
<p>Pattern:
^(?!111111111|222222222|333333333|444444444|555555555|777777777|888888888|123456789|012345678)<br />
(?!666|000|9\d{2})\d{3}(?!00)\d{2}(?!0{4})\d{4}$</p>
<p>Cannot contain special characters, spaces, or hyphens.</p>
<p>Must never contain all zeros in any group (000######, ###00####,
#####0000).</p>
<p>Must never contain 666 or 900–999 in the first digit group
(666######, 900######, 901######, etc.).</p>
<p>Must not be all repeated digits or a descending sequence.</p></td>
</tr>
<tr class="odd">
<td>timeZone</td>
<td>Opt</td>
<td>body</td>
<td>integer</td>
<td><p>The time zone for the consumer. Defaults to “-05” for eastern
standard time (EST) unless a value is provided.</p>
<p>If Fiserv provides first-tier Customer Care for the client, this
field is required.</p></td>
</tr>
</tbody>
</table>

### Response

| Parameter | Req  | Data Type                 | Description                                                                                                                  |
|------------|-----|----------|-----------------------------------------------|
| result    | Cond | [ResultType](#resulttype) | Result information. Condition: Only returned when the request fails. No content returned for success (HTTP status code 204). |

### Sample API Usage

#### Request Body

```json
{
"name": {
"firstName": "Deandre",
"lastName": "Davis",
"middleName": "kf",
"nickname": "zigzag Zuri"
},
"contactEndPoints": [
{
"status": "Active",
"endPointType": "Address",
"address": {
"addressType": "USAddress",
"usAddress": {
"address1": "6000 Perimeter Dr",
"address2": "Ste 3",
"address3": "Mailbox 1034",
"city": "Dublin",
"state": "OH",
"zipCode": "43016"
}
},
"defaultEndpoint": true
},
{
"status": "Active",
"endPointType": "Phone",
"phone": {
"phoneType": "Evening",
"value": "3314856634"
},
"defaultEndpoint": true
},
{
"status": "Active",
"endPointType": "Email",
"email": {
"value": "heftygear@meager.gov"
},
"defaultEndpoint": false
}
],
"birthDate": "1996-09-30",
"category": "qkmwtv",
"identityValidationInformation": {
"idType": "StateIssuedDriverLicense",
"idNumber": "4173351777",
"idIssuingState": "OH",
"idIssuingCountry": "US"
},
"locale": "fr-CA",
"occupation": "ekumpefnyzaontw",
"taxId": "138282116",
"timeZone": -4
}
```

#### Response

| 204 No Content |
|----------------|

## Delete a User (Managed User)

The User Delete API inactivates a specific user from the bill payment
service at Fiserv. This request places the consumer in an inactive
status, and all future payments will not be processed. To be
inactivated, the consumer must have been enrolled at Fiserv and not
currently inactivated.

Note

For the authentication request, the client\_id must be configured for
UserMaintenance. Do not send fiserv.identity.billpay.subscriberId; see
[Sample API Usage: Authentication Request for User
Maintenance](#sample-api-usage-authentication-request-for-user-maintenance).

**When to use:** Use the User Delete API if the consumer indicates they
no longer want to utilize bill payment services, or if the consumer
closes their account(s) associated with the FI's online banking
application. The client must send Fiserv the User Delete request to
ensure that no future payments will be generated for that consumer.

Examples:

-   The consumer closes their account(s) with the FI such that no bill
    pay eligible accounts remain.

-   The consumer makes a selection to disable the bill payment service.

### Method and Endpoint

| DELETE | /api/v2/me/Users/{userId} |
|------|-----------------------------|

### Request

<table>
<colgroup>
<col style="width: 16%" />
<col style="width: 3%" />
<col style="width: 8%" />
<col style="width: 7%" />
<col style="width: 63%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Param Type</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>userId</td>
<td>Req</td>
<td>path</td>
<td>string</td>
<td>Identifier for the user.</td>
</tr>
<tr class="even">
<td>idType</td>
<td>Opt</td>
<td>query</td>
<td>string</td>
<td>Identifies the user ID type. Valid values: SubscriberId,
ExternalSubscriberId, CheckFreeNextUserId<br />
CheckFreeNextUserId is the default.</td>
</tr>
</tbody>
</table>

### Response

| Parameter | Req  | Data Type                 | Description                                                                                                                                  |
|------------|------|-------|-----------------------------------------------|
| result    | Cond | [ResultType](#resulttype) | Result associated with the request. Condition: Only returned when the request fails. No content returned for success (HTTP status code 204). |

### Sample API Usage

#### Request URL

| https://api-checkfreenext-cert.fiservapps.com/api/v2/me/users/6ff131da5af2e811a83000505689ef1f |
|------------------------------------------------------------------------|

#### Response

| 204 No Content |
|----------------|

## Get User Information (Managed User)

The User Get API retrieves the current consumer profile for a managed
user from the Fiserv system. The consumer profile includes the name,
address, phone number, email, and other attributes of the consumer.

Note

For the authentication request, the client\_id must be configured for
UserMaintenance. Do not send fiserv.identity.billpay.subscriberId; see
[Sample API Usage: Authentication Request for User
Maintenance](#sample-api-usage-authentication-request-for-user-maintenance).

Because the maintenance grant is scoped to a tenant, the user ID for the
consumer must be provided in the User Get request.

### Method and Endpoint

| GET | /api/v2/me/Users/{userId} |
|-----|---------------------------|

###  Request

<table>
<colgroup>
<col style="width: 14%" />
<col style="width: 4%" />
<col style="width: 9%" />
<col style="width: 7%" />
<col style="width: 63%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Param Type</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>userId</td>
<td>Req</td>
<td>path</td>
<td>string</td>
<td>Identifier for the managed user.</td>
</tr>
<tr class="even">
<td>idType</td>
<td>Opt</td>
<td>query</td>
<td>string</td>
<td>Identifies the user ID type. Valid values: SubscriberId,
ExternalSubscriberId, CheckFreeNextUserId<br />
CheckFreeNextUserId is the default.</td>
</tr>
</tbody>
</table>

### Response

| Parameter | Req  | Data Type                 | Description                                                                                                                                  |
|------------|-----|----------|-----------------------------------------------|
| data      | Cond | [UserV2](#userv2)         | Consumer information. Condition: Only returned when the request is successful.                                                               |
| result    | Cond | [ResultType](#resulttype) | Result associated with the request. Condition: Only returned when the request fails. No content returned for success (HTTP status code 200). |

### Sample API Usage

#### Request URL

| https://api-checkfreenext-cert.fiservapps.com/api/v2/me/users/6ff131da5af2e811a83000505689ef1f |
|------------------------------------------------------------------------|

#### Response

```json

{
"data": {
"userId": "6ff131da5af2e811a83000505689ef1f",
"hasPayees": true,
"name": {
"firstName": "John",
"lastName": "Johnson",
"middleName": "M",
"nickname": "Johnny"
},
"contactEndPoints": [
{
"status": "Active",
"endPointType": "Email",
"email": {
"value": "test@abc.com"
},
"defaultEndpoint": true
},
{
"status": "Active",
"endPointType": "Phone",
"phone": {
"phoneType": "Evening",
"value": "9999999999"
},
"defaultEndpoint": false
},
{
"status": "Active",
"endPointType": "Address",
"address": {
"addressType": "USAddress",
"usAddress": {
"address1": "1187 Made Up St.",
"address2": "Apt 715",
"city": "Athens",
"state": "GA",
"zipCode": "30306"
}
},
"defaultEndpoint": false
}
],
"sensitiveInformationUri": "/api/v2/me/sensitiveInformation",
"timezone": -5,
"country": "USA",
"status": "Active",
"languageCode": "en",
"languageCountry": "US",
"locale": "en-US",
"newUser": true,
"occupation": "Accountants"
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

## Get User Information

The User Get API retrieves the current consumer profile from the Fiserv
system for the user associated with the issued bearer token. The
consumer profile includes the name, address, phone number, email, and
other attributes of the consumer.

**When to use:** You may choose to retrieve the consumer’s profile
information for displaying directly to the end user and enabling the
consumer to make updates to their profile. Displaying the consumer’s
current email address is a common interaction when enabling that
consumer to sign up for e‑bills, payment reminders, or other email
alerts that will be sent to that email address.

Examples:

-   Retrieving and displaying the consumer’s profile.

-   Displaying the consumer’s email address when enabling other features
    that will generate email alerts to that email address.

-   "Synching" the consumer’s profile by retrieving the current profile
    information and comparing that to the current profile held by the
    FI's online banking application, so that any updates may be
    submitted back to Fiserv.

### Method and Endpoint

| GET | /api/v2/me |
|------|-----------|

### Response

| Parameter | Req  | Data Type                 | Description                                                                                                                                  |
|------------|-----|----------|-----------------------------------------------|
| data      | Cond | [UserV2](#userv2)         | Consumer information. Condition: Only returned when the request is successful.                                                               |
| result    | Cond | [ResultType](#resulttype) | Result associated with the request. Condition: Only returned when the request fails. No content returned for success (HTTP status code 200). |

### Sample API Usage

#### Response

```json

{
"data": {
"userId": "SampleUserId",
"hasPayees": true,
"name": {
"firstName": "John",
"lastName": "Smith",
"middleName": "Q",
"nickname": "John",
"prefix": "MR."
},
"contactEndPoints": [
{
"status": "Active",
"endPointType": "Address",
"address": {
"addressType": "USAddress",
"usAddress": {
"address1": "5641 Main St",
"city": "ATHENS",
"state": "GA",
"zipCode": "30306"
}
},
"defaultEndpoint": true
},
{
"status": "Active",
"endPointType": "Email",
"email": {
"value": "john.smith@fiserv.com"
},
"defaultEndpoint": true
},
{
"status": "Active",
"endPointType": "Phone",
"phone": {
"phoneType": "Day",
"value": "6716631453"
},
"defaultEndpoint": false
},
{
"status": "Active",
"endPointType": "Phone",
"phone": {
"phoneType": "Evening",
"value": "8674755167"
},
"defaultEndpoint": true
}
],
"sensitiveInformationUri": "/api/v2/me/sensitiveInformation",
"timeZone": -5,
"country": "USA",
"status": "Active",
"languageCode": "EN",
"languageCountry": "US",
"locale": "EN-US",
"newUser": false
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

## Record User’s Consent Information

The Consents Post API records a consumer’s consent or non-consent to use
a bill service provider and/or credit reporting agency to search for
potential payees for the consumer. This API should be used in
conjunction with the Potential Payee API set to implement a bill
discovery use case within your custom UI. The UI must pass the user’s
consent to bill pay services to be able to find bills using the
available sources.

Note

When the consentType is “CreditBureau”, the language presented to the
user for consent should comply with FCRA s604.

### Method and Endpoint

| POST | /api/v1/me/Consents |
|------|---------------------|

### Request

| Parameter   | Req | Param Type | Data Type                    | Description                                                                                                                      |
|-----------|-----|--------|-------|-------------------------------------------|
| ConsentList | Req | body       | Array of [Consent](#consent) | List of consent items. For example, a consumer may agree to find bills from a bill service provider or a credit bureau, or both. |

### Response

| Parameter | Req  | Data Type                 | Description                                                                                                                  |
|------------|-----|----------|-----------------------------------------------|
| result    | Cond | [ResultType](#resulttype) | Result information. Condition: Only returned when the request fails. No content returned for success (HTTP status code 204). |

### Sample API Usage

#### Request Body

```json

{
"consents": [
{
"consentText": "Can we search your bill information?",
"consentType": "BillDiscovery",
"consentGiven": true
},
{
"consentText": "Can we search your credit records?",
"consentType": "CreditBureau",
"consentGiven": false
}
]
}

```

#### Response

| 204 No Content |
|----------------|

## Return User’s Consent Information

The Consents Get API returns a consumer’s consent or non-consent to use
a bill service provider and/or credit reporting agency to search for
potential payees for the consumer.

### Method and Endpoint

| GET  | /api/v1/me/Consents |
|------|---------------------|

### Response

<table>
<colgroup>
<col style="width: 15%" />
<col style="width: 5%" />
<col style="width: 12%" />
<col style="width: 66%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>billDiscoveryUserConsent</td>
<td>Req</td>
<td>boolean</td>
<td><p>Indicates if the user has given consent to the retrieval of bill
information via Bill Discovery.</p>
<p>Will be set to true when the:</p>
<ul>
<li><p>Consumer has explicitly given consent for Bill
Discovery.</p></li>
</ul>
<p>Will be set to false when the:</p>
<ul>
<li><p>Consumer has explicitly refused consent for Bill
Discovery.</p></li>
<li><p>Consumer has no consent record to be returned.</p></li>
</ul></td>
</tr>
<tr class="even">
<td>creditBureauUserConsent</td>
<td>Req</td>
<td>boolean</td>
<td><p>Indicates if the user has given consent to the retrieval of bill
information via credit bureau.</p>
<p>Will be set to true when the:</p>
<ul>
<li><p>Consumer has explicitly given consent to access credit bureau
information for Bill Discovery.</p></li>
</ul>
<p>Will be set to false when the:</p>
<ul>
<li><p>Consumer has explicitly refused consent to access credit bureau
information for Bill Discovery.</p></li>
<li><p>Consumer has no consent record to be returned</p></li>
</ul></td>
</tr>
<tr class="odd">
<td>result</td>
<td>Req</td>
<td><a href="#resulttype">ResultType</a></td>
<td>Result information.</td>
</tr>
</tbody>
</table>

### Sample API Usage

#### Response

```json
{
"data": {
"billDiscoveryUserConsent": true
"creditBureauUserConsent": true
},
"result": {
"success": true,
"resultInfo": [
]
}
}
```
## Get Features

The Features Get API retrieves feature capability information for a
consumer's tenant/sponsor.

### Method and Endpoint

| GET | /api/v1/me/features |
|-----|---------------------|

### Response

| Parameter | Req  | Data Type                 | Description                                                                                                                                  |
|------------|-----|----------|-----------------------------------------------|
| data      | Req  | [Features](#features)     | Information about features.                                                                                                                  |
| result    | Cond | [ResultType](#resulttype) | Result associated with the request. Condition: Only returned when the request fails. No content returned for success (HTTP status code 200). |

### Sample API Usage

#### Response
```json
{
"data": {
"supportsBillDiscovery": true,
"supportsCardFundedBillPay": true,
"billDiscoveryProviderType": [
"BillServiceProvider",
"CreditBureau"
],
"transactionHistoryMonths": 24,
"lddEnabled": true,
"cardFundingOptions": {
"sponsorFirstPartyDebit": true,
"sponsorFirstPartyCredit": false,
"subscriberFirstPartyDebit": true,
"subscriberFirstPartyCredit": false,
"subscriberThirdPartyDebit": false
},
"supportsClassicView": true
}
}
```
## Get Sensitive Information

This API enables retrieving a consumer’s sensitive information.

The parameter **sensitiveInformationUri** (returned for [Get User
Information (Managed User)](#get-user-information-managed-user) and [Get
User Information](#get-user-information)) returns the endpoint shown
below.

### Method and Endpoint

| GET | /api/v2/me/sensitiveInformation |
|-----|---------------------------------|

### Response

| Parameter | Req  | Data Type                                         | Description                                                                                                                                  |
|------------|------|-----------|--------------------------------------------|
| data      | Req  | [SensitiveInformationV2](#sensitiveinformationv2) | Consumer's sensitive information.                                                                                                            |
| result    | Cond | [ResultType](#resulttype)                         | Result associated with the request. Condition: Only returned when the request fails. No content returned for success (HTTP status code 200). |

## Post eBill Notification Email Delivery Failure

The eBill Notification Email Delivery Failure Post API enables the FI to
inform Fiserv of eBill notification email delivery failures. eBill
notifications include eBills via email (EVEs) and stale eBill reminders.
If an FI cannot deliver these notifications within 24 hours to a
consumer, the FI needs to inform Fiserv that the FI was unable to
deliver an eBill notification. Fiserv uses this data to inform the
biller so that the biller can take appropriate action to avoid potential
future payment failures. For example, if necessary, the biller may
suspend eBill service and resume delivery of paper bills.

The response from Fiserv to the FI indicates if the FI’s request was
processed successfully. Note that success means that Fiserv received the
report of the eBill notification email delivery failure and the request
was successfully validated.

### Method and Endpoint

| POST | /api/v1/me/Users/{userid}/billDeliveryFailure/{ebillId} |
|------|----------------------------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description                                                                                       |
|-----------|-----|--------|-------|-------------------------------------------|
| userId    | Req | path       | string    | Identifier for the user. By default, this is the subscriber ID.                                   |
| ebillId   | Req | path       | string    | The unique identifier for the eBill. This must be the exact eBill ID from the eBill notification. |

### Response

| Parameter | Req | Data Type                 | Description                         |
|------------|-----|----------|-----------------------------------------------|
| result    | Req | [ResultType](#resulttype) | Result associated with the request. |

### Sample API Usage

#### Request URL

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/users/*userId*/billDeliveryFailure/20210614024559232737 |
|------------------------------------------------------------------------|

#### Response
```json
{
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
# 

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

-   [Determine a consumer's eligibility for bill
    discovery](#get-consumer-eligibility-for-bill-discovery)

-   [Get a list of potential payees](#get-potential-payees-list)

-   [Get a specific potential payee](#get-potential-payee-single)

-   [Verify a potential payee](#verify-potential-payee)

-   [Dismiss a potential payee](#dismiss-potential-payee)

-   [Get an unmasked potential payee account
    number](#get-a-potential-payees-unmasked-account-number)

## Get Consumer Eligibility for Bill Discovery

This API indicates whether a consumer is eligible for bill discovery.

### Method and Endpoint

| GET | /api/v1/me/PotentialPayees/eligibility |
|------|------------------|
### Response

| Parameter | Req | Data Type                                                     | Description                       |
|---------|----|------------|------------------------------------------------|
| data      | Req | [BillDiscoveryUserEligibility](#billdiscoveryusereligibility) | Consumer eligibility information. |
| result    | Req | [ResultType](#resulttype)                                     | Result information.               |

The values returned in the response are dependent on sponsor
configuration as follows:

-   If a sponsor is not enabled for Bill Discovery or one of the Bill
    Discovery data sources, the API returns creditReportEligible and
    billServiceProviderEligible as false. Zero is returned for the
    potential payee count.

-   When BillDiscovery is configured as true for the sponsor:

<!-- -->

-   If the sponsor is configured with eligibility rules, those rules
    will be taken into account (based on the number of payments made
    over a period of time). If the consumer is not eligible, the
    outstandingPotentialPayeesCount is still returned, allowing a
    consumer to act on any previously found payees. The
    creditReportEligible and billServiceProviderEligible flags are
    returned based on consumer eligibility and sponsor configuration.

-   If the sponsor is not configured with eligibility rules, then the
    creditReportEligible and billServiceProviderEligible flags are
    returned based on the sponsor configuration for these data sources.
    The outstandingPotentialPayeesCount reflects the number of potential
    payees previously found and not acted on for the consumer. In this
    situation, this call is not necessary.

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

<table>
<colgroup>
<col style="width: 16%" />
<col style="width: 3%" />
<col style="width: 8%" />
<col style="width: 7%" />
<col style="width: 63%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Param Type</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>excludeDormantAccounts</td>
<td>Opt</td>
<td>query</td>
<td>boolean</td>
<td><p>Indicates whether to exclude dormant payee accounts from the
results.</p>
<p>True – Exclude dormant payee accounts from the results.</p>
<p>False – Include dormant payee accounts. This is the default.</p>
<p>If the search parameter value is false, the excludeDormantAccounts
value must be false.</p></td>
</tr>
<tr class="even">
<td>search</td>
<td>Opt</td>
<td>query</td>
<td>boolean</td>
<td><p>Indicates whether to perform a new search for potential
payees.</p>
<p>True – Perform a new search. This is the default.</p>
<p>False – Do not perform a new search (return only potential payees
already identified).</p>
<p>If the search parameter value is false, the excludeDormantAccounts
value must be false.</p></td>
</tr>
</tbody>
</table>

### Response

| Parameter | Req | Data Type                                 | Description               |
|---------|----|-----------|-------------------------------------------------|
| data      | Req | [PotentialPayeeList](#potentialpayeelist) | List of potential payees. |
| result    | Req | [ResultType](#resulttype)                 | Result information.       |

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
| data      | Req | [PotentialPayee](#potentialpayee) | Information about the potential payee. |
| result    | Req | [ResultType](#resulttype)         | Result information.                    |

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

| Parameter          | Req | Param Type | Data Type                                        | Description                                                                                           |
|-----------|-----|-------|---------|------------------------------------------|
| id                 | Req | path       | string                                           | Identifier for the potential payee.                                                                   |
| verificationTokens | Req | body       | Array of [VerificationToken](#verificationtoken) | Array of verification token information provided by the consumer to verify the merchant relationship. |

### Response

| Parameter | Req | Data Type                                     | Description                            |
|---------|----|-----------|-------------------------------------------------|
| data      | Req | [VerificationResponse](#verificationresponse) | Information about the potential payee. |
| result    | Req | [ResultType](#resulttype)                     | Result information.                    |

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
| result    | Cond | [ResultType](#resulttype) | Result information. Condition: Only returned when the request fails. No content returned for success (HTTP status code 204). |

### Sample API Usage

#### Request URL

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/potentialPayees/e421657097d54c6b8fb8d585dd70aed4/dismiss |
|------------------------------------------------------------------------|

#### Response

| 204 No Content |
|----------------|

## Get a Potential Payee’s Unmasked Account Number

This API enables retrieving a potential payee’s unmasked account number.

The parameter **unmaskedAccountNumberUri** (returned for [Get Potential
Payees](#get-potential-payees-list), [Get Potential
Payee](#get-potential-payee-single), and [Verify Potential
Payee](#verify-potential-payee)) returns the endpoint shown below.

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
| result    | Cond | [ResultType](#resulttype) | Result associated with the request. Condition: Only returned when the request fails. No content returned for success (HTTP status code 200). |

# 

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

-   [Get an unmasked payee account
    number](#get-a-payees-unmasked-account-number)

-   [Get a list of automatic transaction options for a
    payee](#get-automatic-transaction-options)

## Get Payees (List)

The Payees Get API returns the list of payees added by the consumer.

### Method and Endpoint

| GET | /api/v1/me/payees |
|-----|-------------------|

### Request

<table>
<colgroup>
<col style="width: 16%" />
<col style="width: 3%" />
<col style="width: 8%" />
<col style="width: 7%" />
<col style="width: 63%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Param Type</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>returnInactivePayees</td>
<td>Opt</td>
<td>query</td>
<td>boolean</td>
<td><p>Indicates whether to return inactive payees in the response.</p>
<p>True – Return inactive payees</p>
<p>False – Do not return inactive payees. This is the default.</p></td>
</tr>
</tbody>
</table>

### Response

| Parameter | Req | Data Type                 | Description         |
|-----------|-----|---------------------------|---------------------|
| data      | Req | [PayeeList](#payeelist)   | List of payees.     |
| result    | Req | [ResultType](#resulttype) | Result information. |

### Sample API Usage

#### Response – Freeform Payee
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
#### Response – Overnight Payee
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
#### Response – E-bill Capable Merchant

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
#### Response – E-bill Activated Merchant

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
#### Response – AutoPay Activated Merchant

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
#### Request URL – Inactive Payees

A returnInactivePayees query parameter can be provided to also see
inactive payees in the list.

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/payees?returnInactivePayees=True |
|------------------------------------------------------------------------|

#### Response – Inactive Payees
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
| data      | Req | [Payee](#payee)           | Information about the payee. |
| result    | Req | [ResultType](#resulttype) | Result information.          |

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

The Payee Post API enables adding a new payee to a consumer’s payee
list, so that payment transactions can be made to that payee.

### Method and Endpoint

| POST | /api/v1/me/payees |
|------|-------------------|

### Request

<table>
<colgroup>
<col style="width: 16%" />
<col style="width: 4%" />
<col style="width: 8%" />
<col style="width: 7%" />
<col style="width: 62%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Param Type</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>overrideAddressValidation</td>
<td>Opt</td>
<td>query</td>
<td>boolean</td>
<td><p>Indicates whether address validation (city, state, ZIP Code,
address combination) must be done or can be skipped.</p>
<p>True - Address validation will be skipped.</p>
<p>False - Address validation will be done. This is the
default.</p></td>
</tr>
<tr class="even">
<td>payeeInfo</td>
<td>Req</td>
<td>body</td>
<td><a href="#payeeinfo">PayeeInfo</a></td>
<td>Payee information.</td>
</tr>
<tr class="odd">
<td>sourceUri</td>
<td>Cond</td>
<td>body</td>
<td>string</td>
<td>The URI from Potential Payees/ Merchant Search/ Payees /
ProbableMerchants.<br />
Blank for an unmanaged merchant.<br />
Condition: If adding a payee by providing only name, account number, and
sourceUri, this is required. (Fiserv must already have a relationship
with the merchant.)</td>
</tr>
<tr class="even">
<td>sourceUriPayeeZipCode</td>
<td>Cond</td>
<td>body</td>
<td>string</td>
<td><p>The payee ZIP Code. This is the ZIP Code that the consumer sees
on their bill for the merchant.<br />
Conditions: If the /api/v1/merchants/Search API returns a value of true
for merchantZipRequired, this is required. If the
/api/v1/me/probableMerchants API returns a value of true for
merchantZipRequired, this is required.</p>
<p>Pattern: ^(\d{5}|\d{9}|\d{11})$</p>
<p>Valid characters: 0–9</p>
<p>Parsed: Chars 1-5 = Zip5, Chars 6-9 = Zip4, Chars 10-11 =
Zip2</p></td>
</tr>
</tbody>
</table>

### Response

| Parameter | Req  | Data Type                 | Description                                                        |
|------------|-----|--------|------------------------------------------------|
| data      | Cond | [BaseModel](#basemodel)   | Response data. Condition: Always returned for successful response. |
| result    | Req  | [ResultType](#resulttype) | Result information.                                                |

### Sample API Usage – Freeform Payee

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
### Sample API Usage – Overnight Payee

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
### Sample API Usage – Merchant Payee

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

The Payee Patch API enables modifying an existing payee on a consumer’s
payee list. This is typically done to update information (such as
address) for a payee if that information has changed. Future payments to
that payee are processed using the modified information. It is
recommended to send only those attributes in the request body that
require updates.

### Method and Endpoint

| PATCH | /api/v1/me/payees/{payeeId} |
|-------|----------------------------|

### Request

<table>
<colgroup>
<col style="width: 16%" />
<col style="width: 3%" />
<col style="width: 8%" />
<col style="width: 13%" />
<col style="width: 58%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Param Type</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>payeeId</td>
<td>Req</td>
<td>path</td>
<td>string</td>
<td>Identifier for the payee.</td>
</tr>
<tr class="even">
<td>modifyPendingPayments</td>
<td>Opt</td>
<td>query</td>
<td>boolean</td>
<td><p>Indicates if the payee modification should be propagated to
pending payments. Valid values:</p>
<p>true – Yes, propagate payee modification to pending payments.</p>
<p>false – No, do not propagate payee modification to pending payments.
This is the default.</p></td>
</tr>
<tr class="odd">
<td>name</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td><p>The name of the payee.</p>
<p>Pattern: ^[\x20-\x5A\x5C\x5F-\x7E]+$</p>
<p>Length: 2–32</p></td>
</tr>
<tr class="even">
<td>nickname</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td><p>The nickname of the payee. Length: 0-30</p>
<p>Pattern: ^[\x20\x2C-\x2E\x30-\x39\x41-\x5A\x61-\x7A\r\n]+$</p></td>
</tr>
<tr class="odd">
<td>accountNumber</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td><p>The consumer’s account number with the payee. Length: 1–32</p>
<p>Pattern: ^[a-zA-Z0-9 !"#$%&amp;-]{1,32}$</p></td>
</tr>
<tr class="even">
<td>contactPhoneNumber</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td><p>Phone number used to contact the payee if there are issues
posting the payment. Length: 10-12</p>

`Pattern:(^[0-9]{10}$)|(^\(?[0-9]{3}\)?-[0-9]{3}-[0-9]{4}$)|(^\(?[0-9]{3}\)?\s?[0-9]{3}-[0-9]{4}$)`

<p>Must be numeric and may contain a dash or space between the numbers
at the appropriate placement. The phone number must be valid based on
the North American Numbering Plan (for example, the area code cannot
begin with a 0 or 1). Example: 234-555-1212</p></td>
</tr>
<tr class="odd">
<td>address</td>
<td>Opt</td>
<td>body</td>
<td><a href="#usaddress">USAddress</a></td>
<td>Payee address information.</td>
</tr>
<tr class="even">
<td>overnightAddress</td>
<td>Opt</td>
<td>body</td>
<td><a href="#usaddress">USAddress</a></td>
<td>Address for overnight payments if different from the address.</td>
</tr>
<tr class="odd">
<td>socialTokens</td>
<td>Opt</td>
<td>body</td>
<td>Array of<br />
SocialToken</td>
<td>Reserved for future use.</td>
</tr>
<tr class="even">
<td>accountTokens</td>
<td>Opt</td>
<td>body</td>
<td>Array of <a href="#accounttokenaddinfo">AccountTokenAddInfo</a></td>
<td>Reserved for future use.</td>
</tr>
</tbody>
</table>

### Response

| Parameter | Req  | Data Type                 | Description                                                                                                                                  |
|-------------|-----|-------|-------------------------------------------------|
| result    | Cond | [ResultType](#resulttype) | Result associated with the request. Condition: Only returned when the request fails. No content returned for success (HTTP status code 204). |

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

#### Request URL – Propagate to Pending Payments

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

#### Request Body – Propagate to Pending Payments
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
#### Response – Propagate to Pending Payments

| 204 No Content |
|----------------|

## Delete a Payee

This API enables deleting a payee from a consumer’s existing list of
payees. Deleting a payee will cancel any e-bill service and any
automatic payment plans associated with the payee.

### Method and Endpoint

| DELETE | /api/v1/me/payees/{payeeId}|
|--------|----------------------------|

### Request

<table>
<colgroup>
<col style="width: 16%" />
<col style="width: 3%" />
<col style="width: 8%" />
<col style="width: 7%" />
<col style="width: 63%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Param Type</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>payeeId</td>
<td>Req</td>
<td>path</td>
<td>string</td>
<td>Identifier for the payee.</td>
</tr>
<tr class="even">
<td>cancelPendingTransactions</td>
<td>Opt</td>
<td>query</td>
<td>boolean</td>
<td><p>Indicates if pending transactions to the payee should be
canceled. Valid values:</p>
<p>true – Yes, cancel pending transactions.</p>
<p>false – No, do not cancel pending transactions. This is the
default.</p></td>
</tr>
</tbody>
</table>

### Response

| Parameter | Req  | Data Type                 | Description                                                                                                                                  |
|------------|------|-------|-----------------------------------------------|
| result    | Cond | [ResultType](#resulttype) | Result associated with the request. Condition: Only returned when the request fails. No content returned for success (HTTP status code 204). |

### Sample API Usage

#### Request URL – Keep Pending Payments

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/payees/c31c83340195458488da45c5e4b83085 |
|------------------------------------------------------------------------|

#### Response – Keep Pending Payments

| 204 No Content |
|----------------|

#### Request URL – Cancel Pending Payments

https://api-checkfreenext-cert.fiservapps.com/api/v1/me/payees/10715add606a48b7a1dfd77fae70e0e4?cancelPendingTransactions=true

#### Response – Cancel Pending Payments

| 204 No Content |
|----------------|

## Get a Payee’s Unmasked Account Number

This API enables retrieving a payee’s unmasked account number.

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
| result    | Cond | [ResultType](#resulttype) | Result associated with the request. Condition: Only returned when the request fails. No content returned for success (HTTP status code 200). |

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
| data      | Req  | [AutomaticTransactionOptions](#automatictransactionoptions) | Automatic transaction options for the payee.                                                                                                 |
| result    | Cond | [ResultType](#resulttype)                                   | Result associated with the request. Condition: Only returned when the request fails. No content returned for success (HTTP status code 200). |

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
# Payee Groups

This set of APIs enables a consumer to:

-   [Get a list of payee groups](#get-payee-groups)

-   [Add payee group](#add-a-payee-group)

-   [Update payee group](#update-a-payee-group)

-   [Delete payee group](#delete-a-payee-group)

## Get Payee Groups

The PayeeGroups Get API enables a consumer to return a list of payee
group names set up by the user. Payees assigned to the group(s) will
also be returned.

### Method and Endpoint

| GET | /api/v1/me/PayeeGroups |
|-----|------------------------|

### Response

| Parameter | Req | Data Type                                     | Description                                                    |
|---------|----|-----------|-------------------------------------------------|
| data      | Req | [PayeeGroupListOutput](#payeegrouplistoutput) | List of payee groups. If there are no groups, 204 is returned. |
| result    | Req | [ResultType](#resulttype)                     | Result information.                                            |

### Sample API Usage

#### Response
```json
{
"data": {
"payeeGroups": [
{
"isVisible": true,
"name": "TestGroup1",
"payeeGroupInfo": [
"payeeUri": "/api/v1/me/payees/3f998588c1784b068e41676de11a36ca",
"payeeUri": "/api/v1/me/payees/f0c603de83af40518fac4b9dfae433f2"
],
"self":
"/api/v1/me/payeeGroups/df1873d54d6a4f14b43ac84b36fb3556",
"id": "df1873d54d6a4f14b43ac84b36fb3556"
}
]
},
"result": {
"success": true,
"resultInfo": []
}
}
```
## Add a Payee Group

The Payee Groups Post API enables a consumer to add a new payee group.
The consumer can assign payees to the group and can specify whether they
want to display the group or not.

### Method and Endpoint

| POST | /api/v1/me/PayeeGroups |
|------|------------------|

### Request

<table>
<colgroup>
<col style="width: 15%" />
<col style="width: 4%" />
<col style="width: 8%" />
<col style="width: 14%" />
<col style="width: 57%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Param Type</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>isVisible</td>
<td>Req</td>
<td>body</td>
<td>boolean</td>
<td><p>Indicates whether whether the payee group will be displayed.</p>
<p>True - Payee group will be displayed. This is the default.</p>
<p>False - Payee group will not be displayed.</p></td>
</tr>
<tr class="even">
<td>name</td>
<td>Req</td>
<td>body</td>
<td>string</td>
<td><p>Unique name for the payee group to be added. Length: 1-32</p>
<p>Pattern: ^[\x20-\x5A\x5C\x5F-\x7E]+$</p></td>
</tr>
<tr class="odd">
<td>payeeGroupInfo</td>
<td>Opt</td>
<td>body</td>
<td>Array of <a href="#payeegrouppayeeitem">PayeeGroupPayeeItem</a></td>
<td>A list of payees to assign to the group. Any payees found to be
invalid will not be added to the group. A warning will be returned in
this case.</td>
</tr>
</tbody>
</table>

### Response

| Parameter | Req  | Data Type                             | Description                                                      |
|------------|-----|----------|-----------------------------------------------|
| data      | Cond | [PayeeGroupOutput](#payeegroupoutput) | Response data. Condition: Required when payeeGroupInfo provided. |
| result    | Req  | [ResultType](#resulttype)             | Result information.                                              |

### Sample API Usage

#### Request Body
```json
{
"isVisible": true,
"name": "NewGroup",
"payeeGroupInfo": [
{
"payeeUri": "/api/v1/me/payees/3d30666ac2104251913349825d45d1f8",
"listItemId": 1
}
]
}
```
#### Response
```json
{
"data": {
"payeeResults": [
{
"listItemId": 1,
"result": {
"success": true,
"resultInfo": []
}
}
],
"self":
"/api/v1/me/payeeGroups/67cd0da939924accbd233220f24fd9f0",
"id": "67cd0da939924accbd233220f24fd9f0"
},
"result": {
"success": true,
"resultInfo": []
}
}
```
## Update a Payee Group

The Payee Groups Put API enables a consumer to modify a payee group
name, specify whether the payee group is displayed, assign payees to a
payee group, or remove payees from a payee group.

Note

To move a payee from one group to another, the consumer must first
remove the payee from its current group and then make another call to
add the payee to the appropriate group.

### Method and Endpoint

| PUT | /api/v1/me/PayeeGroups/{id} |
|-----|------------------|

### Request

<table>
<colgroup>
<col style="width: 16%" />
<col style="width: 3%" />
<col style="width: 8%" />
<col style="width: 14%" />
<col style="width: 57%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Param Type</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>id</td>
<td>Req</td>
<td>path</td>
<td>string</td>
<td>Identifier for the payee group.</td>
</tr>
<tr class="even">
<td>isVisible</td>
<td>Req</td>
<td>body</td>
<td>boolean</td>
<td><p>Indicates whether whether the payee group will be displayed.</p>
<p>True - Payee group will be displayed. This is the default.</p>
<p>False - Payee group will not be displayed.</p></td>
</tr>
<tr class="odd">
<td>name</td>
<td>Req</td>
<td>body</td>
<td>string</td>
<td><p>Unique name for the payee group. Length: 1-32</p>
<p>Pattern: ^[\x20-\x5A\x5C\x5F-\x7E]+$</p></td>
</tr>
<tr class="even">
<td>payeeGroupInfo</td>
<td>Opt</td>
<td>body</td>
<td>Array of <a href="#payeegrouppayeeitem">PayeeGroupPayeeItem</a></td>
<td><p>A list of payees to assign to the group. Any payees to be added
to or retained in the group should be provided in this array.</p>
<p>Note that the following actions will remove <strong>all</strong>
payees from a group:</p>
<ul>
<li><p>Submitting an empty array</p></li>
<li><p>Submitting an array specifying “null”</p></li>
<li><p>Not submitting an array.</p></li>
</ul>
<p>Any payees found to be invalid will not be added to the group. A
warning will be returned in this case.</p></td>
</tr>
</tbody>
</table>

### Response

| Parameter | Req  | Data Type                                   | Description                                                                                                                                  |
|------------|-----|-----------|----------------------------------------------|
| data      | Cond | [PutPayeeGroupOutput](#putpayeegroupoutput) | Response data. Condition: Required when payeeGroupInfo provided.                                                                             |
| result    | Cond | [ResultType](#resulttype)                   | Result associated with the request. Condition: Only returned when the request fails. No content returned for success (HTTP status code 204). |

### Sample API Usage

#### Request URL

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/payeeGroups/3fc7a190e69d4fe3ac8e2b56dc75b62e |
|------------------------------------------------------------------------|

#### Request Body
```json
{
"isVisible": false,
"name": "Utilities",
"payeeGroupInfo": [
{
"payeeUri": "/api/v1/me/payees/57235cfe940a481d8ceb5a9157fdbbe2",
"listItemId": 1
},
{
"payeeUri": "/api/v1/me/payees/3e0133c6d6414dc083f00526a6de2a36",
"listItemId": 2
}
],
"self":
"/api/v1/me/payeeGroups/3fc7a190e69d4fe3ac8e2b56dc75b62e",
"id": "3fc7a190e69d4fe3ac8e2b56dc75b62e"
}
```
#### Response
```json
{
"data": {
"payeeResults": [
{
"listItemId": 1,
"result": {
"success": true,
"resultInfo": []
}
},
{
"listItemId": 2,
"result": {
"success": true,
"resultInfo": []
}
}
]
},
"result": {
"success": true,
"resultInfo": []
}
}
```
## Delete a Payee Group

This API enables a consumer to delete an existing payee group name. Any
payees previously assigned to that group will become unassigned (no
longer assigned to any particular group).

### Method and Endpoint

| DELETE | /api/v1/me/PayeeGroups/{id}|
|--------|----------------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description                                   |
|------------|----|-------|-------|-------------------------------------------|
| id        | Req | path       | string    | Identifier for the payee group to be deleted. |

### Response

| Parameter | Req  | Data Type                 | Description                                                                                                                                  |
|------------|------|-------|-----------------------------------------------|
| result    | Cond | [ResultType](#resulttype) | Result associated with the request. Condition: Only returned when the request fails. No content returned for success (HTTP status code 204). |

### Sample API Usage

#### Request 

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/payeeGroups/d882685743114c5cad7f3cc9e7087506 |
|------------------------------------------------------------------------|

#### Response

| 204 No Content |
|----------------|

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

<table>
<colgroup>
<col style="width: 16%" />
<col style="width: 6%" />
<col style="width: 8%" />
<col style="width: 7%" />
<col style="width: 60%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Param Type</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>name</td>
<td>Req</td>
<td>query</td>
<td>string</td>
<td><p>Length: 3–32</p>
<p>The name of the merchant name to be found. The search characters
entered shall be those contained within the merchant name (not a “starts
with” search).</p></td>
</tr>
<tr class="even">
<td>returnLocalMerchants</td>
<td>Opt</td>
<td>query</td>
<td>boolean</td>
<td><p>Indicates whether to return merchants that are relevant to the
consumer’s ZIP Code.</p>
<p>True – If there is no match, return local merchants in the response.
If there is a match or partial match, return those matches only. Default
is true.</p>
<p>False – If no matches are found, do not return any local
merchants.</p></td>
</tr>
</tbody>
</table>

### Response

<table>
<colgroup>
<col style="width: 14%" />
<col style="width: 4%" />
<col style="width: 12%" />
<col style="width: 67%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>data</td>
<td>Req</td>
<td>Array of <a href="#merchant">Merchant</a></td>
<td><p>List of merchants found based on the provided name search
criteria and the returnLocalMerchants flag. Merchants matched are
returned in the following order:<br />
1. Exact matches<br />
2. Merchants containing the search criteria</p>
<p>If no matches are found based upon the entered search criteria,
merchants relevant to the consumer’s ZIP Code will be returned if the
returnLocalMerchants flag is true.</p></td>
</tr>
<tr class="even">
<td>result</td>
<td>Req</td>
<td><a href="#resulttype">ResultType</a></td>
<td>Result information.</td>
</tr>
</tbody>
</table>

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

# Probable Merchants

This API allows the calling application to:

-   [Get probable merchants](#get-probable-merchants)

## Get Probable Merchants

The ProbableMerchants Get API presents "probable" or "likely" merchants
that a consumer would make payments to. This API returns a list of
merchants that are typically paid by other bill payment users in that
consumer’s ZIP Code area (the consumer’s ZIP Code is identified using
the subscriber ID). Local utility companies, financial institutions,
service companies, etc. may all be returned.

**When to use:** A client's UI application that enables consumers to
manage their payees can use the ProbableMerchants Get API in conjunction
with the Merchants Search Get API. By using these in combination, a UI
application can present the consumer with the options of selecting from
a list of probable merchants or entering a payee name.

Examples:

-   Add a new payee to a consumer’s existing list of payees.

-   Present a special flow to new users that helps them to get started
    using bill pay functionality.

### Method and Endpoint

| GET | /api/v1/me/probableMerchants |
|-----|----------------------------|

### Response

| Parameter | Req | Data Type                               | Description                 |
|------------|-----|----------|-----------------------------------------------|
| data      | Req | [ProbableMerchants](#probablemerchants) | List of probable merchants. |
| result    | Req | [ResultType](#resulttype)               | Result information.         |

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
# Reminders

This set of APIs allows the calling application to:

-   [Get a list of bill payment reminder
    models](#get-reminder-models-list)

-   [Get a single reminder model](#get-reminder-model-single)

-   [Add a reminder model](#add-a-reminder-model)

-   [Update a reminder model](#update-a-reminder-model)

-   [Delete a reminder model](#delete-a-reminder-model)

## Get Reminder Models (List)

The Reminders Get API returns a list of a consumer’s bill payment
reminder models. Each model contains the parameters that are used to
generate individual reminder messages. For example, a **reminder model**
might indicate that a **reminder message** (ToDo) is to be generated for
a payment typically due on the 15th of the month, for $15.00, monthly,
and that the consumer wants to be reminded 10 days prior to the 15th.

**When to use**: Use this API if you want to present the ability to a
consumer to set up and manage bill payment reminders. This is typically
called as part of displaying information for a specific payee to present
any bill payment reminder instructions that have previously been set up
for that payee by that consumer.

### Method and Endpoint


| GET | /api/v1/me/Reminders |
|-----|----------------------|

### Response

| Parameter | Req  | Data Type                     | Description                                                                                                                                         |
|------------|-----|----------|-----------------------------------------------|
| data      | Req  | [ReminderList](#reminderlist) | Reminder models. There is an empty array if there is no data to return.                                                                             |
| result    | Cond | [ResultType](#resulttype)     | Result associated with the request. Condition: Only returned when the request fails. No result content returned for success (HTTP status code 200). |

### Sample API Usage

#### Response
```json
{
"data": {
"reminders": [
{
"alertAtLeadTimeBeforePaymentDueDate": true,
"alertIfNoBillPaymentMadeByDueDate": true,
"alertWhenBillPaymentProcesses": true,
"amount": 10.0,
"firstReminderDate": "2022-12-13",
"frequency": "Annually",
"leadTime": "Lead28Days",
"currentDueDate": "2022-12-13",
"payeeUri": "/api/v1/me/payees/dcd2c702d1b84c7ead5497516fbadc0b",
"self": "/api/v1/me/reminders/c458cad50a8a4cec98d8f5e816f4ba99",
"id": "c458cad50a8a4cec98d8f5e816f4ba99"
}
]
},
"result": {
"success": true,
"resultInfo": []
}
}
```
## Get Reminder Model (Single)

The Reminders Get API returns a specific bill payment reminder model for
a consumer.

### Method and Endpoint


| GET | /api/v1/me/Reminders/{id} |
|-----|---------------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description                        |
|-----------|-----|------------|-----------|------------------------------------|
| id        | Req | path       | string    | Identifier for the reminder model. |

### Response

| Parameter | Req | Data Type                 | Description                           |
|---------|----|-----------|-------------------------------------------------|
| data      | Req | [Reminder](#reminder)     | Information about the reminder model. |
| result    | Req | [ResultType](#resulttype) | Result information.                   |

### Sample API Usage

#### Request URL

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/Reminders/21343a099774452e87fbe866cf1eef01 |
|------------------------------------------------------------------------|

#### Response
```json
{
"data": {
"alertAtLeadTimeBeforePaymentDueDate": true,
"alertIfNoBillPaymentMadeByDueDate": true,
"alertWhenBillPaymentProcesses": true,
"amount": 10.0,
"firstReminderDate": "2022-10-30",
"frequency": "Weekly",
"leadTime": "Lead03Days",
"currentDueDate": "2022-10-30",
"payeeUri": "/api/v1/me/payees/186dd42d11f04ba1adef2806a10128b5",
"self": "/api/v1/me/reminders/21343a099774452e87fbe866cf1eef01",
"id": "21343a099774452e87fbe866cf1eef01"
},
"result": {
"success": true,
"resultInfo": []
}
}
```
## Add a Reminder Model

The Reminders Post API enables a consumer to set up a new bill payment
reminder model for one of their payees. Each model contains the
parameters that are used to generate individual reminder messages. For
example, a **reminder model** might indicate that a **reminder**
**message** (ToDo) is to be generated for a payment typically due on the
15th of the month, for $15.00, monthly, and that the consumer wants to
be reminded 10 days prior to the 15th.

**When to use:** Use this API if you want to present the ability to a
consumer to set up and manage bill payment reminder instructions. This
API is typically called as part of displaying information for a specific
payee and allows the consumer to set up a new bill payment reminder for
that payee.

Examples:

• Viewing a payee – Use this operation to enable a new bill payment
reminder for that payee.

• Viewing existing reminders – Use this operation to allow a consumer to
set up new bill payment reminders for other payees.

### Method and Endpoint


| POST | /api/v1/me/Reminders |
|------|----------------------|

### Request

<table>
<colgroup>
<col style="width: 23%" />
<col style="width: 4%" />
<col style="width: 8%" />
<col style="width: 7%" />
<col style="width: 56%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Param Type</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>alertAtLeadTimeBeforePaymentDueDate</td>
<td>Req</td>
<td>body</td>
<td>boolean</td>
<td><p>Indicates if the consumer is reminded at lead time (see leadTime)
before a payment is due.</p>
<p>Valid values:</p>
<p>true – Remind the consumer at lead time before a payment is due.</p>
<p>false – Do not remind the consumer at lead time before a payment is
due.</p></td>
</tr>
<tr class="even">
<td>alertIfNoBillPaymentMadeByDueDate</td>
<td>Req</td>
<td>body</td>
<td>boolean</td>
<td><p>Indicates if the consumer is reminded if no payment has been made
by the due date.</p>
<p>Valid values:</p>
<p>true – Remind the consumer that a payment is due.</p>
<p>false – Do not remind the consumer that a payment is due.</p></td>
</tr>
<tr class="odd">
<td>alertWhenBillPaymentProcesses</td>
<td>Req</td>
<td>body</td>
<td>boolean</td>
<td><p>Indicates if the consumer is notified when a payment is
processed.</p>
<p>Valid values:</p>
<p>true – Notify the consumer when a payment has been marked as
processed.</p>
<p>false – Do not notify the consumer when a payment has been marked as
processed.</p></td>
</tr>
<tr class="even">
<td>amount</td>
<td>Req</td>
<td>body</td>
<td>number</td>
<td><p>The payment amount associated with this reminder.</p>
<p>Pattern: ^\d+(\.\d{1,2})?$</p></td>
</tr>
<tr class="odd">
<td>firstReminderDate</td>
<td>Req</td>
<td>body</td>
<td>string</td>
<td>Date for the first bill payment reminder in yyyy-MM-dd format. The
first date of a reminder should be greater than today plus the number of
lead days requested.</td>
</tr>
<tr class="even">
<td>frequency</td>
<td>Req</td>
<td>body</td>
<td>string</td>
<td><p>The frequency of how often the reminder is automatically created.
Valid values:</p>
<p>Weekly (should not be used in combination with leadTime of 10, 14,
21, or 28 days)<br />
Every2Weeks (should not be used in combination with leadTime of 14, 21,
or 28 days)<br />
TwiceAMonth (should not be used in combination with leadTime of 21 or 28
days)<br />
Every4Weeks<br />
Monthly<br />
Every2Months<br />
Every3Months<br />
Every4Months<br />
Every6Months<br />
Annually</p></td>
</tr>
<tr class="odd">
<td>leadTime</td>
<td>Req</td>
<td>body</td>
<td>string</td>
<td><p>Number of days (excluding any risk management calculations)
before the due date that the reminder will be created. Valid values:</p>
<p>Lead03Days<br />
Lead05Days<br />
Lead10Days (should not be used in combination with frequency
Weekly)<br />
Lead14Days (should not be used in combination with frequency Weekly or
Every2Weeks)<br />
Lead21Days (should not be used in combination with frequency Weekly,
Every2Weeks, or TwiceAMonth)<br />
Lead28Days (should not be used in combination with frequency Weekly,
Every2Weeks, or TwiceAMonth)</p></td>
</tr>
<tr class="even">
<td>payeeUri</td>
<td>Req</td>
<td>body</td>
<td>string</td>
<td>URI for the payee. This matches the payee URI returned in GET
Payees.</td>
</tr>
</tbody>
</table>

### Response

| Parameter | Req  | Data Type                 | Description                                                        |
|------------|-----|--------|------------------------------------------------|
| data      | Cond | [BaseModel](#basemodel)   | Response data. Condition: Always returned for successful response. |
| result    | Req  | [ResultType](#resulttype) | Result information.                                                |

### Sample API Usage

#### Request Body
```json
{
"alertAtLeadTimeBeforePaymentDueDate": true,
"alertIfNoBillPaymentMadeByDueDate": true,
"alertWhenBillPaymentProcesses": true,
"amount": 10.0,
"firstReminderDate": "2022-11-27",
"frequency": "Weekly",
"leadTime": "Lead03Days",
"payeeUri": "/api/v1/me/payees/bc3e8eab2ccb4d43ad5214ea19463864"
}
```
#### Response
```json
{
"data": {
"self": "/api/v1/me/reminders/07b04270363d4c58beb3040ac3505bcd",
"id": "07b04270363d4c58beb3040ac3505bcd"
},
"result": {
"success": true,
"resultInfo": []
}
}
```
## Update a Reminder Model

The Reminders Patch API enables a consumer to modify an existing bill
payment reminder model for one of their payees. Each model contains the
parameters that are used to generate individual reminder messages. For
example, a **reminder model** might indicate that a **reminder message**
(ToDo) is to be generated for a payment typically due on the 15th of the
month, for $15.00, monthly, and that the consumer wants to be reminded
10 days prior to the 15th.

**When to use:** Use this API if you want to present the ability to a
consumer to set up and manage bill payment reminder instructions. This
operation is typically called as part of displaying information for a
specific payee and allows the consumer to modify an existing bill
payment reminder for that payee.

Examples:

• Viewing a payee – Use this operation to enable modifying an existing
bill payment reminder for that payee.

• Viewing existing reminders – Use this operation to allow a consumer to
modify an existing payment reminder

### Method and Endpoint


| PATCH | /api/v1/me/Reminders/{id}|
|--------|--------------------------|

### Request

<table>
<colgroup>
<col style="width: 23%" />
<col style="width: 3%" />
<col style="width: 7%" />
<col style="width: 11%" />
<col style="width: 52%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Param Type</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>id</td>
<td>Req</td>
<td>path</td>
<td>string</td>
<td>Identifier for the reminder model.</td>
</tr>
<tr class="even">
<td>alertAtLeadTimeBeforePaymentDueDate</td>
<td>Opt</td>
<td>body</td>
<td>boolean</td>
<td><p>Indicates if the user is reminded at lead time (see LeadTime)
before a payment is due.</p>
<p>Valid values:</p>
<p>true – Remind the user at lead time before a payment is due.</p>
<p>false – Do not remind the user at lead time before a payment is
due.</p></td>
</tr>
<tr class="odd">
<td>alertIfNoBillPaymentMadeByDueDate</td>
<td>Opt</td>
<td>body</td>
<td>boolean</td>
<td><p>Indicates if the user is reminded if no payment has been made by
the due date.</p>
<p>Valid values:</p>
<p>true – Remind the user that a payment is due.</p>
<p>false – Do not remind the user that a payment is due.</p></td>
</tr>
<tr class="even">
<td>alertWhenBillPaymentProcesses</td>
<td>Opt</td>
<td>body</td>
<td>boolean</td>
<td><p>Indicates if the user is notified when a payment is
processed.</p>
<p>Valid values:</p>
<p>true – Notify the user when a payment has been marked as
processed.</p>
<p>false – Do not notify the user when a payment has been marked as
processed.</p></td>
</tr>
<tr class="odd">
<td>amount</td>
<td>Opt</td>
<td>body</td>
<td>number</td>
<td><p>The payment amount associated with this reminder.</p>
<p>Pattern: ^\d+(\.\d{1,2})?$</p></td>
</tr>
<tr class="even">
<td>firstReminderDate</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td>First date for the bill payment reminder in yyyy-MM-dd format. The
first date of a reminder should be greater than today plus the number of
lead days requested.</td>
</tr>
<tr class="odd">
<td>frequency</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td><p>The frequency of how often the reminder is automatically created.
Valid values:</p>
<p>Weekly (should not be used in combination with leadTime of 10, 14,
21, or 28 days)<br />
Every2Weeks (should not be used in combination with leadTime of 14, 21,
or 28 days)<br />
TwiceAMonth (should not be used in combination with leadTime of 21 or 28
days)<br />
Every4Weeks<br />
Monthly<br />
Every2Months<br />
Every3Months<br />
Every4Months<br />
Every6Months<br />
Annually</p></td>
</tr>
<tr class="even">
<td>leadTime</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td><p>Number of days (excluding any risk management calculations)
before the due date that the reminder will be created. Valid values:</p>
<p>Lead03Days<br />
Lead05Days<br />
Lead10Days (should not be used in combination with frequency
Weekly)<br />
Lead14Days (should not be used in combination with frequency Weekly or
Every2Weeks)<br />
Lead21Days (should not be used in combination with frequency Weekly,
Every2Weeks, or TwiceAMonth)<br />
Lead28Days (should not be used in combination with frequency Weekly,
Every2Weeks, or TwiceAMonth)</p></td>
</tr>
</tbody>
</table>

### Response

| Parameter | Req  | Data Type                 | Description                                                                                                                                  |
|-------------|-----|-------|-------------------------------------------------|
| result    | Cond | [ResultType](#resulttype) | Result associated with the request. Condition: Only returned when the request fails. No content returned for success (HTTP status code 204). |

### Sample API Usage

#### Request URL

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/Reminders/e5b32963fe774f6bb436b9dc0529c4ee |
|------------------------------------------------------------------------|

#### Request Body
```json
{
"alertAtLeadTimeBeforePaymentDueDate": false,
"alertIfNoBillPaymentMadeByDueDate": false,
"alertWhenBillPaymentProcesses": false,
"amount": 12.21,
"firstReminderDate": "2023-01-17",
"frequency": "Monthly",
"leadTime": "Lead05Days"
}
```
#### Response

| 204 No Content |
|----------------|

## Delete a Reminder Model

The Reminders Delete API enables a consumer to cancel an existing bill
payment reminder model for one of their payees.

**When to use:** Use this API if you want to present the ability to a
consumer to manage bill payment reminder instructions. This operation is
typically called as part of displaying information for a specific payee
and allows the consumer to stop receiving bill payment reminders set up
for that payee.

Examples:

• Viewing a payee – Use this operation to enable canceling an existing
bill payment reminder for that payee.

• Viewing existing reminders – Use this operation to allow a consumer to
cancel an existing payment reminder.

### Method and Endpoint

| DELETE | /api/v1/me/Reminders/{id}|
|--------|--------------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description                                      |
|------------|----|-------|-------|-------------------------------------------|
| id        | Req | path       | string    | Identifier for the reminder model to be deleted. |

### Response

| Parameter | Req  | Data Type                 | Description                                                                                                                                  |
|------------|------|-------|-----------------------------------------------|
| result    | Cond | [ResultType](#resulttype) | Result associated with the request. Condition: Only returned when the request fails. No content returned for success (HTTP status code 204). |

### Sample API Usage

#### Request 

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/Reminders/356685037c1e421fae79aad446c2895f |
|------------------------------------------------------------------------|

#### Response

| 204 No Content |
|----------------|

# ToDos

A ToDo is any item (for example, an unpaid bill) that requires an action
from the consumer.

This set of APIs allows the calling application to:

-   [Get to-do items for a consumer](#get-todo-items-list)

-   [Get a single to-do item for a consumer](#get-todo-item-single)

-   [Dismiss a to-do item for a consumer](#dismiss-a-todo-item)

## Get ToDo Items (List)

The ToDos Get API enables the retrieval of ToDo items for a consumer
that they can take action on.

**When to use**: Use this API if you want to present a list of to-do
items and allow the consumer to take action on those items.

### Method and Endpoint

| GET | /api/v1/me/ToDos |
|-----|------------------|

### Response

| Parameter | Req  | Data Type                 | Description                                                                                                                                         |
|------------|-----|----------|-----------------------------------------------|
| data      | Req  | Array of [ToDo](#todo)    | ToDo items. There is an empty array if there is no data to return.                                                                                  |
| result    | Cond | [ResultType](#resulttype) | Result associated with the request. Condition: Only returned when the request fails. No result content returned for success (HTTP status code 200). |

### Sample API Usage

#### Response
```json
{
"toDoType": "UnpaidBill",
"toDoDetailUri":
"/api/v1/me/bills/5372f785-0bbf-47ec-8c08-7952670fcb84",
"payeeUri":
"/api/v1/me/payees/3f4d1e3f-b08e-4f45-9d2b-7c2281e8ccdb",
"actionDate": "2018-11-26",
"amount": 100,
"maskedAccountNumber": "*******1586",
"description": "AT&amp;T",
"self": "/api/v1/me/todos/5372f785-0bbf-47ec-8c08-7952670fcb84",
"id": "5372f785-0bbf-47ec-8c08-7952670fcb84"
}
```
## Get ToDo Item (Single)

The ToDos Get API returns a specific ToDo item for a consumer.

### Method and Endpoint

| GET | /api/v1/me/ToDos/{id} |
|-----|-----------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description                   |
|-----------|-----|------------|-----------|-------------------------------|
| id        | Req | path       | string    | Identifier for the ToDo item. |

### Response

| Parameter | Req | Data Type                 | Description                      |
|---------|----|-----------|-------------------------------------------------|
| data      | Req | [ToDo](#todo)             | Information about the ToDo item. |
| result    | Req | [ResultType](#resulttype) | Result information.              |

### Sample API Usage

#### Request URL

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/todos/ea4c60e2f3274a69a9078f5088e0f092 |
|------------------------------------------------------------------------|

#### Response
```json
{
"data": {
"toDoType": "UnpaidBill",
"toDoDetailUri":
"/api/v1/me/bills/ea4c60e2f3274a69a9078f5088e0f092",
"payeeUri": "/api/v1/me/payees/3c321d78a62e40cb908846d2e7d85528",
"actionDate": "2019-09-23",
"amount": 64.69,
"maskedAccountNumber": "*******1248",
"description": "American Eagle",
"self": "/api/v1/me/todos/ea4c60e2f3274a69a9078f5088e0f092",
"id": "ea4c60e2f3274a69a9078f5088e0f092"
},
"result": {
"success": true,
"resultInfo": [
]
}
}
```
## Dismiss a ToDo Item

This API is used when the consumer wants to dismiss a ToDo from the ToDo
list. When a ToDo is dismissed, the source item is removed from the list
as well.

### Method and Endpoint

| PATCH | /api/v1/me/ToDos/{id}/dismiss|
|--------|-----------------------------|

### Request

| Parameter | Req | Param Type | Data Type | Description                   |
|-----------|-----|------------|-----------|-------------------------------|
| id        | Req | path       | string    | Identifier for the ToDo item. |

### Response

| Parameter | Req  | Data Type                 | Description                                                                                                                  |
|------------|-----|----------|-----------------------------------------------|
| result    | Cond | [ResultType](#resulttype) | Result information. Condition: Only returned when the request fails. No content returned for success (HTTP status code 204). |

### Sample API Usage

#### Request URL

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/ToDos/1c1acf920e48487abada730cc8851640/dismiss |
|------------------------------------------------------------------------|

#### Response

| 204 No Content |
|----------------|

# Bills

This set of APIs allows you to:

-   [Get a list of bills or a specific bill](#get-bills)

-   [Get bill detail](#get-bill-detail)

-   [Update a bill](#update-a-bill)

## Get Bills

The Bills Get API returns the list of all due bills for a given
consumer. The list will include any unpaid e-bills and bill due alerts
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

<table>
<colgroup>
<col style="width: 13%" />
<col style="width: 4%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 64%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Param Type</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>id</td>
<td>Opt</td>
<td>path</td>
<td>string</td>
<td>Identifier for the bill.</td>
</tr>
<tr class="even">
<td>numberOfDays</td>
<td>Opt</td>
<td>query</td>
<td>string</td>
<td>Number of days from the StartingDate that the list of bills returned
will encompass. If a filter is not supplied, all bills for a consumer
are returned.</td>
</tr>
<tr class="odd">
<td>startingDate</td>
<td>Opt</td>
<td>query</td>
<td>string</td>
<td><p>Date that is the starting point for the list of bills to be
returned.<br />
Must contain a valid date in the format yyyy-MM-dd</p>
<p>If a filter is not supplied, all bills for a consumer are
returned.</p></td>
</tr>
</tbody>
</table>

### Response

| Parameter | Req | Data Type                 | Description                                            |
|----------|-----|----------|------------------------------------------------|
| data      | Req | Array of [Bill](#bill)    | There is an empty array if there is no data to return. |
| result    | Req | [ResultType](#resulttype) | Result information.                                    |

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

<table>
<colgroup>
<col style="width: 13%" />
<col style="width: 4%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 64%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Param Type</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>id</td>
<td>Req</td>
<td>path</td>
<td>string</td>
<td>Identifier for the bill.</td>
</tr>
<tr class="even">
<td>redirect</td>
<td>Opt</td>
<td>query</td>
<td>boolean</td>
<td><p>Indicates if the bill detail URL should be returned in a redirect
URL.</p>
<p>True – Return HTTP response 307 (Temporary Redirect), which contains
the bill detail URL in the Location header. This is the default if not
provided in the request.</p>
<p>False – Return the bill detail URL in the response body.</p></td>
</tr>
</tbody>
</table>

### Response

If the value of the redirect parameter in the request is:

-   **true** or null, HTTP response 307 is returned for successful
    requests. The Location header contains the bill detail URL.

-   **false**, the bill detail URL is returned in the response.

| Parameter | Req | Data Type                 | Description                                                              |
|----------|-----|----------|------------------------------------------------|
| data      | Req | string                    | Bill detail URL. There is an empty string if there is no data to return. |
| result    | Req | [ResultType](#resulttype) | Result information.                                                      |

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

<table>
<colgroup>
<col style="width: 12%" />
<col style="width: 4%" />
<col style="width: 8%" />
<col style="width: 9%" />
<col style="width: 64%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Param Type</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>id</td>
<td>Req</td>
<td>path</td>
<td>string</td>
<td>Identifier for the bill.</td>
</tr>
<tr class="even">
<td>note</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td>Optional note entered by the consumer with information about the
bill and its resolution. Length: 1-80</td>
</tr>
<tr class="odd">
<td>reason</td>
<td>Req</td>
<td>body</td>
<td>string</td>
<td><p>Indicates the method of the bill’s resolution. Valid values:</p>
<p>NoneSpecified<br />
Bank<br />
Check<br />
Cash<br />
NotPaid<br />
Other<br />
BillerWebSite<br />
Phone<br />
Mail<br />
Office<br />
ZeroBalanceBill<br />
ContestedBill</p></td>
</tr>
</tbody>
</table>

### Response

| Parameter | Req  | Data Type                 | Description                                                                                                                                  |
|------------|-----|-----------|---------------------------------------------|
| result    | Cond | [ResultType](#resulttype) | Result associated with the request. Condition: Only returned when the request fails. No content returned for success (HTTP status code 204). |

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

# 

# E-bill Activation

These APIs are an extension to the Payee APIs. They are only applicable
to the payees that support and can deliver bills electronically. For an
e‑bill capable payee, these APIs can be used to offer and manage e-bill
activation and service.

-   [Begin the process of activating e-bill service for a
    payee](#get-payee-e-bill-capability)

-   [Complete the process of activating e‑bill service for a
    payee](#activate-payee-e-bill-service)

-   [Get e-bill service information for a
    payee](#get-payee-e-bill-service)

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

<table>
<colgroup>
<col style="width: 10%" />
<col style="width: 4%" />
<col style="width: 14%" />
<col style="width: 69%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>data</td>
<td>Req</td>
<td><a href="#ebillcapability">EbillCapability</a></td>
<td>Information required for e-bill activation. Empty if there is no
data to return.</td>
</tr>
<tr class="even">
<td>result</td>
<td>Cond</td>
<td><a href="#resulttype">ResultType</a></td>
<td>Result information.<br />
Condition: Only returned when the request fails. No result content
returned for success (HTTP status code 200). (Known issue: result is
currently being returned in response for success.)</td>
</tr>
</tbody>
</table>

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
| ebillServiceActivateInput | Req | body       | [EbillServiceActivateInput](#ebillserviceactivateinput) | Information for e-bill service activation. |

### Response

| Parameter | Req | Data Type                                                 | Description                                              |
|---------|----|------------|------------------------------------------------|
| data      | Req | [EbillServiceActivateOutput](#ebillserviceactivateoutput) | E-bill service information. Empty if the request failed. |
| result    | Req | [ResultType](#resulttype)                                 | Result information.                                      |

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

<table>
<colgroup>
<col style="width: 10%" />
<col style="width: 4%" />
<col style="width: 14%" />
<col style="width: 69%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>data</td>
<td>Req</td>
<td><a href="#ebillaccountdetail">EbillAccountDetail</a></td>
<td>E-bill service information.</td>
</tr>
<tr class="even">
<td>result</td>
<td>Cond</td>
<td><a href="#resulttype">ResultType</a></td>
<td>Result information.<br />
Condition: Only returned when the request fails. No result content
returned for success. (Known issue: result is currently being returned
in response for success.)</td>
</tr>
</tbody>
</table>

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

<table>
<colgroup>
<col style="width: 10%" />
<col style="width: 4%" />
<col style="width: 15%" />
<col style="width: 68%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>result</td>
<td>Cond</td>
<td><a href="#resulttype">ResultType</a></td>
<td>Result information.<br />
Condition: Only returned when the request fails. No content returned for
success (HTTP status code 204).</td>
</tr>
</tbody>
</table>

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
| result    | Cond | [ResultType](#resulttype) | Result associated with the request. Condition: Only returned when the request fails. No content returned for success (HTTP status code 204). |

### Sample API Usage

#### Request URL

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/payees/c7b699810d0b40178810b0a5d8feb02f/ebillService |
|------------------------------------------------------------------------|

#### Response

| 204 No Content |
|----------------|

# BankAccounts

A bank account is the financial account of the consumer at the financial
institution that has been enabled to fund a bill payment. The bank
accounts will be set up for use with bill pay using existing Builder
APIs.

This set of APIs allows the calling application to:

-   [Retrieve a financial institution
    name](#get-financial-institution-name)

-   Get a list of bank accounts to be used to fund a payment

<!-- -->

- -  [Managed user](#get-bank-accounts-for-managed-user-list)

- -  [Consumer](#get-bank-accounts-for-a-consumer-list)

<!-- -->

-   Get a specific bank account

<!-- -->

- -  [Managed user](#get-a-bank-account-for-a-managed-user-single)

- -  [Consumer](#get-a-bank-account-for-a-consumer-single)

<!-- -->

-   [Add a bank account for a managed
    user](#add-a-bank-account-for-a-managed-user)

-   Update a bank account

<!-- -->

- -   [Managed user](#update-a-bank-account-for-a-managed-user)

- -  [Consumer](#update-a-bank-account-for-a-consumer)

<!-- -->

-   [Delete a bank account for a managed
    user](#delete-a-bank-account-for-a-managed-user)

-   [Get the unmasked account number for a bank
    account](#get-an-unmasked-bank-account-number)

## Get Financial Institution Name

The Financial Institution Get API is used to retrieve the FI name for a
given routing number.

### Method and Endpoint

| GET | /api/v1/financiallnstitution|
|-----|----------------------------|

### Request

<table>
<colgroup>
<col style="width: 16%" />
<col style="width: 4%" />
<col style="width: 9%" />
<col style="width: 7%" />
<col style="width: 61%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Param Type</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>routingTransitNumber</td>
<td>Req</td>
<td>query</td>
<td>string</td>
<td><p>Routing and transit number for a bank account. Length: 9</p>
<p>Pattern: ^[0-9]{9}$</p></td>
</tr>
</tbody>
</table>

### Response

| Parameter | Req  | Data Type                                               | Description                                                        |
|-----------|-----|-----------|----------------------------------------------|
| data      | Cond | [FinancialInstitutionModel](#financialinstitutionmodel) | Response data. Condition: Always returned for successful response. |
| result    | Req  | [ResultType](#resulttype)                               | Result information.                                                |

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
Maintenance](#sample-api-usage-authentication-request-for-user-maintenance).

Because the maintenance grant is scoped to a tenant, the user ID for the
consumer must be provided in the BankAccounts Get request.

### Method and Endpoint

| GET | /api/v1/users/{userId}/bankAccounts|
|-----|----------------------------|

### Request

<table>
<colgroup>
<col style="width: 16%" />
<col style="width: 4%" />
<col style="width: 9%" />
<col style="width: 7%" />
<col style="width: 61%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Param Type</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>userId</td>
<td>Req</td>
<td>path</td>
<td>string</td>
<td>Identifier for the managed user.</td>
</tr>
<tr class="even">
<td>returnInactiveBankAccounts</td>
<td>Opt</td>
<td>query</td>
<td>boolean</td>
<td><p>Indicates whether to return inactive bank accounts in the
response.</p>
<p>True – Return inactive bank accounts</p>
<p>False – Do not return inactive bank accounts. This is the
default.</p></td>
</tr>
<tr class="odd">
<td>idType</td>
<td>Opt</td>
<td>query</td>
<td>string</td>
<td>Identifies the user ID type. Valid values: SubscriberId,
ExternalSubscriberId, CheckFreeNextUserId<br />
CheckFreeNextUserId is the default.</td>
</tr>
</tbody>
</table>

### Response

| Parameter | Req | Data Type                                            | Description                                                                                                          |
|-----------|----|-----------|-----------------------------------------------|
| data      | Req | Array of [BankAccountGetModel](#bankaccountgetmodel) | Collection of all bank accounts available to fund a payment. Empty if there is no data to return or an error occurs. |
| result    | Req | [ResultType](#resulttype)                            | Result information.                                                                                                  |

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

<table>
<colgroup>
<col style="width: 19%" />
<col style="width: 4%" />
<col style="width: 8%" />
<col style="width: 7%" />
<col style="width: 58%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Param Type</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>returnInactiveBankAccounts</td>
<td>Opt</td>
<td>query</td>
<td>boolean</td>
<td><p>Indicates whether to return inactive bank accounts in the
response.</p>
<p>True – Return inactive bank accounts</p>
<p>False – Do not return inactive bank accounts. This is the
default.</p></td>
</tr>
</tbody>
</table>

### Response

| Parameter | Req | Data Type                                            | Description                                                                                                          |
|-----------|----|-----------|-----------------------------------------------|
| data      | Req | Array of [BankAccountGetModel](#bankaccountgetmodel) | Collection of all bank accounts available to fund a payment. Empty if there is no data to return or an error occurs. |
| result    | Req | [ResultType](#resulttype)                            | Result information.                                                                                                  |

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
Maintenance](#sample-api-usage-authentication-request-for-user-maintenance).

Because the maintenance grant is scoped to a tenant, the user ID for the
consumer must be provided in the BankAccounts Get request.

### Method and Endpoint

| GET | /api/v1/users/{userId}/bankAccounts/{bankAccountCommonId}|
|-----|----------------------------|

### Request

<table>
<colgroup>
<col style="width: 14%" />
<col style="width: 4%" />
<col style="width: 9%" />
<col style="width: 7%" />
<col style="width: 63%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Param Type</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>userId</td>
<td>Req</td>
<td>path</td>
<td>string</td>
<td>Identifier for the managed user.</td>
</tr>
<tr class="even">
<td>bankAccountCommonId</td>
<td>Req</td>
<td>path</td>
<td>string</td>
<td>Identifier for the bank account.</td>
</tr>
<tr class="odd">
<td>idType</td>
<td>Opt</td>
<td>query</td>
<td>string</td>
<td>Identifies the user ID type. Valid values: SubscriberId,
ExternalSubscriberId, CheckFreeNextUserId<br />
CheckFreeNextUserId is the default.</td>
</tr>
</tbody>
</table>

### Response

| Parameter | Req | Data Type                                   | Description                         |
|------------|-----|-----------|---------------------------------------------|
| data      | Req | [BankAccountGetModel](#bankaccountgetmodel) | Information about the bank account. |
| result    | Req | [ResultType](#resulttype)                   | Result information.                 |

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
| data      | Req | [BankAccountGetModel](#bankaccountgetmodel) | Information about the bank account. |
| result    | Req | [ResultType](#resulttype)                   | Result information.                 |

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
Maintenance](#sample-api-usage-authentication-request-for-user-maintenance).

Because the maintenance grant is scoped to a tenant, the user ID for the
consumer must be provided in the BankAccounts Post request.

### Method and Endpoint

| POST | /api/v1/me/users/{userId}/bankAccounts|
|------|---------------------------------------|

###  Request

<table>
<colgroup>
<col style="width: 18%" />
<col style="width: 4%" />
<col style="width: 12%" />
<col style="width: 12%" />
<col style="width: 52%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Param Type</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>userId</td>
<td>Req</td>
<td>path</td>
<td>string</td>
<td>Identifier for the managed user.</td>
</tr>
<tr class="even">
<td>idType</td>
<td>Opt</td>
<td>query</td>
<td>string</td>
<td>Identifies the user ID type. Valid values: SubscriberId,
ExternalSubscriberId, CheckFreeNextUserId<br />
CheckFreeNextUserId is the default.</td>
</tr>
<tr class="odd">
<td>routingTransitNumber</td>
<td>Req</td>
<td>body</td>
<td>string</td>
<td><p>Routing and transit number for the bank account. Length: 9<br />
Must be numeric</p>
<p>Pattern: ^[0-9]{9}$</p></td>
</tr>
<tr class="even">
<td>accountNumber</td>
<td>Req</td>
<td>body</td>
<td>string</td>
<td><p>Consumer’s bank account number. Length: 1–22</p>
<p>Pattern: ^[a-zA-Z0-9]{1,22}$</p>
<p>Must contain uppercase characters when alphabetic characters are part
of the account number.</p></td>
</tr>
<tr class="odd">
<td>accountType</td>
<td>Req</td>
<td>body</td>
<td>string</td>
<td>The type of bank account. Valid values: "Checking", "Savings",
"InstallmentLoan", "IndividualRetirement", "CommercialLoan",
"MoneyMarket", "LineOfCredit", "Brokerage", "SpecialDeposit"</td>
</tr>
<tr class="even">
<td>nickname</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td><p>A description of the bank account used to help identify it in a
list. Length: 1–30</p>
<p>Pattern: ^[a-zA-Z0-9_(){}&amp;@!+#.'$,%^ *-]*</p>
<p>Do not enter any sensitive information such as the account
number.</p></td>
</tr>
<tr class="odd">
<td>startingCheckNumber</td>
<td>Opt</td>
<td>body</td>
<td>integer</td>
<td><p>Starting check number for paper drafts. Maximum value: 9999</p>
<p>Valid values: 1-9999</p></td>
</tr>
<tr class="even">
<td>isPreferred</td>
<td>Opt</td>
<td>body</td>
<td>boolean</td>
<td>Indicates if the account is the preferred account. The preferred
account flag indicates the consumer’s preferred choice of bank account
used to fund bill payment transactions. True if it is a preferred
account.</td>
</tr>
<tr class="odd">
<td>isBusiness</td>
<td>Req</td>
<td>body</td>
<td>boolean</td>
<td>Indicates if the account is a business account. True if it is a
business account.</td>
</tr>
<tr class="even">
<td>businessName</td>
<td>Cond</td>
<td>body</td>
<td>string</td>
<td><p>When isBusiness is true, this is the name of the business. Max
length: 40</p>
<p>Pattern: ^[a-zA-Z0-9_(){}&amp;@!+#.'$,%^ *-]*</p></td>
</tr>
<tr class="odd">
<td>primaryAccountOwner</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td><p>Name of the primary owner of this account. If not provided, this
value defaults to the consumer’s name.</p>
<p>Length: 1-35</p>
<p>Pattern: ^[a-zA-Z0-9_(){}&amp;@!+#.'$,%^ *-]*</p></td>
</tr>
<tr class="even">
<td>secondaryAccountOwner</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td><p>Name of the secondary owner of this account, if applicable.</p>
<p>Length: 1-35</p>
<p>Pattern: ^[a-zA-Z0-9_(){}&amp;@!+#.'$,%^ *-]*</p></td>
</tr>
<tr class="odd">
<td>checkPrintAddress</td>
<td>Opt</td>
<td>body</td>
<td><a href="#usaddress">USAddress</a></td>
<td>The U.S. address printed on the checks. If provided, this is the
consumer’s address to be printed in the upper left corner of the
check.</td>
</tr>
<tr class="even">
<td>bankingOptions</td>
<td>Opt</td>
<td>body</td>
<td><a href="#bankingoptions">BankingOptions</a></td>
<td>Banking options when banking service is enabled.</td>
</tr>
</tbody>
</table>

### Response

| Parameter | Req  | Data Type                 | Description                                                        |
|------------|-----|--------|------------------------------------------------|
| data      | Cond | [BaseModel](#basemodel)   | Response data. Condition: Always returned for successful response. |
| result    | Req  | [ResultType](#resulttype) | Result Information.                                                |

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
Maintenance](#sample-api-usage-authentication-request-for-user-maintenance).

Because the maintenance grant is scoped to a tenant, the user ID for the
consumer must be provided in the BankAccounts Patch request.

### Method and Endpoint

| PATCH | /api/v1/me/users/{userId}/bankAccounts/{bankAccountCommonId}|
|--------|--------------------------|

### Request

<table>
<colgroup>
<col style="width: 18%" />
<col style="width: 4%" />
<col style="width: 8%" />
<col style="width: 10%" />
<col style="width: 58%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Param Type</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>userId</td>
<td>Req</td>
<td>path</td>
<td>string</td>
<td>Identifier for the managed user.</td>
</tr>
<tr class="even">
<td>bankAccountCommonId</td>
<td>Req</td>
<td>path</td>
<td>string</td>
<td>Identifier for the bank account.</td>
</tr>
<tr class="odd">
<td>idType</td>
<td>Opt</td>
<td>query</td>
<td>string</td>
<td>Identifies the user ID type. Valid values: SubscriberId,
ExternalSubscriberId, CheckFreeNextUserId<br />
CheckFreeNextUserId is the default.</td>
</tr>
<tr class="even">
<td>nickname</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td><p>A description of the bank account used to help identify it in a
list. Length: 1–30</p>
<p>Pattern: ^[a-zA-Z0-9_(){}&amp;@!+#.'$,%^ *-]*</p>
<p>Do not enter any sensitive information such as the account
number.</p></td>
</tr>
<tr class="odd">
<td>startingCheckNumber</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td><p>Starting check number for paper drafts. Length: 1-9</p>
<p>Pattern: ^[0-9]*</p></td>
</tr>
<tr class="even">
<td>isPreferred</td>
<td>Opt</td>
<td>body</td>
<td>boolean</td>
<td>Indicates if the account is the preferred account. The preferred
account flag indicates the consumer’s preferred choice of bank account
used to fund bill payment transactions. True if it is a preferred
account.<br />
Cannot be changed to “false”. By making another account the preferred
account, isPreferred will become false for this account.</td>
</tr>
<tr class="odd">
<td>isBusiness</td>
<td>Opt</td>
<td>body</td>
<td>boolean</td>
<td>Indicates if the account is a business account. True if it is a
business account.</td>
</tr>
<tr class="even">
<td>businessName</td>
<td>Cond</td>
<td>body</td>
<td>string</td>
<td><p>When isBusiness is true, this is the name of the business. Cannot
have a value when isBusiness is “false”. Max length: 40</p>
<p>Pattern: ^[a-zA-Z0-9_(){}&amp;@!+#.'$,%^ *-]*</p></td>
</tr>
<tr class="odd">
<td>primaryAccountOwner</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td><p>Name of the primary owner of this account. Length: 1-35</p>
<p>Pattern: ^[a-zA-Z0-9_(){}&amp;@!+#.'$,%^ *-]*</p></td>
</tr>
<tr class="even">
<td>secondaryAccountOwner</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td><p>Name of the secondary owner of this account, if applicable.
Length: 1-35</p>
<p>Pattern: ^[a-zA-Z0-9_(){}&amp;@!+#.'$,%^ *-]*</p></td>
</tr>
<tr class="odd">
<td>checkPrintAddress</td>
<td>Opt</td>
<td>body</td>
<td><a href="#usaddress">USAddress</a></td>
<td>The U.S. address printed on the checks. If provided, this is the
consumer’s address to be printed in the upper left corner of the
check.</td>
</tr>
<tr class="even">
<td>bankingOptions</td>
<td>Opt</td>
<td>body</td>
<td><a href="#bankingoptions">BankingOptions</a></td>
<td>Banking options when banking service is enabled.</td>
</tr>
</tbody>
</table>

### Response

| Parameter | Req  | Data Type                 | Description                                                                                                                  |
|-------------|-----|-------|-------------------------------------------------|
| result    | Cond | [ResultType](#resulttype) | Result information. Condition: Only returned when the request fails. No content returned for success (HTTP status code 204). |

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

<table>
<colgroup>
<col style="width: 18%" />
<col style="width: 4%" />
<col style="width: 8%" />
<col style="width: 10%" />
<col style="width: 58%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Param Type</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>bankAccountCommonId</td>
<td>Req</td>
<td>path</td>
<td>string</td>
<td>Identifier for the bank account.</td>
</tr>
<tr class="even">
<td>nickname</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td><p>A description of the bank account used to help identify it in a
list. Length: 1–30</p>
<p>Pattern: ^[a-zA-Z0-9_(){}&amp;@!+#.'$,%^ *-]*</p>
<p>Do not enter any sensitive information such as the account
number.</p></td>
</tr>
<tr class="odd">
<td>startingCheckNumber</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td><p>Starting check number for paper drafts. Length: 1-9</p>
<p>Pattern: ^[0-9]*</p></td>
</tr>
<tr class="even">
<td>isPreferred</td>
<td>Opt</td>
<td>body</td>
<td>boolean</td>
<td>Indicates if the account is the preferred account. The preferred
account flag indicates the consumer’s preferred choice of bank account
used to fund bill payment transactions. True if it is a preferred
account.<br />
Cannot be changed to “false”. By making another account the preferred
account, isPreferred will become false for this account.</td>
</tr>
<tr class="odd">
<td>isBusiness</td>
<td>Opt</td>
<td>body</td>
<td>boolean</td>
<td>Indicates if the account is a business account. True if it is a
business account.</td>
</tr>
<tr class="even">
<td>businessName</td>
<td>Cond</td>
<td>body</td>
<td>string</td>
<td><p>When isBusiness is true, this is the name of the business. Cannot
have a value when isBusiness is “false”. Max length: 40</p>
<p>Pattern: ^[a-zA-Z0-9_(){}&amp;@!+#.'$,%^ *-]*</p></td>
</tr>
<tr class="odd">
<td>primaryAccountOwner</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td><p>Name of the primary owner of this account. Length: 1-35</p>
<p>Pattern: ^[a-zA-Z0-9_(){}&amp;@!+#.'$,%^ *-]*</p></td>
</tr>
<tr class="even">
<td>secondaryAccountOwner</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td><p>Name of the secondary owner of this account, if applicable.
Length: 1-35</p>
<p>Pattern: ^[a-zA-Z0-9_(){}&amp;@!+#.'$,%^ *-]*</p></td>
</tr>
<tr class="odd">
<td>checkPrintAddress</td>
<td>Opt</td>
<td>body</td>
<td><a href="#usaddress">USAddress</a></td>
<td>The U.S. address printed on the checks. If provided, this is the
consumer’s address to be printed in the upper left corner of the
check.</td>
</tr>
<tr class="even">
<td>bankingOptions</td>
<td>Opt</td>
<td>body</td>
<td><a href="#bankingoptions">BankingOptions</a></td>
<td>Banking options when banking service is enabled.</td>
</tr>
</tbody>
</table>

### Response

| Parameter | Req  | Data Type                 | Description                                                                                                                  |
|-------------|-----|-------|-------------------------------------------------|
| result    | Cond | [ResultType](#resulttype) | Result information. Condition: Only returned when the request fails. No content returned for success (HTTP status code 204). |

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
Maintenance](#sample-api-usage-authentication-request-for-user-maintenance).

Because the maintenance grant is scoped to a tenant, the user ID for the
consumer must be provided in the BankAccounts Delete request.

### Method and Endpoint

| DELETE | /api/v1/me/users/{userId}/bankAccounts/{bankAccountCommonId}|
|--------|----------------------------|

### Request

<table>
<colgroup>
<col style="width: 16%" />
<col style="width: 3%" />
<col style="width: 8%" />
<col style="width: 7%" />
<col style="width: 63%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Param Type</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>userId</td>
<td>Req</td>
<td>path</td>
<td>string</td>
<td>Identifier for the managed user.</td>
</tr>
<tr class="even">
<td>bankAccountCommonId</td>
<td>Req</td>
<td>path</td>
<td>string</td>
<td>Identifier for the bank account.</td>
</tr>
<tr class="odd">
<td>idType</td>
<td>Opt</td>
<td>query</td>
<td>string</td>
<td>Identifies the user ID type. Valid values: SubscriberId,
ExternalSubscriberId, CheckFreeNextUserId<br />
CheckFreeNextUserId is the default.</td>
</tr>
</tbody>
</table>

### Response

| Parameter | Req  | Data Type                 | Description                                                                                                                                  |
|------------|------|-------|-----------------------------------------------|
| result    | Cond | [ResultType](#resulttype) | Result associated with the request. Condition: Only returned when the request fails. No content returned for success (HTTP status code 204). |

### Sample API Usage

#### Request URL

https://api-checkfreenext-cert.fiservapps.com/api/v1/me/users/e31cb7a57466e911a83500505689ef1f/bankAccounts/
60411aefbaff493d8a89b86fd1cee178

#### Response

| 204 No Content |
|----------------|

## Get an Unmasked Bank Account Number

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
| result    | Cond | [ResultType](#resulttype) | Result associated with the request. Condition: Only returned when the request fails. No content returned for success (HTTP status code 200). |

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

<table>
<colgroup>
<col style="width: 16%" />
<col style="width: 4%" />
<col style="width: 8%" />
<col style="width: 7%" />
<col style="width: 62%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Param Type</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>cardNumber</td>
<td>Req</td>
<td>body</td>
<td>string</td>
<td><p>Consumer card account number. Must be numeric. Length: 15-19</p>
<p>Pattern: ^[0-9]*</p></td>
</tr>
<tr class="even">
<td>cardType</td>
<td>Req</td>
<td>body</td>
<td>string</td>
<td>Indicates the type of card. Valid values: AmericanExpress, Discover,
MasterCard, Visa</td>
</tr>
<tr class="odd">
<td>expirationMonth</td>
<td>Req</td>
<td>body</td>
<td>integer</td>
<td><p>Month in which the card expires. Length: 2. Value: 01–12</p>
<p>Pattern: ^([1-9]|1[012])$</p></td>
</tr>
<tr class="even">
<td>expirationYear</td>
<td>Req</td>
<td>body</td>
<td>integer</td>
<td><p>Year in which the card expires. Length: 4. Format: yyyy</p>
<p>Pattern: ^[0-9]{4}$</p></td>
</tr>
<tr class="odd">
<td>externalAccountDescription</td>
<td>Req</td>
<td>body</td>
<td>string</td>
<td>External account description that is free-form text, such as: “Bank
Name” Visa. Length: 1–32</td>
</tr>
<tr class="even">
<td>nameOnCard</td>
<td>Req</td>
<td>body</td>
<td>string</td>
<td>Name printed on the card. Length: 1–80</td>
</tr>
<tr class="odd">
<td>nickname</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td><p>A description of the card used to help identify it in a list.
Length: 1–30</p>
<p>No validations; free-form text. Do not enter any sensitive
information such as the account number.</p></td>
</tr>
<tr class="even">
<td>cvv</td>
<td>Cond</td>
<td>body</td>
<td>integer</td>
<td><p>The CVV number on the card. Must be numeric. Length: 3-4.</p>
<p>Pattern: ^[0-9]*</p>
<p>This value is not stored or displayed anywhere.</p>
<p>Condition: Required for cards added by consumer. (Not required for
cards added by FI.)</p></td>
</tr>
<tr class="odd">
<td>billingAddress</td>
<td>Req</td>
<td>body</td>
<td><a href="#usaddress">USAddress</a></td>
<td>The billing address for the card.</td>
</tr>
</tbody>
</table>

### Response

<table>
<colgroup>
<col style="width: 16%" />
<col style="width: 4%" />
<col style="width: 9%" />
<col style="width: 69%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>data</td>
<td>Cond</td>
<td><a href="#cardaccountpostresponse">CardAccount<br />
PostResponse</a></td>
<td>Response data. Condition: Always returned for successful
response.</td>
</tr>
<tr class="even">
<td>result</td>
<td>Req</td>
<td><a href="#resulttype">ResultType</a></td>
<td>Result Information.</td>
</tr>
</tbody>
</table>

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

<table>
<colgroup>
<col style="width: 16%" />
<col style="width: 4%" />
<col style="width: 8%" />
<col style="width: 7%" />
<col style="width: 62%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Param Type</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>cardId</td>
<td>Req</td>
<td>path</td>
<td>string</td>
<td>Identifier for the card account.</td>
</tr>
<tr class="even">
<td>billingAddress</td>
<td>Opt</td>
<td>body</td>
<td><a href="#usaddress">USAddress</a></td>
<td>The billing address for the card.</td>
</tr>
<tr class="odd">
<td>expirationMonth</td>
<td>Opt</td>
<td>body</td>
<td>integer</td>
<td><p>Month in which the card expires. Length: 2. Value: 01–12</p>
<p>Pattern: ^([1-9]|1[012])$</p></td>
</tr>
<tr class="even">
<td>expirationYear</td>
<td>Opt</td>
<td>body</td>
<td>integer</td>
<td><p>Year in which the card expires. Length: 4. Format: yyyy</p>
<p>Pattern: ^[0-9]{4}$</p></td>
</tr>
<tr class="odd">
<td>externalAccountDescription</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td>External account description that is free-form text, such as: “Bank
Name” Visa. Length: 1–32</td>
</tr>
<tr class="even">
<td>nameOnCard</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td>Name printed on the card. Length: 1–80</td>
</tr>
<tr class="odd">
<td>nickname</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td>A description of the card used to help identify it in a list. No
validations; free-form text. Do not enter any sensitive information such
as the account number. Length: 1–30</td>
</tr>
<tr class="even">
<td>cvv</td>
<td>Cond</td>
<td>body</td>
<td>integer</td>
<td><p>The CVV number on the card. Must be numeric. Length: 3-4.</p>
<p>Pattern: ^[0-9]*</p>
<p>Condition: For cards added by consumer, required if anything except
nickname and/or externalAccountDescription is modified. Not used for
FI-added cards.</p>
<p>This value is not stored or displayed anywhere.</p></td>
</tr>
</tbody>
</table>

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

# TransactionCalendar

This API allows you to:

-   [Get a list of possible transaction
    dates](#get-transaction-calendar)

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

<table>
<colgroup>
<col style="width: 15%" />
<col style="width: 4%" />
<col style="width: 8%" />
<col style="width: 9%" />
<col style="width: 62%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Param Type</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>transactionDestinationUri</td>
<td>Cond</td>
<td>query</td>
<td>Array of string</td>
<td>The destination for a transaction; this is a URI for a payee, bill,
or ToDo item. Multiple destinations can be provided.<br />
Condition: Either transactionDestinationUri <strong>or</strong>
transactionUri must be provided.</td>
</tr>
<tr class="even">
<td>fundingAccountUri</td>
<td>Opt</td>
<td>query</td>
<td>string</td>
<td>The funding account for the transaction.</td>
</tr>
<tr class="odd">
<td>amount</td>
<td>Opt</td>
<td>query</td>
<td>double</td>
<td><p>The amount of the transaction.</p>
<p>If the consumer provides the amount and the supplied funding account
is a bank account, the amount will be validated against the minimum and
maximum value of the sponsor bill pay profile limit. An error will be
thrown if the amount is not within the bounds of the sponsor bill pay
profile limit.</p></td>
</tr>
<tr class="even">
<td>startDate</td>
<td>Opt</td>
<td>query</td>
<td>string</td>
<td>The desired start date for the list of dates. Format:
yyyy-MM-dd</td>
</tr>
<tr class="odd">
<td>endDate</td>
<td>Opt</td>
<td>query</td>
<td>string</td>
<td>The desired end date for the list of dates. Format: yyyy-MM-dd</td>
</tr>
<tr class="even">
<td>transactionUri</td>
<td>Cond</td>
<td>query</td>
<td>string</td>
<td>The URI for an existing transaction. Condition: Either
transactionDestinationUri <strong>or</strong> transactionUri must be
provided.</td>
</tr>
</tbody>
</table>

### Response

| Parameter | Req | Data Type                                              | Description                                            |
|----------|-----|----------|------------------------------------------------|
| data      | Req | Array of [TransactionCalendar](#transactioncalendar-1) | There is an empty array if there is no data to return. |
| result    | Req | [ResultType](#resulttype)                              | Result information.                                    |

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

<table>
<colgroup>
<col style="width: 11%" />
<col style="width: 4%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 66%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Param Type</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>startDate</td>
<td>Opt</td>
<td>query</td>
<td>string<br />
&lt;date-time&gt;</td>
<td>The desired start date for the list of transactions. Format:
yyyy-MM-dd<br />
Default is 6 months in the past.</td>
</tr>
<tr class="even">
<td>endDate</td>
<td>Opt</td>
<td>query</td>
<td>string<br />
&lt;date-time&gt;</td>
<td>The desired end date for the list of transactions. Format:
yyyy-MM-dd<br />
Default is all transactions scheduled in the future.</td>
</tr>
<tr class="odd">
<td>sort</td>
<td>Opt</td>
<td>query</td>
<td>string</td>
<td><p>Sort the response based on the given input. These are the
eligible values with which to sort: amount, deliveryDate, payeeName,
deliveryTrackingNumber, confirmationNumber, debitDate, fee, note, memo,
deliveryMethod, status, transactionType</p>
<p>A negative sign (-) before the parameter indicates that the response
should be sorted in descending order. For example,
“sort=-amount”</p></td>
</tr>
<tr class="even">
<td>start</td>
<td>Opt</td>
<td>query</td>
<td>integer</td>
<td>Specifies the starting record to be fetched.</td>
</tr>
<tr class="odd">
<td>limit</td>
<td>Opt</td>
<td>query</td>
<td>integer</td>
<td>Specifies the number of records to be fetched.</td>
</tr>
<tr class="even">
<td>payeeUri</td>
<td>Opt</td>
<td>query</td>
<td>string</td>
<td>The URI to the payee that this transaction is associated with.</td>
</tr>
<tr class="odd">
<td>status</td>
<td>Opt</td>
<td>query</td>
<td>string</td>
<td>The status of the payment transaction.</td>
</tr>
<tr class="even">
<td>fundingAccountUri</td>
<td>Opt</td>
<td>query</td>
<td>string</td>
<td>The funding account for the transaction.</td>
</tr>
<tr class="odd">
<td>minAmount</td>
<td>Opt</td>
<td>query</td>
<td>double</td>
<td>The minimum transaction amount.</td>
</tr>
<tr class="even">
<td>maxAmount</td>
<td>Opt</td>
<td>query</td>
<td>double</td>
<td>The maximum transaction amount.</td>
</tr>
</tbody>
</table>

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

<table>
<colgroup>
<col style="width: 15%" />
<col style="width: 3%" />
<col style="width: 8%" />
<col style="width: 7%" />
<col style="width: 63%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Param Type</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>allowDuplicateTransaction</td>
<td>Opt</td>
<td>query</td>
<td>boolean</td>
<td><p>Indicates that duplicate transactions are allowed. </p>
<p>Valid values:<br />
true – Allow duplicate transactions<br />
false – Check for duplicate transactions. This is the default.</p>
<p>A transaction cannot be a duplicate unless the
allowDuplicateTransaction flag is true. A duplicate is defined as having
the same payee, transaction amount, and transaction date.</p>
<p>This does not apply to SameDay Payments. Duplicate transactions are
not allowed for SameDay Payments.</p></td>
</tr>
<tr class="even">
<td>transaction</td>
<td>Req</td>
<td>body</td>
<td>Array of <a href="#transaction">Transaction</a></td>
<td>List of transactions to be scheduled for the consumer. Limited to 30
or fewer per request.</td>
</tr>
</tbody>
</table>

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

<table>
<colgroup>
<col style="width: 12%" />
<col style="width: 4%" />
<col style="width: 8%" />
<col style="width: 9%" />
<col style="width: 64%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Param Type</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>transactionId</td>
<td>Req</td>
<td>path</td>
<td>string</td>
<td>Identifier for the transaction.</td>
</tr>
<tr class="even">
<td>amount</td>
<td>Opt</td>
<td>body</td>
<td>double</td>
<td><p>The amount of the transaction.</p>
<p>Pattern: ^\d+(\.\d{1,2})?$</p></td>
</tr>
<tr class="odd">
<td>deliveryDate</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td>The date that the payment transaction is to be delivered to its
destination in yyyy-MM-dd format.</td>
</tr>
<tr class="even">
<td>fundingAccountUri</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td>The funding account URI for the transaction. Account types that are
eligible to be used are those enabled for Bill Payment (service) for the
tenant/sponsor.</td>
</tr>
<tr class="odd">
<td>memo</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td>Transaction memo. Maximum of 34 characters. This text will be
printed on the check sent to the payee for this payment. A payment memo
is only used for payments that are to be processed via a paper
check.</td>
</tr>
<tr class="even">
<td>note</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td><p>A consumer’s “note to self.” This note is not submitted to the
payee. Length: 0–255</p>
<p>Pattern:
^[\x2A-\x2E\x30-\x39\x40-\x5A\x5F\x5E\x61-\x7A\x20\x21\x23-\x25]+$</p>
<p>The note field only allows the following character sets: a-z, A-Z,
0-9, _ @!+#.$,%^*-</p></td>
</tr>
<tr class="odd">
<td>withdrawNow</td>
<td>Opt</td>
<td>body</td>
<td>boolean</td>
<td>Reserved for future use.</td>
</tr>
<tr class="even">
<td>cavv</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td>Placeholder for 3-D Secure. Cardholder Authentication Verification
Value. Used if the transaction is funded by a card account and the
institution is participating in 3-D Secure.</td>
</tr>
</tbody>
</table>

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

# AutomaticTransactions

This set of APIs allows you to:

-   [Get a list of automatic transactions for a
    consumer](#get-automatic-transactions-list)

-   [Get a specific automatic transaction for a
    consumer](#get-automatic-transaction-single)

-   [Set up a new recurring model for one of a consumer’s
    payees](#set-up-an-automatic-transaction)

-   [Update an automatic transaction](#update-an-automatic-transaction)

-   [Cancel an automatic transaction](#delete-an-automatic-transaction)

## Get Automatic Transactions (List)

The Automatic Transactions Get API returns the list of recurring payment
models and automatic payment models that have been set up for a specific
consumer.

### Method and Endpoint

| GET | /api/v1/me/automaticTransactions|
|-----|----------------------------|

### Request

<table>
<colgroup>
<col style="width: 14%" />
<col style="width: 4%" />
<col style="width: 8%" />
<col style="width: 8%" />
<col style="width: 64%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Param Type</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>pageSize</td>
<td>Opt</td>
<td>query</td>
<td>integer</td>
<td>Specifies the number of items per page.</td>
</tr>
<tr class="even">
<td>pageNumber</td>
<td>Opt</td>
<td>query</td>
<td>integer</td>
<td>Specifies the page number from which results should be
returned.</td>
</tr>
<tr class="odd">
<td>returnInactiveAutomatic<br />
Transactions</td>
<td>Opt</td>
<td>query</td>
<td>boolean</td>
<td><p>Indicates whether to return inactive automatic transactions in
the response.</p>
<p>True – Return inactive automatic transactions</p>
<p>False – Do not return inactive automatic transactions. This is the
default.</p></td>
</tr>
<tr class="even">
<td>destinationUri</td>
<td>Opt</td>
<td>query</td>
<td>string</td>
<td>The destination of the automatic transaction. Can be either a bill
or a payee.</td>
</tr>
</tbody>
</table>

### Response

<table>
<colgroup>
<col style="width: 13%" />
<col style="width: 4%" />
<col style="width: 13%" />
<col style="width: 68%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>automaticTransactions</td>
<td>Req</td>
<td>Array of <a
href="#automatictransactionoutputdetail">AutomaticTransaction<br />
OutputDetail</a></td>
<td>List of found automatic transactions. There is an empty array if
there is no data to return.</td>
</tr>
<tr class="even">
<td>result</td>
<td>Cond</td>
<td><a href="#resulttype">ResultType</a></td>
<td>Result information. Condition: Only returned when the request fails.
No content returned for success (HTTP status code 200).</td>
</tr>
</tbody>
</table>

### Sample API Usage

#### Response
```json
{
"automaticTransactions": [
{
"modifiableFields": [
"fundingAccountUri",
"amount",
"endDate",
"endTransactionAmount",
"frequency",
"initiationDate",
"maximumTransactionCount",
"memo",
"transactionScheduledAlert",
"transactionSentAlert",
"recurringScheduleExpireAlert"
],
"id": "b3fea21d83d04051a6241e9284812ee6",
"self":
"/api/v1/me/automatictransactions/b3fea21d83d04051a6241e9284812ee6",
"status": "Active",
"fundingAccountUri":
"/api/v1/me/bankAccounts/8af5bc0b652e41ce9352f24b1ab4b553",
"destinationUri":
"/api/v1/me/payees/a9667fb77a3e4dad99f44250267a9aed",
"billTransactionSchedule": null,
"recurringTransactionSchedule": {
"amount": 15.0,
"endDate": "2023-09-19",
"frequency": "Every3Months",
"initialTransactionAmount": 15.0,
"initiationDate": "2022-09-19",
"nextTransactionDate": "2022-09-19",
"maximumTransactionCount": 5,
"remainingTransactionCount": 4,
"memo": "",
"transactionScheduledAlert": true,
"transactionSentAlert": true,
"recurringScheduleExpireAlert": true,
"duration": "XNumberOfPayments",
"nextTransactionGenerationDate": "2022-12-19"
}
},
{
"modifiableFields": [
"fundingAccountUri",
"amount",
"endDate",
"endTransactionAmount",
"frequency",
"initiationDate",
"maximumTransactionCount",
"memo",
"transactionScheduledAlert",
"transactionSentAlert",
"recurringScheduleExpireAlert"
],
"id": "e37ea97a92a341f2bd844e545f087f21",
"self":
"/api/v1/me/automatictransactions/e37ea97a92a341f2bd844e545f087f21",
"status": "Active",
"fundingAccountUri":
"/api/v1/me/bankAccounts/8af5bc0b652e41ce9352f24b1ab4b553",
"destinationUri":
"/api/v1/me/payees/a9667fb77a3e4dad99f44250267a9aed",
"billTransactionSchedule": null,
"recurringTransactionSchedule": {
"amount": 23.33,
"endDate": "2022-09-26",
"frequency": "Weekly",
"initialTransactionAmount": 23.33,
"initiationDate": "2022-09-19",
"nextTransactionDate": "2022-09-19",
"maximumTransactionCount": 2,
"remainingTransactionCount": 1,
"memo": "",
"transactionScheduledAlert": true,
"transactionSentAlert": true,
"recurringScheduleExpireAlert": true,
"duration": "XNumberOfPayments",
"nextTransactionGenerationDate": "2022-09-26"
}
}
]
}
```
## Get Automatic Transaction (Single)

The Automatic Transactions Get API can be invoked with a specific
automatic transaction identifier to get the respective automatic payment
plan.

### Method and Endpoint

| GET | /api/v1/me/automaticTransactions/{automaticTransactionId}|
|-----|----------------------------|

### Request

| Parameter              | Req | Param Type | Data Type | Description                               |
|-----------|-----|-------|-------|--------------------------------------------|
| automaticTransactionId | Req | path       | string    | Identifier for the automatic transaction. |

### Response

<table>
<colgroup>
<col style="width: 13%" />
<col style="width: 4%" />
<col style="width: 13%" />
<col style="width: 68%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>automaticTransactions</td>
<td>Req</td>
<td><a
href="#automatictransactionoutputdetail">AutomaticTransaction<br />
OutputDetail</a></td>
<td>Details for the automatic payment plan.</td>
</tr>
<tr class="even">
<td>result</td>
<td>Cond</td>
<td><a href="#resulttype">ResultType</a></td>
<td>Result information. Condition: Only returned when the request fails.
No content returned for success (HTTP status code 200).</td>
</tr>
</tbody>
</table>

### Sample API Usage

#### Response
```json
{
"modifiableFields": [
"fundingAccountUri",
"amount",
"endDate",
"endTransactionAmount",
"frequency",
"initiationDate",
"maximumTransactionCount",
"memo",
"transactionScheduledAlert",
"transactionSentAlert",
"recurringScheduleExpireAlert"
],
"id": "b3fea21d83d04051a6241e9284812ee6",
"self":
"/api/v1/me/automatictransactions/b3fea21d83d04051a6241e9284812ee6",
"status": "Active",
"fundingAccountUri":
"/api/v1/me/bankAccounts/8af5bc0b652e41ce9352f24b1ab4b553",
"destinationUri":
"/api/v1/me/payees/a9667fb77a3e4dad99f44250267a9aed",
"billTransactionSchedule": null,
"recurringTransactionSchedule": {
"amount": 15.0,
"endDate": "2023-09-19",
"frequency": "Every3Months",
"initialTransactionAmount": 15.0,
"initiationDate": "2022-09-19",
"nextTransactionDate": "2022-09-19",
"maximumTransactionCount": 5,
"remainingTransactionCount": 4,
"memo": "",
"transactionScheduledAlert": true,
"transactionSentAlert": true,
"recurringScheduleExpireAlert": true,
"duration": "XNumberOfPayments",
"nextTransactionGenerationDate": "2022-12-19"
}
}
```
## Set Up an Automatic Transaction

The Automatic Transactions Post API enables a consumer to set up a new
recurring model for one of that consumer’s payees. A recurring model is
an automatic payment instruction set up by the consumer to pay a
specified amount to one of the consumer's payees on a regular basis. The
consumer specifies the starting date and frequency of payments.

### Method and Endpoint

| POST | /api/v1/me/automaticTransactions |
|------|------------------|

### Request

<table>
<colgroup>
<col style="width: 17%" />
<col style="width: 4%" />
<col style="width: 8%" />
<col style="width: 16%" />
<col style="width: 52%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Param Type</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>fundingAccountUri</td>
<td>Req</td>
<td>body</td>
<td>string</td>
<td>The source funding account URI for the automatic transaction.</td>
</tr>
<tr class="even">
<td>destinationUri</td>
<td>Req</td>
<td>body</td>
<td>string</td>
<td>The destination of the automatic transaction. Can be either a bill
or a payee.</td>
</tr>
<tr class="odd">
<td>billTransactionSchedule</td>
<td>Cond</td>
<td>body</td>
<td><a href="#billtransactionschedule">BillTransaction<br />
Schedule</a></td>
<td><p>BillTransactionSchedule defines a transaction schedule for an
e-bill automatic payment.</p>
<p>Condition: Either billTransactionSchedule or
recurringTransactionSchedule is required.</p></td>
</tr>
<tr class="even">
<td>recurringTransactionSchedule</td>
<td>Cond</td>
<td>body</td>
<td><a href="#recurringtransactionschedule">RecurringTransaction<br />
Schedule</a></td>
<td><p>RecurringTransactionSchedule defines a transaction schedule for a
payee.</p>
<p>Condition: Either billTransactionSchedule or
recurringTransactionSchedule is required.</p></td>
</tr>
<tr class="odd">
<td>cavv</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td>Placeholder for 3-D Secure. Cardholder Authentication Verification
Value. Used if the transaction is funded by a card account and the
institution is participating in 3-D Secure.<br />
Only applies to the first transaction generated from a recurring
transaction schedule (if the transaction is within 90 days).</td>
</tr>
</tbody>
</table>

### Response

| Parameter | Req  | Data Type                 | Description                                                        |
|-------------|-----|-------|-------------------------------------------------|
| data      | Cond | [BaseModel](#basemodel)   | Response data. Condition: Always returned for successful response. |
| result    | Req  | [ResultType](#resulttype) | Result information.                                                |

### Sample API Usage

#### Request Body
```json
{
"fundingAccountUri":
"/api/v1/me/bankAccounts/8af5bc0b652e41ce9352f24b1ab4b553",
"destinationUri":
"/api/v1/me/payees/a9667fb77a3e4dad99f44250267a9aed",
"billTransactionSchedule": null,
"recurringTransactionSchedule": {
"amount": 15.0,
"endDate": null,
"endTransactionAmount": null,
"frequency": "Weekly",
"initialTransactionAmount": null,
"initiationDate": "2018-06-29",
"maximumTransactionCount": 15,
"memo": null,
"transactionScheduledAlert": true,
"transactionSentAlert": true,
"recurringScheduleExpireAlert": true
}
}
```
#### Response
```json
{
"data": {
"self":
"/api/v1/me/automatictransactions/b3fea21d83d04051a6241e9284812ee6",
"id": "b3fea21d83d04051a6241e9284812ee6"
},
"result": {
"success": true,
"resultInfo": [
{
"resultCategory": "Info",
"code": "0000",
"field": null,
"fieldPath": null,
"listItemId": null,
"description": "Success."
}
]
}
}
```
## Update an Automatic Transaction

The Automatic Transactions Patch API enables changes to be made to an
automatic transaction that has already been set up.

### Method and Endpoint

| PATCH | /api/v1/me/automaticTransactions/{automaticTransactionId}|
|--------|--------------------------|

### Request

<table>
<colgroup>
<col style="width: 17%" />
<col style="width: 4%" />
<col style="width: 8%" />
<col style="width: 16%" />
<col style="width: 52%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Param Type</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>automaticTransactionId</td>
<td>Req</td>
<td>path</td>
<td>string</td>
<td>Identifier for the automatic transaction.</td>
</tr>
<tr class="even">
<td>fundingAccountUri</td>
<td>Opt</td>
<td>body</td>
<td>string</td>
<td>The source funding account URI for the automatic transaction.</td>
</tr>
<tr class="odd">
<td>billTransactionSchedule</td>
<td>Opt</td>
<td>body</td>
<td><a href="#billtransactionschedulepatch">BillTransaction<br />
SchedulePatch</a></td>
<td>BillTransactionSchedule defines a transaction schedule for an e-bill
automatic payment.</td>
</tr>
<tr class="even">
<td>recurringTransactionSchedule</td>
<td>Opt</td>
<td>body</td>
<td><a
href="#recurringtransactionschedulepatch">RecurringTransaction<br />
SchedulePatch</a></td>
<td>RecurringTransactionSchedule defines a transaction schedule for a
payee.</td>
</tr>
<tr class="odd">
<td><strong>cavv</strong></td>
<td><strong>Opt</strong></td>
<td><strong>body</strong></td>
<td>string</td>
<td>Placeholder for 3-D Secure. Cardholder Authentication Verification
Value. Used if the transaction is funded by a card account and the
institution is participating in 3-D Secure.<br />
Only applies to the first transaction generated from a recurring
transaction schedule (if the transaction is within 90 days).</td>
</tr>
</tbody>
</table>

### Response

| Parameter | Req  | Data Type                                                    | Description                                                                                                                                                                        |
|------------|-----|------------|--------------------------------------------|
| data      | Cond | Array of [TransactionsNotModified](#transactionsnotmodified) | List of transactions that could not be modified. Condition: A spawned payment could not be modified. Only applies to transactions generated from a recurring transaction schedule. |
| result    | Cond | [ResultType](#resulttype)                                    | Result information. Condition: Only returned when the request fails. No content returned for success (HTTP status code 204).                                                       |

### Sample API Usage

#### Request URL

| https://api-checkfreenext-cert.fiservapps.com/api/v1/me/automaticTransactions/e149c86d52554704b1d000fc4141ae08 |
|------------------------------------------------------------------------|

#### Request Body
```json
{
"fundingAccountUri":
"/api/v1/me/bankAccounts/545a5e3d624743b09c4933a156bad5af"
}
```
#### Response

| 204 No Content |
|----------------|

## Delete an Automatic Transaction

The Automatic Transactions Delete API enables an automatic transaction
that has been set up to be canceled.

### Method and Endpoint

| DELETE | /api/v1/me/automaticTransactions/{automaticTransactionId}|
|--------|----------------------------|

### Request

<table>
<colgroup>
<col style="width: 16%" />
<col style="width: 3%" />
<col style="width: 8%" />
<col style="width: 7%" />
<col style="width: 63%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Param Type</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>automaticTransactionId</td>
<td>Req</td>
<td>path</td>
<td>string</td>
<td>Identifier for the automatic transaction.</td>
</tr>
<tr class="even">
<td>cancelPendingTransactions</td>
<td>Opt</td>
<td>query</td>
<td>boolean</td>
<td><p>Indicates if pending transactions should be canceled.</p>
<p>true – Yes, cancel pending payments.</p>
<p>false – No, do not cancel pending payments. This is the default.</p>
<p>The option to cancel pending transactions only applies to automatic
transactions that have a RecurringTransactionSchedule. This does not
apply to e-bill automatic payments (BillTransactionSchedule).</p></td>
</tr>
</tbody>
</table>

### Response

| Parameter | Req  | Data Type                                                                    | Description                                                                                                                                                                                                                     |
|-------------|-----|---------------|----------------------------------------|
| data      | Cond | Array of [PendingTransactionsNotCancelled](#pendingtransactionsnotcancelled) | List of transactions that could not be canceled. Condition: cancelPendingTransactions is true in the request and a payment could not be canceled. Only applies to transactions generated from a recurring transaction schedule. |
| result    | Cond | [ResultType](#resulttype)                                                    | Result associated with the request. Condition: Only returned when the request fails. No content returned for success (HTTP status code 204).                                                                                    |

### Sample API Usage

#### Request URL

https://api-checkfreenext-cert.fiservapps.com/api/v1/me/automaticTransactions/e149c86d52554704b1d000fc4141ae08?
cancelPendingTransactions=true

#### Response

| 204 No Content |
|----------------|

# 

# Complex Objects

#### AccountToken

| Parameter | Req | Data Type                   | Description               |
|-----------|-----|-----------------------------|---------------------------|
| account   | Req | [BankAccount](#bankaccount) | Bank account information. |

#### AccountTokenAddInfo

Reserved for future use.

| Parameter | Req | Data Type                                 | Description               |
|------------|----|-------|-------------------------------------------------|
| account   | Req | [BankAccountAddInfo](#bankaccountaddinfo) | Bank account information. |

#### Address

| Parameter   | Req | Data Type               | Description                             |
|------------|-----|----------|-----------------------------------------------|
| addressType | Req | string                  | Type of address. Valid value: USAddress |
| usAddress   | Req | [USAddress](#usaddress) | USAddress is required.                  |

#### AutomaticTransactionOptions

<table>
<colgroup>
<col style="width: 26%" />
<col style="width: 5%" />
<col style="width: 26%" />
<col style="width: 40%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>billTransactionScheduleActive</td>
<td>Req</td>
<td>boolean</td>
<td>Indicates if a bill transaction schedule is already active (i.e.,
eBill AutoPay is active) for the payee.</td>
</tr>
<tr class="even">
<td>billTransactionScheduleCapable</td>
<td>Req</td>
<td>boolean</td>
<td><p>Indicates when a payee is capable of an automatic transaction
schedule for paying ebills (eBill AutoPay). This is true when:</p>
<ul>
<li><p>Payee is active.</p></li>
<li><p>Payee is e-bill enabled.</p></li>
<li><p>Biller supports AutoPay.</p></li>
<li><p>The first e-bill has been received.</p></li>
</ul></td>
</tr>
<tr class="odd">
<td>billTransactionScheduleTypesSupported</td>
<td>Cond</td>
<td><a
href="#billtransactionscheduletypessupported">BillTransactionScheduleTypesSupported</a></td>
<td><p>Condition: Required when billTransactionScheduleCapable is
true.</p>
<p>Lists the allowable payment schedule types for bill transaction
schedule (eBill Autopay).</p></td>
</tr>
<tr class="even">
<td>recurringTransactionScheduleActive</td>
<td>Req</td>
<td>boolean</td>
<td>Indicates if a recurring transaction schedule (one or more) is
already set up for the payee.</td>
</tr>
<tr class="odd">
<td>recurringTransactionScheduleCapable</td>
<td>Req</td>
<td>boolean</td>
<td>Indicates when a payee is capable of an automatic transaction
schedule as a recurring model. This is true when the payee is
active.</td>
</tr>
<tr class="even">
<td>recurringTransactionScheduleTypesSupported</td>
<td>Cond</td>
<td><a
href="#recurringtransactionscheduletypessupported">RecurringTransactionScheduleTypesSupported</a></td>
<td><p>Condition: Required when recurringTransactionScheduleCapable is
true.</p>
<p>Lists the allowable payment schedule types for a recurring
transaction schedule.</p></td>
</tr>
</tbody>
</table>

#### AutomaticTransactionOutputDetail

<table>
<colgroup>
<col style="width: 18%" />
<col style="width: 7%" />
<col style="width: 15%" />
<col style="width: 58%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>modifiableFields</td>
<td>Req</td>
<td>Array of string</td>
<td>List of fields that can be changed for the automatic
transaction.</td>
</tr>
<tr class="even">
<td>id</td>
<td>Req</td>
<td>string</td>
<td>The identifier of this automatic transaction.</td>
</tr>
<tr class="odd">
<td>self</td>
<td>Req</td>
<td>string</td>
<td>Relative URI pointing to the automatic transaction itself.</td>
</tr>
<tr class="even">
<td>status</td>
<td>Req</td>
<td>string</td>
<td><p>The status of the recurring or automatic payment model. Valid
values:</p>
<p>Active<br />
Canceled<br />
Completed<br />
Pending</p>
<p>Active and Canceled apply to both recurring models and e-bill
automatic payments.</p>
<p>Completed applies to recurring models only.</p>
<p>Pending applies only to e-bill automatic payments where e-bill
service activation is awaiting biller confirmation.</p></td>
</tr>
<tr class="odd">
<td>fundingAccountUri</td>
<td>Req</td>
<td>string</td>
<td>The source funding account URI for the automatic transaction.</td>
</tr>
<tr class="even">
<td>destinationUri</td>
<td>Req</td>
<td>string</td>
<td>The destination of the automatic transaction. Can be either a bill
or a payee.</td>
</tr>
<tr class="odd">
<td>billTransactionSchedule</td>
<td>Cond</td>
<td><a href="#billtransactionschedule">BillTransactionSchedule</a></td>
<td><p>Returned if available.</p>
<p>BillTransactionSchedule defines a transaction schedule for an e-bill
automatic payment.</p></td>
</tr>
<tr class="even">
<td>recurringTransactionSchedule</td>
<td>Cond</td>
<td><a href="#recurringtransactionschedule">RecurringTransaction<br />
Schedule</a></td>
<td><p>Returned if available.</p>
<p>RecurringTransactionSchedule defines a transaction schedule for a
payee.</p></td>
</tr>
</tbody>
</table>

#### BankAccount

<table>
<colgroup>
<col style="width: 15%" />
<col style="width: 7%" />
<col style="width: 10%" />
<col style="width: 66%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>accountType</td>
<td>Req</td>
<td>string</td>
<td>The type of bank account. Valid values: "Checking", "Savings",
"InstallmentLoan", "IndividualRetirement", "CommercialLoan",
"MoneyMarket", "LineOfCredit", "Brokerage", "SpecialDeposit"</td>
</tr>
<tr class="even">
<td>maskedAccountNumber</td>
<td>Req</td>
<td>string</td>
<td>The account number in masked form, for presentment in a user
interface. For example, “***********4656”</td>
</tr>
<tr class="odd">
<td>nickname</td>
<td>Opt</td>
<td>string</td>
<td><p>The nickname for the account, if one exists. Otherwise, returns
null.</p>
<p>Pattern: ^[a-zA-Z0-9_(){}&amp;@!+#.'$, %*-]{1,30}$</p></td>
</tr>
<tr class="even">
<td>accountBalance</td>
<td>Opt</td>
<td>double</td>
<td>The account balance.</td>
</tr>
<tr class="odd">
<td>isPreferred</td>
<td>Req</td>
<td>boolean</td>
<td>Indicates if the account is the preferred account. The preferred
account flag indicates the consumer’s preferred choice of bank account
used to fund bill payment transactions.</td>
</tr>
<tr class="even">
<td>unmaskedAccount<br />
NumberUri</td>
<td>Req</td>
<td>string</td>
<td>Link to the unmasked account number for the account.</td>
</tr>
<tr class="odd">
<td>routingTransitNumber</td>
<td>Req</td>
<td>string</td>
<td><p>Routing and transit number for the consumer's bank account.</p>
<p>Pattern: ^[0-9]{9}$</p></td>
</tr>
<tr class="even">
<td>isBusiness</td>
<td>Req</td>
<td>boolean</td>
<td>Indicates if this is a business account. True if it is a business
account.</td>
</tr>
<tr class="odd">
<td>businessName</td>
<td>Cond</td>
<td>string</td>
<td><p>When isBusiness is true, this is the name of the business.</p>
<p>Pattern: ^[a-zA-Z0-9_(){}&amp;@!+#.'$,%^ *-]*</p></td>
</tr>
<tr class="even">
<td>primaryAccountOwner</td>
<td>Opt</td>
<td>string</td>
<td><p>Name of the primary account holder for this account. Used only
for individual consumers.</p>
<p>Pattern: ^[a-zA-Z0-9_(){}&amp;@!+#.'$,%^ *-]*</p></td>
</tr>
<tr class="odd">
<td>secondaryAccount<br />
Owner</td>
<td>Opt</td>
<td>string</td>
<td><p>Name of the secondary account holder, if applicable. This field
allows a consumer to add an additional name on a printed check that
appears on the line beneath the name in the PrimaryAccountOwner field.
Used only for individual consumers.</p>
<p>Pattern: ^[a-zA-Z0-9_(){}&amp;@!+#.'$,%^ *-]*</p></td>
</tr>
<tr class="even">
<td>confirmationStatus</td>
<td>Opt</td>
<td>string</td>
<td>Account’s verification/confirmation status. Valid values: Confirmed,
Failed, Pending, InProgress</td>
</tr>
<tr class="odd">
<td>accountStatus</td>
<td>Opt</td>
<td>string</td>
<td>Account status. Valid values: Active, Inactive, Pending,
Rejected</td>
</tr>
<tr class="even">
<td>checkPrintAddress</td>
<td>Opt</td>
<td><a href="#usaddress">USAddress</a></td>
<td>The U.S. address printed on the checks. If provided, this is the
consumer’s address to be printed in the upper left corner of the
check.</td>
</tr>
<tr class="odd">
<td>bankingOptions</td>
<td>Opt</td>
<td><a href="#bankingoptions">BankingOptions</a></td>
<td>Banking options when banking service is enabled.</td>
</tr>
</tbody>
</table>

#### BankAccountAddInfo

<table>
<colgroup>
<col style="width: 14%" />
<col style="width: 4%" />
<col style="width: 9%" />
<col style="width: 71%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>routingTransitNumber</td>
<td>Req</td>
<td>string</td>
<td><p>Routing and transit number for the user’s bank account. Length:
9</p>
<p>Pattern: ^[0-9]{9}$</p>
<p>Must contain a valid number in the format 999999999.</p></td>
</tr>
<tr class="even">
<td>accountType</td>
<td>Req</td>
<td>string</td>
<td>The type of bank account. Valid values: "Checking", "Savings",
"InstallmentLoan", "IndividualRetirement", "CommercialLoan",
"MoneyMarket", "LineOfCredit", "Brokerage", "SpecialDeposit"</td>
</tr>
<tr class="odd">
<td>accountNumber</td>
<td>Req</td>
<td>string</td>
<td><p>User’s bank account number. Account number as it appears on the
MICR line of the user’s paper check. This number is used by the user as
the bank account number, and it cannot be changed. Length: 1–22</p>
<p>Pattern: ^[a-zA-Z0-9]{1,22}$</p>
<p>Must contain at least one character. Must contain uppercase
characters when alphabetic characters are part of the account
number.</p></td>
</tr>
</tbody>
</table>

#### BankAccountGetModel

<table>
<colgroup>
<col style="width: 14%" />
<col style="width: 7%" />
<col style="width: 10%" />
<col style="width: 67%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>self</td>
<td>Req</td>
<td>string</td>
<td>Relative URI pointing to the object itself.</td>
</tr>
<tr class="even">
<td>id</td>
<td>Req</td>
<td>string</td>
<td>Unique identifier of the object.</td>
</tr>
<tr class="odd">
<td>modifiableFields</td>
<td>Req</td>
<td>Array of string</td>
<td>List of fields that can be changed for the bank account.</td>
</tr>
<tr class="even">
<td>accountType</td>
<td>Req</td>
<td>string</td>
<td>The type of bank account. Valid values: "Checking", "Savings",
"InstallmentLoan", "IndividualRetirement", "CommercialLoan",
"MoneyMarket", "LineOfCredit", "Brokerage", "SpecialDeposit"</td>
</tr>
<tr class="odd">
<td>maskedAccountNumber</td>
<td>Req</td>
<td>string</td>
<td>The account number in masked form, for presentment in a user
interface. For example, “***********4656”</td>
</tr>
<tr class="even">
<td>nickname</td>
<td>Opt</td>
<td>string</td>
<td><p>The nickname for the account, if one exists. Otherwise, returns
null.</p>
<p>Pattern: ^[a-zA-Z0-9_(){}&amp;@!+#.'$, %*-]{1,30}$</p></td>
</tr>
<tr class="odd">
<td>accountBalance</td>
<td>Opt</td>
<td>double</td>
<td>The account balance.</td>
</tr>
<tr class="even">
<td>isPreferred</td>
<td>Req</td>
<td>boolean</td>
<td>Indicates if the account is the preferred account. The preferred
account flag indicates the consumer’s preferred choice of bank account
used to fund bill payment transactions.</td>
</tr>
<tr class="odd">
<td>unmaskedAccount<br />
NumberUri</td>
<td>Req</td>
<td>string</td>
<td>Link to the unmasked account number for the account. See “<a
href="#get-an-unmasked-bank-account-number">Get an Unmasked Bank Account
Number.</a>”</td>
</tr>
<tr class="even">
<td>routingTransitNumber</td>
<td>Req</td>
<td>string</td>
<td><p>Routing and transit number for the bank account. Length: 9</p>
<p>Pattern: ^[0-9]{9}$</p></td>
</tr>
<tr class="odd">
<td>isBusiness</td>
<td>Req</td>
<td>boolean</td>
<td>Indicates if the account is a business account. True if it is a
business account.</td>
</tr>
<tr class="even">
<td>businessName</td>
<td>Cond</td>
<td>string</td>
<td><p>When isBusiness is true, this is the name of the business.</p>
<p>Pattern: ^[a-zA-Z0-9_(){}&amp;@!+#.'$,%^ *-]*</p></td>
</tr>
<tr class="odd">
<td>primaryAccountOwner</td>
<td>Opt</td>
<td>string</td>
<td><p>Name of the primary owner of this account.</p>
<p>Pattern: ^[a-zA-Z0-9_(){}&amp;@!+#.'$,%^ *-]*</p></td>
</tr>
<tr class="even">
<td>secondaryAccount<br />
Owner</td>
<td>Opt</td>
<td>string</td>
<td><p>Name of the secondary owner of this account, if applicable.</p>
<p>Pattern: ^[a-zA-Z0-9_(){}&amp;@!+#.'$,%^ *-]*</p></td>
</tr>
<tr class="odd">
<td>confirmationStatus</td>
<td>Opt</td>
<td>string</td>
<td>Account’s verification/confirmation status. Valid values: Confirmed,
Failed, Pending, InProgress</td>
</tr>
<tr class="even">
<td>accountStatus</td>
<td>Opt</td>
<td>string</td>
<td>Account status. Valid values: Active, Inactive, Pending,
Rejected</td>
</tr>
<tr class="odd">
<td>checkPrintAddress</td>
<td>Opt</td>
<td><a href="#usaddress">USAddress</a></td>
<td>The U.S. address printed on the checks. If provided, this is the
consumer’s address to be printed in the upper left corner of the
check.</td>
</tr>
<tr class="even">
<td>bankingOptions</td>
<td>Opt</td>
<td><a href="#bankingoptions">BankingOptions</a></td>
<td>Banking options when banking service is enabled.</td>
</tr>
</tbody>
</table>

#### BankingOptions

| Parameter                     | Req | Data Type | Description                                                                  |
|--------------|----|-------|-----------------------------------------------|
| isDownloadTransactionsEnabled | Req | boolean   | True if the consumer can download transactions related to this bank account. |
| isSourceTransferAccount       | Req | boolean   | True if the consumer can transfer funds from this account.                   |
| isDestinationTransferAccount  | Req | boolean   | True if the consumer can transfer funds into this account.                   |

#### BaseModel

| Parameter | Req  | Data Type | Description                                                                            |
|------------|-----|-------|-------------------------------------------------|
| self      | Cond | string    | URI pointing to the object itself. Condition: Always returned for successful response. |
| id        | Cond | string    | Unique identifier for the object. Condition: Always returned for successful response.  |

#### Bill

<table>
<colgroup>
<col style="width: 23%" />
<col style="width: 4%" />
<col style="width: 14%" />
<col style="width: 57%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>amountDue</td>
<td>Req</td>
<td>double</td>
<td>The current amount that the consumer owes to the biller.</td>
</tr>
<tr class="even">
<td>balanceAmountDue</td>
<td>Opt</td>
<td>double</td>
<td>The total outstanding balance for the consumer’s account with the
biller.</td>
</tr>
<tr class="odd">
<td>billerInfo</td>
<td>Req</td>
<td><a href="#billerinfo">BillerInfo</a></td>
<td>Details about the biller.</td>
</tr>
<tr class="even">
<td>billType</td>
<td>Req</td>
<td>string</td>
<td><p>Type of bill.</p>
<p>FromBiller indicates a distributed e-bill.</p>
<p>FromBillDueAlert indicates a bill due alert.</p></td>
</tr>
<tr class="odd">
<td>dateFirstViewed</td>
<td>Cond</td>
<td>string</td>
<td>Date that the e-bill was first viewed. Condition: The e-bill has
been viewed. Format: yyyy-MM-dd.</td>
</tr>
<tr class="even">
<td>destinationUrl</td>
<td>Req</td>
<td>string</td>
<td>The destination URL of the payee.</td>
</tr>
<tr class="odd">
<td>dueDate</td>
<td>Req</td>
<td>string</td>
<td>The date that the biller has indicated that the bill is due. If this
date is displayed to the consumer on a UI, the date should be derived
using eastern time (ET). Format yyyy-MM-dd.</td>
</tr>
<tr class="even">
<td>actionByDate</td>
<td>Opt</td>
<td>string</td>
<td>The latest date and time that a transaction must be initiated by in
order to be received by the biller on the dueDate. Not returned if the
calculated date/time is earlier than the current date/time.</td>
</tr>
<tr class="odd">
<td>dueDateText</td>
<td>Opt</td>
<td>string</td>
<td>Text that describes the biller’s preferred method of expressing the
date that payment is due, such as “Due Upon Receipt.” Maximum length:
20</td>
</tr>
<tr class="even">
<td>detailUrl</td>
<td>Req</td>
<td>string</td>
<td>Bill detail URL. The bill detail URL expires in a timeframe set by
the biller. The default time is 30 minutes but may be as low as 10
minutes. Long-term storage of the URL is not possible because it is only
valid for 30 minutes (default) because of encrypted tokens.</td>
</tr>
<tr class="odd">
<td>filedBillInfo</td>
<td>Opt</td>
<td><a href="#filedbillinfo">FiledBillInfo</a></td>
<td>Filed bill details. Only provided if e-bill has been filed.</td>
</tr>
<tr class="even">
<td>isDetailUrlIframeSupported</td>
<td>Req</td>
<td>boolean</td>
<td>Indicates whether the URL can be displayed in an iFrame.</td>
</tr>
<tr class="odd">
<td>minimumAmountDue</td>
<td>Opt</td>
<td>double</td>
<td>The minimum amount that the consumer owes to the biller.</td>
</tr>
<tr class="even">
<td>replacedBillId</td>
<td>Cond</td>
<td>string</td>
<td>The identifier for a replacement e-bill. Condition: The e-bill is a
replacement bill.</td>
</tr>
<tr class="odd">
<td>status</td>
<td>Req</td>
<td>string</td>
<td><p>The current state of the e-bill, such as whether it is paid or
unpaid. Valid values:</p>
<p>Paid<br />
Unpaid<br />
PaymentFailed<br />
PaymentCanceled<br />
Filed</p></td>
</tr>
<tr class="even">
<td>useDueDateText</td>
<td>Req</td>
<td>boolean</td>
<td><p>Specifies whether to use the text provided in dueDateText. Valid
values:</p>
<p>true (use the text)</p>
<p>false (do not use the text)</p></td>
</tr>
<tr class="odd">
<td>self</td>
<td>Req</td>
<td>string</td>
<td>URI pointing to the e-bill itself.</td>
</tr>
<tr class="even">
<td>id</td>
<td>Req</td>
<td>string</td>
<td>Unique identifier for the e-bill.</td>
</tr>
</tbody>
</table>

#### BillDiscoveryUserEligibility

<table>
<colgroup>
<col style="width: 15%" />
<col style="width: 4%" />
<col style="width: 8%" />
<col style="width: 70%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>creditReportEligible</td>
<td>Req</td>
<td>boolean</td>
<td>Indicates if the consumer is eligible for a credit report
search.</td>
</tr>
<tr class="even">
<td>billServiceProviderEligible</td>
<td>Req</td>
<td>boolean</td>
<td>Indicates if the consumer is eligible for a bill service provider
search.</td>
</tr>
<tr class="odd">
<td>outstandingPotential<br />
PayeesCount</td>
<td>Req</td>
<td>integer</td>
<td>Count of outstanding potential payees (i.e., potential payees that
have already been identified and for which no action has been taken by
the consumer).</td>
</tr>
</tbody>
</table>

#### BillTransactionSchedule

<table>
<colgroup>
<col style="width: 23%" />
<col style="width: 4%" />
<col style="width: 14%" />
<col style="width: 57%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>amount</td>
<td>Cond</td>
<td>double</td>
<td><p>The transaction amount. Required if the transactionType is
FixedAmount.</p>
<p>Pattern: ^\d+(\.\d{1,2})?$</p></td>
</tr>
<tr class="even">
<td>maximumTransactionAmount</td>
<td>Opt</td>
<td>double</td>
<td><p>The upper limit of the transaction amount. If the delivered bill
amount is greater than this value, then the transaction amount will
equal the maximumTransactionAmount value. Only applies to
TransactionType “AmountDue”, “MinimumAmountDue” or “AccountBalance”.</p>
<p>Pattern: ^\d+(\.\d{1,2})?$</p></td>
</tr>
<tr class="odd">
<td>scheduleDaysBefore</td>
<td>Cond</td>
<td>integer</td>
<td><p>Condition: Required if transactionInitiationType is
DaysBeforeDueDate.</p>
<p>If present, will generate transactions the specified number of
business days prior to the normally scheduled date.</p>
<p>Valid values: 1, 2, 3, 4, 5</p></td>
</tr>
<tr class="even">
<td>transactionInitiationType</td>
<td>Req</td>
<td>string</td>
<td>Specifies when a transaction should be generated based on bill
information. Valid values:<br />
DueDate<br />
UponReceipt<br />
DaysBeforeDueDate</td>
</tr>
<tr class="odd">
<td>transactionType</td>
<td>Req</td>
<td>string</td>
<td>The payment structure for the automatic transaction. Valid
values:<br />
FixedAmount<br />
AmountDue<br />
MinimumAmountDue<br />
AccountBalance</td>
</tr>
<tr class="even">
<td>transactionScheduledAlert</td>
<td>Req</td>
<td>boolean</td>
<td><p>Indicates if the consumer is notified when a transaction is
scheduled.</p>
<p>True - Notify the consumer when a transaction is scheduled.</p>
<p>False - Do not notify the consumer when a transaction is scheduled.
This is the default.</p></td>
</tr>
<tr class="odd">
<td>transactionSentAlert</td>
<td>Req</td>
<td>boolean</td>
<td><p>Indicates if the consumer is notified when a transaction is
sent.</p>
<p>True - Notify the consumer when a transaction is processed.</p>
<p>False - Do not notify the consumer when a transaction is processed.
This is the default.</p></td>
</tr>
</tbody>
</table>

#### BillTransactionSchedulePatch

<table>
<colgroup>
<col style="width: 23%" />
<col style="width: 4%" />
<col style="width: 14%" />
<col style="width: 57%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>amount</td>
<td>Cond</td>
<td>double</td>
<td><p>The transaction amount. Required if the transactionType is
FixedAmount.</p>
<p>Pattern: ^\d+(\.\d{1,2})?$</p></td>
</tr>
<tr class="even">
<td>maximumTransactionAmount</td>
<td>Opt</td>
<td>double</td>
<td><p>The upper limit of the transaction amount. If the delivered bill
amount is greater than this value, then the transaction will be
scheduled at this amount. Only applies to TransactionType “AmountDue”,
“MinimumAmountDue” or “AccountBalance”.</p>
<p>Pattern: ^\d+(\.\d{1,2})?$</p></td>
</tr>
<tr class="odd">
<td>scheduleDaysBefore</td>
<td>Cond</td>
<td>integer</td>
<td><p>Condition: Required if transactionInitiationType is
DaysBeforeDueDate.</p>
<p>If present, will generate transactions the specified number of
business days prior to the normally scheduled date.</p>
<p>Valid values: 1, 2, 3, 4, 5</p></td>
</tr>
<tr class="even">
<td>transactionInitiationType</td>
<td>Opt</td>
<td>string</td>
<td>Specifies when a transaction should be generated based on bill
information. Valid values:<br />
DueDate<br />
UponReceipt<br />
DaysBeforeDueDate</td>
</tr>
<tr class="odd">
<td>transactionType</td>
<td>Opt</td>
<td>string</td>
<td>The payment structure for the automatic transaction. Valid
values:<br />
FixedAmount<br />
AmountDue<br />
MinimumAmountDue<br />
AccountBalance</td>
</tr>
<tr class="even">
<td>transactionScheduledAlert</td>
<td>Opt</td>
<td>boolean</td>
<td><p>Indicates if the consumer is notified when a transaction is
scheduled.</p>
<p>True - Notify the consumer when a transaction is scheduled.</p>
<p>False - Do not notify the consumer when a transaction is scheduled.
This is the default.</p></td>
</tr>
<tr class="odd">
<td>transactionSentAlert</td>
<td>Opt</td>
<td>boolean</td>
<td><p>Indicates if the consumer is notified when a transaction is
sent.</p>
<p>True - Notify the consumer when a transaction is processed.</p>
<p>False - Do not notify the consumer when a transaction is processed.
This is the default.</p></td>
</tr>
</tbody>
</table>

#### BillTransactionScheduleTypesSupported

<table>
<colgroup>
<col style="width: 23%" />
<col style="width: 4%" />
<col style="width: 14%" />
<col style="width: 57%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>transactionTypes</td>
<td>Req</td>
<td>Array of string</td>
<td>The payment structure for the automatic transaction. Valid
values:<br />
FixedAmount<br />
AmountDue<br />
MinimumAmountDue<br />
AccountBalance</td>
</tr>
<tr class="even">
<td>transactionInitiationTypes</td>
<td>Req</td>
<td>Array of string</td>
<td>Specifies when a transaction should be generated based on bill
information. Valid values:<br />
DueDate<br />
UponReceipt<br />
DaysBeforeDueDate</td>
</tr>
<tr class="odd">
<td>eligibleFundingAccounts</td>
<td>Req</td>
<td>Array of <a
href="#eligiblefundingaccount">EligibleFundingAccount</a></td>
<td>A list of eligible funding accounts with the available automatic
transaction options.</td>
</tr>
</tbody>
</table>

#### BillerInfo

| Parameter             | Req | Data Type | Description                                                                                                                                                                                                                                                                                                               |
|------------|-----|-------|-------------------------------------------------|
| billerLogoUrl         | Opt | string    | URL of the biller logo. The preferred logo size is 90 × 90 pixels.                                                                                                                                                                                                                                                        |
| billReferenceImageUrl | Opt | string    | URL where the bill reference image (teaser ad image) is stored. Used to display the image on the user interface. As a best practice, the image should be no larger than 80 x 24 pixels.                                                                                                                                   |
| billReferenceLinkUrl  | Opt | string    | URL for the teaser ad that the consumer may follow to receive additional information from the biller. This URL is used in conjunction with either the Bill Reference Image URL or the Bill Reference Text. The consumer can click on either the image or text to be transferred to the biller site specified by this URL. |
| billReferenceText     | Opt | string    | Text that can be used instead of the image link. Using this text may improve performance over downloading an image (teaser ad).                                                                                                                                                                                           |

#### CardAccountAddLimits

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 3%" />
<col style="width: 8%" />
<col style="width: 66%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>rollingPeriodDaysLimit</td>
<td>Req</td>
<td>integer</td>
<td>Number of days for which to enforce a rolling time period
limit.</td>
</tr>
<tr class="even">
<td>rollingPeriodDaysLimitModifiable</td>
<td>Req</td>
<td>boolean</td>
<td><p>Indicates if the number of days for the rolling time period can
be changed.</p>
<p>true – Can be changed.<br />
false – Cannot be changed.</p></td>
</tr>
<tr class="odd">
<td>rollingPeriodCardsLimit</td>
<td>Req</td>
<td>integer</td>
<td>Maximum number of card accounts that can be added during the rolling
time period.</td>
</tr>
<tr class="even">
<td>rollingPeriodCardsRemainingCount</td>
<td>Req</td>
<td>integer</td>
<td>Remaining number of card accounts that can be added by the consumer
during the current rolling time period.</td>
</tr>
<tr class="odd">
<td>rollingPeriodCardsLimitModifiable</td>
<td>Req</td>
<td>boolean</td>
<td><p>Indicates if the number of card accounts that can be added during
the rolling time period can be changed.</p>
<p>true – Can be changed.<br />
false – Cannot be changed.</p></td>
</tr>
<tr class="even">
<td>sameCardDeleteAddLimit</td>
<td>Req</td>
<td>integer</td>
<td>Number of times a consumer can add the same card account (delete/add
again).</td>
</tr>
<tr class="odd">
<td>sameCardDeleteAddLimitModifiable</td>
<td>Req</td>
<td>boolean</td>
<td><p>Indicates if the number of times a consumer can add the same card
account can be changed.</p>
<p>true – Can be changed.<br />
false – Cannot be changed.</p></td>
</tr>
<tr class="even">
<td>activeCardLimit</td>
<td>Req</td>
<td>integer</td>
<td>Maximum number of active card accounts.</td>
</tr>
<tr class="odd">
<td>activeCardRemainingCount</td>
<td>Req</td>
<td>integer</td>
<td>Remaining number of active card accounts that can be added by the
consumer.</td>
</tr>
<tr class="even">
<td>activeCardLimitModifiable</td>
<td>Req</td>
<td>boolean</td>
<td><p>Indicates if the number of active card accounts that can be added
by the consumer can be changed.</p>
<p>true – Can be changed.<br />
false – Cannot be changed.</p></td>
</tr>
</tbody>
</table>

#### CardAccountGetModel

<table>
<colgroup>
<col style="width: 16%" />
<col style="width: 3%" />
<col style="width: 8%" />
<col style="width: 71%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>self</td>
<td>Req</td>
<td>string</td>
<td>Relative URI pointing to the object itself.</td>
</tr>
<tr class="even">
<td>id</td>
<td>Req</td>
<td>string</td>
<td>Unique identifier of the object.</td>
</tr>
<tr class="odd">
<td>addedBy</td>
<td>Opt</td>
<td>string</td>
<td><p>Indicates the entity that added the card account. Valid
values:</p>
<p>Sponsor – Indicates the card was added by the FI</p>
<p>Subscriber – Indicates the card was added by the consumer</p></td>
</tr>
<tr class="even">
<td>cardNumberMasked</td>
<td>Req</td>
<td>string</td>
<td>The card account number in masked form. For example,
"********1234".</td>
</tr>
<tr class="odd">
<td>modifiableFields</td>
<td>Req</td>
<td>Array of string</td>
<td>List of fields that can be changed for the card.</td>
</tr>
<tr class="even">
<td>deleteAllowed</td>
<td>Req</td>
<td>boolean</td>
<td>Indicates whether the originator of this GET call is allowed to
delete this card.</td>
</tr>
<tr class="odd">
<td>cardType</td>
<td>Req</td>
<td>string</td>
<td>Indicates the type of card. Valid values: AmericanExpress, Discover,
MasterCard, Visa</td>
</tr>
<tr class="even">
<td>isDebit</td>
<td>Req</td>
<td>boolean</td>
<td>Indicates if the card is a debit card.</td>
</tr>
<tr class="odd">
<td>billingAddress</td>
<td>Req</td>
<td><a href="#usaddress">USAddress</a></td>
<td>The billing address for the card.</td>
</tr>
<tr class="even">
<td>expirationMonth</td>
<td>Req</td>
<td>integer</td>
<td>Month in which the card expires.</td>
</tr>
<tr class="odd">
<td>expirationYear</td>
<td>Req</td>
<td>integer</td>
<td>Year in which the card expires.</td>
</tr>
<tr class="even">
<td>externalAccountDescription</td>
<td>Req</td>
<td>string</td>
<td>External account description that is free-form text, such as: “Bank
Name” Visa</td>
</tr>
<tr class="odd">
<td>nameOnCard</td>
<td>Req</td>
<td>string</td>
<td>Name printed on the card.</td>
</tr>
<tr class="even">
<td>nickname</td>
<td>Opt</td>
<td>string</td>
<td>A description of the card used to help identify it in a list.</td>
</tr>
</tbody>
</table>

#### CardAccountPostResponse

| Parameter | Req | Data Type | Description                        |
|-----------|-----|-----------|------------------------------------|
| self      | Req | string    | URI pointing to the object itself. |
| id        | Req | string    | Unique identifier for the object.  |

#### CardFundingOptions

| Parameter                  | Req | Data Type | Description                                                                              |
|------------|-----|--------|------------------------------------------------|
| sponsorFirstPartyDebit     | Req | boolean   | If true, specifies that the sponsor can add first-party debit cards.                     |
| sponsorFirstPartyCredit    | Req | boolean   | If true, specifies that the sponsor can add first-party credit cards.                    |
| subscriberFirstPartyDebit  | Req | boolean   | If true, specifies that the sponsor allows the consumer to add first-party debit cards.  |
| subscriberFirstPartyCredit | Req | boolean   | If true, specifies that the sponsor allows the consumer to add first-party credit cards. |
| subscriberThirdPartyDebit  | Req | boolean   | If true, specifies that the sponsor allows the consumer to add third-party debit cards.  |

#### Consent

| Parameter    | Req | Data Type | Description                                                    |
|------------|-----|-------|-------------------------------------------------|
| consentGiven | Req | boolean   | Indicates whether consent was granted.                         |
| consentText  | Req | string    | Consent text that was presented to the consumer. Min length: 1 |
| consentType  | Req | string    | The type of consent. Valid values: BillDiscovery, CreditBureau |

#### ContactEndpoint

<table>
<colgroup>
<col style="width: 15%" />
<col style="width: 8%" />
<col style="width: 9%" />
<col style="width: 66%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>status</td>
<td>Req</td>
<td>string</td>
<td><p>The status of the contact end point.</p>
<p>Valid values: Unspecified, Active, Inactive, Suspended, NotFound,
Invalid, Onhold</p></td>
</tr>
<tr class="even">
<td>endPointType</td>
<td>Req</td>
<td>string</td>
<td><p>The type of contact end point.</p>
<p>Valid values: Phone, Email, Address</p></td>
</tr>
<tr class="odd">
<td>address</td>
<td>Cond</td>
<td><a href="#address">Address</a></td>
<td>The address of the consumer. Condition: A contact endpoint is
required (address, phone, or email).</td>
</tr>
<tr class="even">
<td>phone</td>
<td>Cond</td>
<td><a href="#phone">Phone</a></td>
<td>Contact phone number for the consumer. Condition: A contact endpoint
is required (address, phone, or email).</td>
</tr>
<tr class="odd">
<td>email</td>
<td>Cond</td>
<td><a href="#email">Email</a></td>
<td><p>Contract email address for the consumer. Condition: A contact
endpoint is required (address, phone, or email).</p>
<p>Length: 5–100</p>
<p>Note: If the sponsor’s Fiserv profile or the Fiserv product’s profile
requires email addresses to be collected, then an email address is
required.</p>
<p>Pattern:
^([_+A-Za-z0-9]+((\.|\-)[_+A-Za-z0-9]+)*@[A-Za-z0-9]+((\.|\-)[A-Za-z0-9]+)*(\.[A-Za-z]{2,8}))$</p>
<p>If provided, this field must contain a valid Internet-style
address.</p></td>
</tr>
<tr class="even">
<td>defaultEndpoint</td>
<td>Req</td>
<td>boolean</td>
<td>Indicates if the endpoint is considered the default for the user.
For example, may be used to indicate the primary email address for the
user to receive correspondence.</td>
</tr>
</tbody>
</table>

#### EbillAccountDetail

<table>
<colgroup>
<col style="width: 16%" />
<col style="width: 4%" />
<col style="width: 8%" />
<col style="width: 70%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>autoActivationType</td>
<td>Opt</td>
<td>string</td>
<td><p>Specifies the type of Auto Activation.</p>
<p>Valid values: EZActivation, BillerMerge, RegularBillerRules</p></td>
</tr>
<tr class="even">
<td>activationType</td>
<td>Opt</td>
<td>string</td>
<td><p>Specifies how the account was activated.</p>
<p>Valid values: AutoActivation, User</p></td>
</tr>
<tr class="odd">
<td>billerThumbnailImageUrl</td>
<td>Opt</td>
<td>string</td>
<td>URL for the biller sample bill thumbnail image.</td>
</tr>
<tr class="even">
<td>billTransactionSchedule<br />
Eligible</td>
<td>Req</td>
<td>boolean</td>
<td><p>Indicates if the account is eligible for Automatic Transactions
(Bill Transaction Schedule). To be eligible, the following conditions
must be true:</p>
<ul>
<li><p>The biller supports AutoPay.</p></li>
<li><p>The first e-bill has been received from the biller.</p></li>
<li><p>The payee is not in a trial period.</p></li>
</ul></td>
</tr>
<tr class="odd">
<td>emailAddressShare</td>
<td>Req</td>
<td>boolean</td>
<td><p>Determines whether the consumer’s email address may be sent to
the biller. Valid values:</p>
<p>true – Service holder’s email address may be sent to the biller.</p>
<p>false – Do not send the service holder’s email address to the
biller.</p></td>
</tr>
<tr class="even">
<td>firstEbillReceived</td>
<td>Req</td>
<td>boolean</td>
<td>Indicates if the first e-bill has been received.</td>
</tr>
<tr class="odd">
<td>name</td>
<td>Req</td>
<td><a href="#individualname">Individual<br />
Name</a></td>
<td>The name of the consumer associated with the e-bill activation
request. May be different from the consumer name. If not provided, the
consumer name will be used.</td>
</tr>
<tr class="even">
<td>isTrialPeriod</td>
<td>Req</td>
<td>boolean</td>
<td><p>Indicates if the biller has initiated a trial period. Valid
values:</p>
<p>true – Trial period initiated.</p>
<p>false – No trial period.</p>
<p>An e-bill account is identified as being in a trial period when:</p>
<ul>
<li><p>The e-bill account is active</p></li>
<li><p>The consumer should be asked to suppress paper</p></li>
<li><p>Trial period end date is valid</p></li>
</ul></td>
</tr>
<tr class="odd">
<td>paperProcessingStatus</td>
<td>Req</td>
<td>string</td>
<td><p>Specifies the state of paper bill delivery. Valid values:</p>
<ul>
<li><p>Unknown – Fiserv does not know if the biller is sending paper
bills or not.</p></li>
<li><p>PaperBillsStillSent – Fiserv knows that the biller is still
sending paper bills.</p></li>
<li><p>NoPaperBillsSent – Fiserv knows that the biller has stopped
sending paper bills.</p></li>
<li><p>EbillDeactivationAfterTrialPeriod – Due to have e-bill service
deactivated by the biller at the end of the trial period if paper
suppression is not initiated.</p></li>
<li><p>PaperBillsStopAfterTrialPeriod – Pending to have paper bills
stopped at the end of the trial period.</p></li>
</ul></td>
</tr>
<tr class="even">
<td>paperSuppression IncentiveMessage</td>
<td>Opt</td>
<td>string</td>
<td>The actual incentive message that is currently active for the
account.</td>
</tr>
<tr class="odd">
<td>paperSuppression TermsUrl</td>
<td>Opt</td>
<td>string</td>
<td>URL for the paper suppression terms.</td>
</tr>
<tr class="even">
<td>promptPaperSuppression</td>
<td>Req</td>
<td>boolean</td>
<td><p>Indicates if the consumer should be prompted to indicate if the
consumer would like to suppress paper bills. Valid values:</p>
<p>true – Prompt the consumer to indicate if the consumer would like to
suppress paper bills.</p>
<p>false – Do not prompt the consumer to indicate if the consumer would
like to suppress paper bills.</p></td>
</tr>
<tr class="odd">
<td>serviceAddress</td>
<td>Req</td>
<td><a href="#usaddress">USAddress</a></td>
<td>Address of the service holder.</td>
</tr>
<tr class="even">
<td>serviceBusinessName</td>
<td>Cond</td>
<td>string</td>
<td>Business name of the service holder. Condition: serviceHolderType is
Business.</td>
</tr>
<tr class="odd">
<td>serviceDaytimePhone Number</td>
<td>Opt</td>
<td>string</td>
<td>Phone number where the service holder can be reached during normal
business hours.</td>
</tr>
<tr class="even">
<td>serviceEveningPhone Number</td>
<td>Opt</td>
<td>string</td>
<td>Phone number where the service holder can be reached after normal
business hours.</td>
</tr>
<tr class="odd">
<td>serviceHolderType</td>
<td>Req</td>
<td>string</td>
<td><p>Identifies if the service holder is an individual or a business.
Valid values:</p>
<ul>
<li><p>Business</p></li>
<li><p>Consumer</p></li>
</ul></td>
</tr>
<tr class="even">
<td>status</td>
<td>Req</td>
<td>string</td>
<td><p>Status of the e-bill service account. Valid values:</p>
<p>Pending<br />
Active<br />
Inactive<br />
Rejected</p></td>
</tr>
<tr class="odd">
<td>trialPeriodEnd</td>
<td>Cond</td>
<td>string</td>
<td><p>Date that the trial period ends for this account. Condition:
isTrialPeriod is true.</p>
<p>Format: yyyy-MM-dd</p></td>
</tr>
<tr class="even">
<td>trialPeriodStart</td>
<td>Cond</td>
<td>string</td>
<td><p>Date that the trial period started for this account. Condition:
isTrialPeriod is true.</p>
<p>Format: yyyy-MM-dd</p></td>
</tr>
<tr class="odd">
<td>transactionScheduleTypesSupported</td>
<td>Req</td>
<td>Array of string</td>
<td><p>The payment structure for the automatic transaction. Valid
values:</p>
<p>FixedAmount<br />
AmountDue<br />
MinimumAmountDue<br />
AccountBalance</p></td>
</tr>
<tr class="even">
<td>self</td>
<td>Req</td>
<td>string</td>
<td>The URI to access the e-bill service item.</td>
</tr>
<tr class="odd">
<td>id</td>
<td>Req</td>
<td>string</td>
<td>The ID of the e-bill service item. This will always be empty
(null).</td>
</tr>
</tbody>
</table>

#### EbillCapability 

<table>
<colgroup>
<col style="width: 14%" />
<col style="width: 5%" />
<col style="width: 14%" />
<col style="width: 65%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>billerName</td>
<td>Opt</td>
<td>string</td>
<td>The Fiserv-determined value of the biller name. This may be
different from the biller name that was passed in the request.</td>
</tr>
<tr class="even">
<td>billerLogoUrl</td>
<td>Opt</td>
<td>string</td>
<td>The URL for the biller logo. The preferred logo size is 90 × 90
pixels.</td>
</tr>
<tr class="odd">
<td>billerSiteUrl</td>
<td>Opt</td>
<td>string</td>
<td>The URL for biller-provided additional information.</td>
</tr>
<tr class="even">
<td>billerThumbnailImage<br />
Url</td>
<td>Opt</td>
<td>string</td>
<td>The URL for the biller sample bill thumbnail image.</td>
</tr>
<tr class="odd">
<td>paperSuppression<br />
Options</td>
<td>Opt</td>
<td>Array of<br />
<a href="#papersuppressionoption">PaperSuppression<br />
Option</a></td>
<td>The paper suppression options that the biller allows.</td>
</tr>
<tr class="even">
<td>preAuthTokens</td>
<td>Opt</td>
<td>Array of <a href="#preauthtoken">PreAuthToken</a></td>
<td><p>List of any pre-authorization tokens that need to be provided by
the consumer to activate e-bill service.</p>
<p>The values for the tokens must be sent in the Payee E-bill Service
POST request.</p></td>
</tr>
<tr class="odd">
<td>restrictionText</td>
<td>Opt</td>
<td>string</td>
<td>Text regarding any restrictions that the biller may have.</td>
</tr>
<tr class="even">
<td>serviceAddressRequired</td>
<td>Req</td>
<td>boolean</td>
<td><p>Indicates if the service address is required for authentication
by the biller. This is the address where the biller provides services.
Often applies to utilities in which the address where services are
provided may differ from the billing address.</p>
<p>Best practice is if this flag is true, the consumer should be
presented with the opportunity to provide a service address that may be
different than their consumer profile address.</p></td>
</tr>
<tr class="odd">
<td>serviceAddress</td>
<td>Cond</td>
<td><a href="#usaddress">USAddress</a></td>
<td>Address of the service holder, pre-filled for the consumer. This
information is taken from the consumer's profile. Condition:
serviceAddressRequired is true.</td>
</tr>
<tr class="even">
<td>trialPeriodDays</td>
<td>Opt</td>
<td>integer</td>
<td>Time period of the biller’s trial period (in days). Only applicable
for those billers supporting a finite trial period. Otherwise, will be
zeros.</td>
</tr>
<tr class="odd">
<td>accountNumberEditable</td>
<td>Req</td>
<td>boolean</td>
<td><p>Indicates if the consumer can edit the account number in a
subsequent Payee E-bill Service POST request. Valid values:</p>
<p>true – The account number can be edited.</p>
<p>false – The account number cannot be edited.</p></td>
</tr>
<tr class="even">
<td>accountNumberHelp<br />
Text</td>
<td>Opt</td>
<td>string</td>
<td>If available, biller-provided help text regarding the consumer’s
account number.</td>
</tr>
<tr class="odd">
<td>self</td>
<td>Req</td>
<td>string</td>
<td>The URI to access the e-bill capability item.</td>
</tr>
<tr class="even">
<td>id</td>
<td>Req</td>
<td>string</td>
<td>The ID of the e-bill capability item.</td>
</tr>
</tbody>
</table>

#### EbillServiceActivateInput

<table>
<colgroup>
<col style="width: 14%" />
<col style="width: 4%" />
<col style="width: 14%" />
<col style="width: 66%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>acceptedTC</td>
<td>Req</td>
<td>boolean</td>
<td>Indicates if the consumer has accepted the biller’s terms and
conditions. Must be true to continue with the add function.</td>
</tr>
<tr class="even">
<td>businessName</td>
<td>Opt</td>
<td>string</td>
<td>Name of the business associated with the e-bill activation
request.</td>
</tr>
<tr class="odd">
<td>accountNumber</td>
<td>Cond</td>
<td>string</td>
<td><p>The consumer’s account number with the payee.</p>
<p>Length 1–32</p>
<p>Pattern: ^[a-zA-Z0-9]{1,32}$</p>
<p>Condition: This field will be populated if Payee E-bill Capability
GET returns true for accountNumberEditable.</p></td>
</tr>
<tr class="even">
<td>emailAddressShare</td>
<td>Opt</td>
<td>boolean</td>
<td>At the time of enrollment, the consumer is commonly presented with a
check box that asks if the consumer wants to share their address with
the biller. If the consumer opts in, the only time that Fiserv shares
the address with the biller is at the time of e-bill enrollment.</td>
</tr>
<tr class="odd">
<td>name</td>
<td>Opt</td>
<td><a href="#individualname">IndividualName</a></td>
<td>Name of the consumer associated with the e-bill activation request.
May be different from the consumer name. If not provided, the consumer
name will be used.</td>
</tr>
<tr class="even">
<td>paperSuppressionOption</td>
<td>Req</td>
<td>string</td>
<td><p>Paper suppression option selected by the consumer. Valid
values:</p>
<p>DualDelivery<br />
PaperSuppression<br />
Trial</p></td>
</tr>
<tr class="odd">
<td>preAuthTokens</td>
<td>Opt</td>
<td>Array of <a href="#preauthtokeninput">PreAuthTokenInput</a></td>
<td>List of any pre-authorization tokens that need to be provided by the
consumer to activate e-bill service.</td>
</tr>
<tr class="even">
<td>serviceAddress</td>
<td>Opt</td>
<td><a href="#usaddress">USAddress</a></td>
<td>Address of the consumer associated with the e-bill activation
request. Will default to the consumer address if not provided. However,
best practice is that if EbillCapability.serviceAddressRequired is true,
the service address should be collected from the consumer via UI
interaction.</td>
</tr>
</tbody>
</table>

#### EbillServiceActivateOutput

<table>
<colgroup>
<col style="width: 14%" />
<col style="width: 4%" />
<col style="width: 14%" />
<col style="width: 66%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>trialPeriodDays</td>
<td>Cond</td>
<td>integer</td>
<td>Duration of the trial period. Condition: In the request,
paperSuppressionOption is Trial.</td>
</tr>
<tr class="even">
<td>activationState</td>
<td>Req</td>
<td>string</td>
<td><p>Determines the state of the e-bill activation request
transaction. This is not provided if the request is not able to be
processed. ActivationState represents the different states of an e-bill
activation request transaction. The initial state is Pending. All
Pending requests are submitted to the biller for approval. The biller
will either accept or reject the e-bill activation request. E-bill
activation requests that have been accepted by the biller are set to an
Accepted state. Those that are rejected will be set to a Rejected state.
Valid values:</p>
<p>Pending<br />
Accepted<br />
Rejected<br />
NotApplicable</p>
<p>NotApplicable is returned when transaction processing failed,
stopped, or was cancelled internally, so the transaction state cannot be
updated. The calling application/UI should consider this as a failed
state.</p></td>
</tr>
<tr class="odd">
<td>activationStateMessage</td>
<td>Opt</td>
<td>string</td>
<td>Optional message that may be associated with the
activationState.</td>
</tr>
<tr class="even">
<td>self</td>
<td>Req</td>
<td>string</td>
<td>The URI to access the e-bill service item.</td>
</tr>
<tr class="odd">
<td>id</td>
<td>Req</td>
<td>string</td>
<td>The ID of the e-bill service item.</td>
</tr>
</tbody>
</table>

#### EligibleFundingAccount

| Parameter         | Req | Data Type        | Description                                                                                                            |
|-----------|-----|-----------|----------------------------------------------|
| fundingAccountUri | Req | string           | The source funding account URI for the automatic transaction.                                                          |
| maxAmount         | Req | number ($double) | Maximum amount allowed for a bill transaction schedule. Applicable only for the transaction type value of FixedAmount. |
| minAmount         | Req | number ($double) | Minimum amount allowed for a bill transaction schedule. Applicable only for the transaction type value of FixedAmount. |

#### Email

<table>
<colgroup>
<col style="width: 14%" />
<col style="width: 5%" />
<col style="width: 8%" />
<col style="width: 70%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>value</td>
<td>Req</td>
<td>string</td>
<td>Email value.<br />
Pattern:
^([_+A-Za-z0-9]+((\.|\-)[_+A-Za-z0-9]+)*@[A-Za-z0-9]+((\.|\-)[A-Za-z0-9]+)*(\.[A-Za-z]{2,8}))$</td>
</tr>
</tbody>
</table>

#### Features

<table>
<colgroup>
<col style="width: 14%" />
<col style="width: 5%" />
<col style="width: 8%" />
<col style="width: 70%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>supportsBillDiscovery</td>
<td>Req</td>
<td>boolean</td>
<td><p>Indicates if the tenant/sponsor offers Bill Discovery.</p>
<p>true – Offers Bill Discovery</p>
<p>false – Does not offer Bill Discovery</p></td>
</tr>
<tr class="even">
<td>supportsCardFunded<br />
BillPay</td>
<td>Req</td>
<td>boolean</td>
<td><p>Indicates if the tenant/sponsor offers card funded bill
payment.</p>
<p>true – Offers card funded bill payment</p>
<p>false – Does not offer card funded bill payment</p></td>
</tr>
<tr class="odd">
<td>billDiscoveryProvider<br />
Type</td>
<td>Cond</td>
<td>Array of string</td>
<td><p>Condition: Required when supportsBillDiscovery is true.<br />
Specifies how bills are discovered—via BillServiceProvider,
CreditBureau, or both.</p>
<p>Valid values: BillServiceProvider, CreditBureau</p></td>
</tr>
<tr class="even">
<td>transactionHistory<br />
Months</td>
<td>Req</td>
<td>integer</td>
<td>Specifies the total months of transaction history supported by the
tenant/sponsor (minimum is 6 months).</td>
</tr>
<tr class="odd">
<td>lddEnabled</td>
<td>Req</td>
<td>boolean</td>
<td>Indicates if LDD service is enabled/supported for the sponsor.</td>
</tr>
<tr class="even">
<td>cardFundingOptions</td>
<td>Cond</td>
<td><a href="#cardfundingoptions">CardFunding Options</a></td>
<td>Condition: Required when supportsCardFundedBillPay is true.<br />
Specifies the types of card funding supported.</td>
</tr>
<tr class="odd">
<td>supportsClassicView</td>
<td>Req</td>
<td>boolean</td>
<td><p>Indicates if the tenant/sponsor offers Classic View.</p>
<p>true – Offers Classic View</p>
<p>false – Does not offer Classic View</p></td>
</tr>
</tbody>
</table>

#### FiledBillInfo

<table>
<colgroup>
<col style="width: 14%" />
<col style="width: 5%" />
<col style="width: 8%" />
<col style="width: 70%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>reason</td>
<td>Req</td>
<td>string</td>
<td><p>Indicates the method of the bill’s resolution. Valid values:</p>
<p>NoneSpecified, Bank, Check, Cash, NotPaid, Other, BillerWebSite,
Phone, Mail, Office, ZeroBalanceBill, ContestedBill</p></td>
</tr>
<tr class="even">
<td>note</td>
<td>Opt</td>
<td>string</td>
<td>Optional note entered by the consumer with information about the
bill and its resolution. Length 1-80</td>
</tr>
</tbody>
</table>

#### FinancialInstitutionModel

| Parameter                | Req | Data Type | Description                 |
|--------------------------|-----|-----------|-----------------------------|
| financialInstitutionName | Req | string    | Financial institution name. |

#### IdentityValidationInformation

<table>
<colgroup>
<col style="width: 14%" />
<col style="width: 5%" />
<col style="width: 8%" />
<col style="width: 70%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>idType</td>
<td>Req</td>
<td>string</td>
<td>Type of identification. Valid values: “StateIssuedDriverLicense”,
“StateIssuedIdCard”, “Passport”, “MilitaryIdCard”</td>
</tr>
<tr class="even">
<td>idNumber</td>
<td>Req</td>
<td>string</td>
<td><p>Identification number on ID. Length: 6-30</p>
<p>Pattern: ^[a-zA-Z0-9_(){}&amp;@!+#.'$,%^ *-]*</p>
<p>Cannot contain all zeroes</p>
<p>Cannot contain all repeating number</p></td>
</tr>
<tr class="odd">
<td>idIssuingState</td>
<td>Cond</td>
<td>string</td>
<td><p>The state that issued identification; must be a valid US state.
Length: 2</p>
<p>Pattern: ^[A-Z]*</p>
<p>Condition: Required when idType = “StateIssuedDriverLicense” or
“StateIssuedIdCard”.</p></td>
</tr>
<tr class="even">
<td>idIssuingCountry</td>
<td>Req</td>
<td>string</td>
<td><p>The country that issued identification; must be a valid country
code (ISO 3166-1 alpha-3 codes). Length: 3</p>
<p>Pattern: ^[a-zA-Z0-9_(){}&amp;@!+#.'$,%^ *-]*</p></td>
</tr>
</tbody>
</table>

#### IndividualName

<table>
<colgroup>
<col style="width: 14%" />
<col style="width: 5%" />
<col style="width: 8%" />
<col style="width: 70%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>first</td>
<td>Req</td>
<td>string</td>
<td><p>First name of the consumer.</p>
<p>Length: 1–32</p>
<p>Pattern: ^[a-zA-Z][a-zA-Z0-9_(){}&amp;@!+#.$,%*-]*</p></td>
</tr>
<tr class="even">
<td>last</td>
<td>Req</td>
<td>string</td>
<td><p>Last name of the consumer.</p>
<p>Length: 1–32</p>
<p>Pattern: ^[a-zA-Z][a-zA-Z0-9_(){}&amp;@!+#. $,%*-]*</p>
<p>Last name is required, regardless of consumer type.</p>
<p>If this is an account for an individual, this is the individual
consumer’s last name. If this is an account for a business, this is the
last name of a principal owner of the business or a person to
contact.</p></td>
</tr>
<tr class="odd">
<td>middle</td>
<td>Opt</td>
<td>string</td>
<td><p>Middle name of the consumer.</p>
<p>Length: 0–32</p>
<p>Pattern: ^[a-zA-Z][a-zA-Z0-9_(){}&amp;@!+#.$,%*-]*</p></td>
</tr>
<tr class="even">
<td>nickname</td>
<td>Opt</td>
<td>string</td>
<td><p>Short name used to refer to the consumer.</p>
<p>For example, BOB is a nickname for ROBERT.</p>
<p>Length: 1–30</p>
<p>Pattern: ^[a-zA-Z0-9_(){}&amp;@!+#.'$, %*-]{1,30}$</p>
<p>When provided, at least one character is required.</p></td>
</tr>
<tr class="odd">
<td>prefix</td>
<td>Opt</td>
<td>string</td>
<td><p>A title relating to the consumer, such as DR., MR., or MS.</p>
<p>Length: 1–6</p>
<p>Pattern: ^[a-zA-Z][a-zA-Z0-9_(){}&amp;@!+#.$,%*-]*</p></td>
</tr>
</tbody>
</table>

#### Merchant

| Parameter           | Req | Data Type | Description                                                                                                                  |
|-----------|-----|-------|-------------------------------------------------|
| logoUri             | Opt | string    | URI for the merchant logo. The preferred logo size is 90 × 90 pixels.                                                        |
| merchantZipRequired | Req | boolean   | Flag to indicate whether the merchant ZIP Code is required when adding as a payee to obtain correct payee merchant matching. |
| name                | Req | string    | Name of the merchant. The response returns a maximum of 32 characters.                                                       |
| self                | Req | string    | URI for the merchant.                                                                                                        |
| id                  | Req | string    | Identifier for the merchant.                                                                                                 |

#### MerchantData

| Parameter        | Req | Data Type | Description                                                   |
|------------|-----|-------|-------------------------------------------------|
| merchantName     | Req | string    | Name of the merchant. Length: 2–32                            |
| merchantLogoUrl  | Opt | string    | Logo for merchant. The preferred logo size is 90 × 90 pixels. |
| merchantUri      | Req | string    | URI pointing to the merchant information.                     |
| legacyMerchantId | Opt | string    | Reserved for future use.                                      |

#### MerchantDetail

| Parameter        | Req | Data Type                            | Description                                           |
|------------|-----|-------|-------------------------------------------------|
| displayPayeeName | Req | string                               | Payee name displayed beneath a logo before selection. |
| logoUri          | Opt | string                               | URL for the payee logo.                               |
| payeeList        | Opt | Array of [PayeeDetail](#payeedetail) |                                                       |

#### Name

<table>
<colgroup>
<col style="width: 14%" />
<col style="width: 5%" />
<col style="width: 8%" />
<col style="width: 70%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>firstName</td>
<td>Req</td>
<td>string</td>
<td><p>First name of the consumer. Length: 1–32</p>
<p>Pattern: ^[a-zA-Z][a-zA-Z0-9_(){}&amp;@!+#.$,%*-]*</p></td>
</tr>
<tr class="even">
<td>lastName</td>
<td>Req</td>
<td>string</td>
<td><p>Last name of the consumer. Length: 1–32</p>
<p>Pattern: ^[a-zA-Z][a-zA-Z0-9_(){}&amp;@!+#. $,%*-]*</p></td>
</tr>
<tr class="odd">
<td>middleName</td>
<td>Opt</td>
<td>string</td>
<td><p>Middle name of the consumer. Length: 0–32</p>
<p>Pattern: ^[a-zA-Z][a-zA-Z0-9_(){}&amp;@!+#.$,%*-]*</p></td>
</tr>
<tr class="even">
<td>nickname</td>
<td>Opt</td>
<td>string</td>
<td><p>Short name used to refer to the consumer. Length: 0–30</p>
<p>For example, BOB is a nickname for ROBERT.</p>
<p>Pattern: ^[a-zA-Z0-9_(){}&amp;@!+#.'$, %*-]{1,30}$</p></td>
</tr>
<tr class="odd">
<td>prefix</td>
<td>Opt</td>
<td>string</td>
<td><p>A title relating to the consumer, such as DR., MR., or MS.
Length: 0–6</p>
<p>Pattern: ^[a-zA-Z][a-zA-Z0-9_(){}&amp;@!+#.$,%*-]*</p></td>
</tr>
</tbody>
</table>

#### PaperSuppressionOption

<table>
<colgroup>
<col style="width: 14%" />
<col style="width: 5%" />
<col style="width: 8%" />
<col style="width: 70%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>isEmbedded</td>
<td>Req</td>
<td>boolean</td>
<td><p>Indicates if the presenter UI needs to enforce the user’s reading
of the actual terms and conditions (Ts&amp;Cs) consent text.</p>
<p>True - Present the actual Ts&amp;Cs to a consumer on the same page
where account number, preauth tokens, service address, etc. are
collected.</p>
<p>False – The presenter can include a URL link that the consumer can
click on to access the Ts&amp;Cs.</p></td>
</tr>
<tr class="even">
<td>option</td>
<td>Req</td>
<td>string</td>
<td><p>Paper suppression options applicable for the selected biller. The
presenter sends the consumer’s choices back to Fiserv in the Payee
E-bill Service POST request. Valid values:</p>
<p>DualDelivery<br />
PaperSuppression<br />
Trial</p></td>
</tr>
<tr class="odd">
<td>termsText</td>
<td>Opt</td>
<td>string</td>
<td>Text that applies to paper suppression. Each paper suppression type
has text (“terms of use”) that should be displayed on the UI. Note that
“terms of use” text is different than Ts&amp;Cs.</td>
</tr>
<tr class="even">
<td>termsUrl</td>
<td>Opt</td>
<td>string</td>
<td>A URL pointing to the actual terms and conditions text. If
isEmbedded is true, the contents of this URL are pulled into a
scrollable box that is presented to the consumer. If isEmbedded is
false, this URL is presented as a link.</td>
</tr>
</tbody>
</table>

#### Payee

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 4%" />
<col style="width: 12%" />
<col style="width: 61%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>name</td>
<td>Req</td>
<td>string</td>
<td><p>The name of the payee.</p>
<p>Length: 2–32</p></td>
</tr>
<tr class="even">
<td>nickname</td>
<td>Opt</td>
<td>string</td>
<td><p>The nickname of the payee. Only populated if provided by the
consumer.</p>
<p>Length: 1-30</p></td>
</tr>
<tr class="odd">
<td>maskedAccountNumber</td>
<td>Cond</td>
<td>string</td>
<td><p>The masked account number for the payee. Not applicable for
payees added as individuals.</p>
<p>Length: 1–32</p>
<p>Condition: Populated only if the consumer has provided an account
number when adding the payee.</p></td>
</tr>
<tr class="even">
<td>unmaskedAccount<br />
NumberUri</td>
<td>Cond</td>
<td>string</td>
<td><p>The URI to the full, unmasked account number. See “<a
href="#get-a-payees-unmasked-account-number">Get a Payee’s Unmasked
Account Number</a>.”<br />
Not applicable for payees added as individuals.</p>
<p>Condition: Populated only if the consumer has provided an account
number when adding the payee.</p></td>
</tr>
<tr class="odd">
<td>category</td>
<td>Opt</td>
<td>string</td>
<td>The consumer-defined category of the payee. Applies to bill payment
payees only.</td>
</tr>
<tr class="even">
<td>contactPhoneNumber</td>
<td>Opt</td>
<td>string</td>
<td>Phone number used to contact the payee if there are issues posting
the payment.</td>
</tr>
<tr class="odd">
<td>merchantUri</td>
<td>Opt</td>
<td>string</td>
<td>The URI to access the merchant directly.</td>
</tr>
<tr class="even">
<td>automaticTransaction<br />
Uris</td>
<td>Opt</td>
<td>Array of string</td>
<td>List of automatic payment models for the payee.</td>
</tr>
<tr class="odd">
<td>reminderModelUri</td>
<td>Opt</td>
<td>string</td>
<td>Reserved for future use.</td>
</tr>
<tr class="even">
<td>address</td>
<td>Opt</td>
<td><a href="#usaddress">USAddress</a></td>
<td>Payee address information.</td>
</tr>
<tr class="odd">
<td>overnightAddress</td>
<td>Opt</td>
<td><a href="#usaddress">USAddress</a></td>
<td>Address for overnight payments if one exists for the payee.</td>
</tr>
<tr class="even">
<td>lastUsedFunding<br />
AccountUri</td>
<td>Opt</td>
<td>string</td>
<td>The URI to access the most recent funding account used in a
transaction with this payee.</td>
</tr>
<tr class="odd">
<td>socialTokens</td>
<td>Opt</td>
<td>Array of<br />
PayeeSocialToken</td>
<td>Reserved for future use.</td>
</tr>
<tr class="even">
<td>accountTokens</td>
<td>Opt</td>
<td>Array of <a href="#payeeaccounttoken">PayeeAccountToken</a></td>
<td>Reserved for future use.</td>
</tr>
<tr class="odd">
<td>ebillService<br />
ActivationUri</td>
<td>Opt</td>
<td>string</td>
<td>The URI used to begin e-bill activation for an e-bill-capable payee.
Will only be provided if payee is e-bill-capable but not e-bill enabled;
otherwise, null is returned.</td>
</tr>
<tr class="even">
<td>ebillServiceActivationType</td>
<td>Req</td>
<td>string</td>
<td>Indicates the type of e-bill service activation for the payee. Valid
values: Full, Lite, None</td>
</tr>
<tr class="odd">
<td>ebillServiceUri</td>
<td>Opt</td>
<td>string</td>
<td>The URI for information needed for activation. Will only be provided
if the payee is e-bill-enabled.</td>
</tr>
<tr class="even">
<td>loginCredentials<br />
Uri</td>
<td>Opt</td>
<td>string</td>
<td>Reserved for future use.</td>
</tr>
<tr class="odd">
<td>payeeLogoUrl</td>
<td>Opt</td>
<td>string</td>
<td>The URI for the payee logo. The preferred logo size is 90 × 90
pixels.</td>
</tr>
<tr class="even">
<td>payeeLogoGeneric</td>
<td>Opt</td>
<td>boolean</td>
<td>Indicates if the logo returned for the payee is generic (not
specific to the payee).</td>
</tr>
<tr class="odd">
<td>modifiableFields</td>
<td>Req</td>
<td>Array of string</td>
<td>List of fields that can be changed for the payee.</td>
</tr>
<tr class="even">
<td>status</td>
<td>Req</td>
<td>string</td>
<td>Status of the payee. Valid values: Active, Inactive</td>
</tr>
<tr class="odd">
<td>source</td>
<td>Opt</td>
<td>string</td>
<td>Reserved for Fiserv use.</td>
</tr>
<tr class="even">
<td>earliestStandardTransactionDate</td>
<td>Opt</td>
<td>string</td>
<td>Earliest date that a standard (non-expedited, non-overnight)
transaction can be scheduled.</td>
</tr>
<tr class="odd">
<td>legacyPayeeId</td>
<td>Req</td>
<td>string</td>
<td>Legacy payee ID.</td>
</tr>
<tr class="even">
<td>ebillServiceActivationStatus</td>
<td>Req</td>
<td>string</td>
<td>Indicates the ebill activation status of the Payee. This flag is
always returned, even if the payee is not ebill capable. Valid values:
NotApplicable, Pending, Capable, Active, Inactive</td>
</tr>
<tr class="odd">
<td>billTransactionScheduleActive</td>
<td>Req</td>
<td>boolean</td>
<td><p>Indicates if a bill transaction schedule is already active (i.e.,
eBill Autopay is active) for the payee.</p>
<p>true - Bill transaction schedule is enabled for the payee.</p>
<p>false – Bill transaction schedule is not enabled for the
payee.</p></td>
</tr>
<tr class="even">
<td>billTransactionScheduleCapable</td>
<td>Req</td>
<td>boolean</td>
<td><p>Indicates when a payee is capable of an automatic transaction
schedule for paying ebills (eBill Autopay). Valid values: true,
false</p>
<p>This is true when:</p>
<ul>
<li><p>Payee is active</p></li>
<li><p>Payee is e-bill enabled</p></li>
<li><p>Biller supports Autopay</p></li>
<li><p>The first e-bill has been received</p></li>
</ul></td>
</tr>
<tr class="odd">
<td>recurringTransactionScheduleActive</td>
<td>Req</td>
<td>boolean</td>
<td><p>Indicates if a recurring transaction schedule (one or more) is
already set up for the payee.</p>
<p>true - Recurring transaction schedule is set up for the payee.</p>
<p>false – Recurring transaction schedule is set up for the
payee.</p></td>
</tr>
<tr class="even">
<td>isPaperTransactionsEnabled</td>
<td>Req</td>
<td>boolean</td>
<td><p>Indicates whether transactions to this payee are typically
processed by check/draft.</p>
<p>true – Transactions are typically processed by check/draft.</p>
<p>false – Transactions are not typically processed by
check/draft.</p></td>
</tr>
<tr class="odd">
<td>isRushDeliveryAvailable</td>
<td>Req</td>
<td>boolean</td>
<td><p>Indicates if rush delivery of the payment is an option for the
payee. Valid values: true, false</p>
<p>true – Rush delivery is an option.</p>
<p>false – Rush delivery is not an option.</p></td>
</tr>
<tr class="even">
<td>self</td>
<td>Req</td>
<td>string</td>
<td>The URI to access the payee directly.</td>
</tr>
<tr class="odd">
<td>id</td>
<td>Req</td>
<td>string</td>
<td>The ID of the payee.</td>
</tr>
</tbody>
</table>

#### PayeeAccountToken

| Parameter    | Req | Data Type                     | Description                                 |
|------------|----|--------|-------------------------------------------------|
| accountToken | Req | [AccountToken](#accounttoken) | The payee's account token.                  |
| self         | Req | string                        | Relative URI pointing to the object itself. |
| id           | Req | string                        | Unique identifier of the object.            |

#### PayeeDetail

| Parameter           | Req | Data Type | Description                                                                                                                  |
|--------------|-----|-----------|--------------------------------------------|
| billerId            | Opt | string    | Unique identifier for the biller assigned by Fiserv. Returned if e-bill capable.                                             |
| onlinePayeeName     | Req | string    | Name of the payee that is displayed when this payee is selected and used to add this payee.                                  |
| merchantZipRequired | Req | boolean   | Flag to indicate whether the merchant ZIP Code is required when adding as a payee to obtain correct payee merchant matching. |
| self                | Req | string    | URI for the probable merchant.                                                                                               |
| id                  | Req | string    | Identifier for the probable merchant.                                                                                        |

#### PayeeGroupDetails

| Parameter      | Req | Data Type         | Description                                                    |
|-----------|----|-----------|----------------------------------------------|
| isVisible      | Req | boolean           | Flag that indicates whether the payee group will be displayed. |
| name           | Req | string            | Name of the payee group. Length: 1-32                          |
| payeeGroupInfo | Opt | Array of payeeUri | List of payees assigned to the payee group.                    |
| self           | Req | string            | URI for the payee group.                                       |
| id             | Req | string            | Identifier for the payee group.                                |

#### PayeeGroupListOutput

| Parameter   | Req | Data Type                                        | Description |
|---------|----|-----------|-------------------------------------------------|
| payeeGroups | Req | Array of [PayeeGroupDetails](#payeegroupdetails) |             |

#### PayeeGroupOutput

| Parameter    | Req | Data Type                                               | Description                             |
|-----------|----|------------|---------------------------------------------|
| payeeResults | Req | [StringIpsListItemResponse](#stringipslistitemresponse) | Results of the payees within the group. |
| self         | Req | string                                                  | URI for the payee group.                |
| id           | Req | string                                                  | Identifier for the payee group.         |

#### PayeeGroupPayeeItem

| Parameter  | Req | Data Type | Description                                                                                                                                                                                                                                                                        |
|-----------|----|-----------|----------------------------------------------|
| payeeUri   | Req | string    | URI for the payee. The payee must be active. A payee can be assigned to only one payee group.                                                                                                                                                                                      |
| listItemId | Req | integer   | Serial number for this payee group request. Each item in the list of requests has a unique ID so that the response can be tied to the requested item in the list. If the request resulted in an error in a list, this ID will tell the caller which item in the list was in error. |

#### PayeeInfo

<table>
<colgroup>
<col style="width: 17%" />
<col style="width: 4%" />
<col style="width: 13%" />
<col style="width: 63%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>name</td>
<td>Req</td>
<td>string</td>
<td><p>The name of the payee.</p>
<p>Pattern: ^[\x20-\x5A\x5C\x5F-\x7E]+$</p>
<p>Length: 2–32</p></td>
</tr>
<tr class="even">
<td>nickname</td>
<td>Opt</td>
<td>string</td>
<td><p>The nickname of the payee. Length: 1-30</p>
<p>Pattern: ^[\x20\x2C-\x2E\x30-\x39\x41-\x5A\x61-\x7A\r\n]+$</p></td>
</tr>
<tr class="odd">
<td>accountNumber</td>
<td>Opt</td>
<td>string</td>
<td><p>The consumer’s account number with the payee. Length: 1–32</p>
<p>Pattern: ^[a-zA-Z0-9 !"#$%&amp;-]{1,32}$</p></td>
</tr>
<tr class="even">
<td>contactPhoneNumber</td>
<td>Cond</td>
<td>string</td>
<td><p>Phone number used to contact the payee if there are issues
posting the transaction. Length: 10-12</p>
<p>Pattern:
(^[0-9]{10}$)|(^\(?[0-9]{3}\)?-[0-9]{3}-[0-9]{4}$)|(^\(?[0-9]{3}\)?\s?[0-9]{3}-[0-9]{4}$)</p>
<p>Must be numeric and may contain a dash or space between the numbers
at the appropriate placement. The phone number must be valid based on
the North American Numbering Plan (for example, the area code cannot
begin with a 0 or 1). Example: 234-555-1212</p>
<p>Condition: Required when sourceUri is not supplied when adding the
payee.</p></td>
</tr>
<tr class="odd">
<td>address</td>
<td>Cond</td>
<td><a href="#usaddress">USAddress</a></td>
<td><p>Payee address information.</p>
<p>Condition: Required when sourceUri is not supplied when adding the
payee.</p></td>
</tr>
<tr class="even">
<td>overnightAddress</td>
<td>Opt</td>
<td><a href="#usaddress">USAddress</a></td>
<td>Address for overnight payments if one exists for the payee.</td>
</tr>
<tr class="odd">
<td>socialTokens</td>
<td>Opt</td>
<td>Array of<br />
SocialToken</td>
<td>Reserved for future use.</td>
</tr>
<tr class="even">
<td>accountTokens</td>
<td>Opt</td>
<td>Array of <a href="#accounttokenaddinfo">AccountTokenAddInfo</a></td>
<td>Reserved for future use.</td>
</tr>
</tbody>
</table>

#### PayeeList

| Parameter | Req | Data Type                | Description |
|-----------|-----|--------------------------|-------------|
| payees    | Req | Array of [Payee](#payee) |             |

#### PendingTransactionsNotCancelled

| Parameter | Req  | Data Type | Description                        |
|-----------|------|-----------|------------------------------------|
| self      | Cond | string    | URI pointing to the object itself. |
| id        | Cond | string    | Unique identifier for the object.  |

#### Phone

<table>
<colgroup>
<col style="width: 15%" />
<col style="width: 3%" />
<col style="width: 9%" />
<col style="width: 71%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>phoneType</td>
<td>Req</td>
<td>string</td>
<td>The type of phone number. Valid values: Day, Evening, Mobile</td>
</tr>
<tr class="even">
<td>value</td>
<td>Req</td>
<td>string</td>
<td><p>The phone number value.<br />
Pattern:
(^[0-9]{10}$)|(^\(?[0-9]{3}\)?-[0-9]{3}-[0-9]{4}$)|(^\(?[0-9]{3}\)?\s?[0-9]{3}-[0-9]{4}$)</p>
<p>For a user enrollment, all non-numeric characters shall be removed
and if there is a leading “1,” it is removed. It may contain a dash or
space between the numbers at the appropriate placement. The phone number
must be valid based on the North American Numbering Plan (for example,
the area code cannot begin with a 0 or 1).</p></td>
</tr>
</tbody>
</table>

#### PotentialPayee

| Parameter                | Req | Data Type                                        | Description                                                                                                                                                                                |
|--------------|-----|----------|---------------------------------------------|
| maskedAccountNumber      | Opt | string                                           | Consumer’s masked account number with the merchant. Length: 1–32. Only the last 4 digits are displayed.                                                                                    |
| unmaskedAccountNumberUri | Opt | string                                           | The URI to the full unmasked account number. See “[Get a Potential Payee’s Unmasked Account Number](#get-a-potential-payees-unmasked-account-number).”                                     |
| additionalInfoRequired   | Req | boolean                                          | Indicates that additional information is required using VerificationToken information.                                                                                                     |
| merchantData             | Req | [MerchantData](#merchantdata)                    | Information about the merchant found.                                                                                                                                                      |
| verificationTokens       | Opt | Array of [VerificationToken](#verificationtoken) | Array of verification token information that the consumer needs to provide to verify the merchant relationship.                                                                            |
| verificationUri          | Opt | string                                           | The URI that will be used to verify the merchant relationship for the potential payee when verification is required.                                                                       |
| dormantAccount           | Req | boolean                                          | Indicates if the payee account is considered dormant. These are accounts that have not been active, with a balance or had a payment for over a year. This will only be returned when true. |
| legacyPayeeId            | Opt | string                                           | Reserved for future use.                                                                                                                                                                   |
| self                     | Req | string                                           | The URI to access the potential payee directly.                                                                                                                                            |
| id                       | Req | string                                           | The ID of the potential payee.                                                                                                                                                             |

#### PotentialPayeeList

| Parameter       | Req | Data Type                                  | Description                     |
|---------|----|-----------|-------------------------------------------------|
| potentialPayees | Req | Array of [PotentialPayee](#potentialpayee) | List of found potential payees. |

#### PreAuthToken

| Parameter         | Req | Data Type | Description                                                                                                                                 |
|-----------|-----|-------|-------------------------------------------------|
| name              | Req | string    | Name of the token element to be provided by the consumer that the biller will use for verification.                                         |
| value             | Req | string    | Value for the authentication token, pre-filled for the consumer. If applicable, this information may be pulled from the consumer's profile. |
| regularExpression | Opt | string    | A validation string that that tells the UI how to validate the token element.                                                               |

#### PreAuthTokenInput

| Parameter | Req | Data Type | Description                                                                                         |
|-----------|-----|-------|-------------------------------------------------|
| name      | Req | string    | Name of the token element to be provided by the consumer that the biller will use for verification. |
| value     | Req | string    | The values from the consumer for the authentication tokens.                                         |

#### ProbableMerchants

| Parameter         | Req | Data Type                                | Description                      |
|---------|----|-----------|------------------------------------------------|
| probablePayeeList | Req | Array of [ProbablePayee](#probablepayee) | Zero or more probable merchants. |

#### ProbablePayee

| Parameter                | Req | Data Type                                  | Description                 |
|-----------|----|-----------|----------------------------------------------|
| categoryId               | Req | integer                                    | Category ID.                |
| category                 | Req | string                                     | Category name.              |
| suggestedDisplayPriority | Req | integer                                    | Suggested display priority. |
| merchantList             | Req | Array of [MerchantDetail](#merchantdetail) | Merchant details.           |

#### PutPayeeGroupOutput

| Parameter    | Req | Data Type                                               | Description                             |
|-----------|----|------------|---------------------------------------------|
| payeeResults | Req | [StringIpsListItemResponse](#stringipslistitemresponse) | Results of the payees within the group. |

#### RecurringEligibleFundingAccount

| Parameter              | Req | Data Type        | Description                                                                                        |
|------------|-----|----------|-----------------------------------------------|
| earliestInitiationDate | Req | string           | Earliest available payment (EAP) date with no fee for bank accounts. EAP+1 date for card accounts. |
| fundingAccountUri      | Req | string           | The source funding account URI for the automatic transaction.                                      |
| maxAmount              | Req | number ($double) | Maximum amount allowed for a recurring payment.                                                    |
| minAmount              | Req | number ($double) | Minimum amount allowed for a recurring payment.                                                    |

#### RecurringTransactionSchedule

<table>
<colgroup>
<col style="width: 17%" />
<col style="width: 5%" />
<col style="width: 8%" />
<col style="width: 68%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>amount</td>
<td>Req</td>
<td>double</td>
<td><p>The transaction amount.</p>
<p>Pattern: ^\d+(\.\d{1,2})?$</p></td>
</tr>
<tr class="even">
<td>endDate</td>
<td>Opt</td>
<td>string</td>
<td><p>The end date for this automatic transaction schedule. A null
value means transactions will be scheduled indefinitely. Should not be
submitted alongside maximumTransactionCount.</p>
<p>Format: yyyy-MM-dd</p></td>
</tr>
<tr class="odd">
<td>endTransactionAmount</td>
<td>Opt</td>
<td>double</td>
<td><p>Amount for the last transaction. This will be provided by the
consumer if the last transaction amount is different from the rest of
the transactions in the recurring model.</p>
<p>Pattern: ^\d+(\.\d{1,2})?$</p></td>
</tr>
<tr class="even">
<td>frequency</td>
<td>Req</td>
<td>string</td>
<td>The frequency that transactions will be scheduled to the payee.
Valid values:<br />
Weekly, Every2Weeks, Every4Weeks, TwiceAMonth, Monthly, Every2Months,
Every3Months, Every4Months, Every6Months, Annually</td>
</tr>
<tr class="odd">
<td>initialTransactionAmount</td>
<td>Opt</td>
<td>double</td>
<td><p>Amount for the first transaction. This will be provided by the
consumer if the first transaction amount is different from the rest of
the transactions in the recurring model.</p>
<p>Pattern: ^\d+(\.\d{1,2})?$</p></td>
</tr>
<tr class="even">
<td>initiationDate</td>
<td>Req</td>
<td>string</td>
<td>The date of the first scheduled transaction. Format: yyyy-MM-dd</td>
</tr>
<tr class="odd">
<td>nextTransactionDate</td>
<td>Opt</td>
<td>string</td>
<td><p>Date of the next payment transaction to be scheduled. If there is
not a next payment (such as when a model is terminated due to reaching
the end of the model), this field is not populated.</p>
<p>Note: Not used for POST.</p></td>
</tr>
<tr class="even">
<td>maximumTransactionCount</td>
<td>Opt</td>
<td>integer</td>
<td>The maximum number of transactions that this automatic transaction
model should process before ending. Should not be submitted alongside
endDate.</td>
</tr>
<tr class="odd">
<td>remainingTransactionCount</td>
<td>Opt</td>
<td>integer</td>
<td>The number of transactions remaining before this automatic payment
model ends.<br />
Note: Not used for POST.</td>
</tr>
<tr class="even">
<td>memo</td>
<td>Opt</td>
<td>string</td>
<td><p>Transaction memo. Maximum length: 34</p>
<p>Pattern: ^[a-zA-Z0-9_(){}&amp;@!+#.'$,%^ *-]*</p></td>
</tr>
<tr class="odd">
<td>transactionScheduledAlert</td>
<td>Req</td>
<td>boolean</td>
<td><p>Indicates if the consumer is notified when a transaction is
scheduled.</p>
<p>True - Notify the consumer when a transaction is scheduled.</p>
<p>False - Do not notify the consumer when a transaction is scheduled.
This is the default.</p></td>
</tr>
<tr class="even">
<td>transactionSentAlert</td>
<td>Req</td>
<td>boolean</td>
<td><p>Indicates if the consumer is notified when a transaction is
processed.</p>
<p>True - Notify the consumer when a transaction is processed.</p>
<p>False - Do not notify the consumer when a transaction is processed.
This is the default.</p></td>
</tr>
<tr class="odd">
<td>recurringScheduleExpireAlert</td>
<td>Req</td>
<td>boolean</td>
<td><p>Indicates if the consumer is notified when the final transaction
from a recurring model is scheduled.</p>
<p>True - Notify the consumer when the final transaction from the
recurring model is scheduled.</p>
<p>False - Do not notify the consumer when the final transaction from
the recurring model is scheduled. This is the default.</p></td>
</tr>
<tr class="even">
<td>duration</td>
<td>Req</td>
<td>string</td>
<td>The duration of recurring model. Valid values:
UntilSubscriberCancels, XNumberOfPayments, SpecificEndDate<br />
Note: Not used for POST.</td>
</tr>
<tr class="odd">
<td>nextTransactionGeneration<br />
Date</td>
<td>Opt</td>
<td>string</td>
<td>Date when the next payment will be generated/spawned. This date may
not correspond to the date of the next actual payment, because that
payment could already have been spawned. This field will not be returned
when there is no next payment to be spawned.<br />
Note: Not used for POST.</td>
</tr>
</tbody>
</table>

#### RecurringTransactionSchedulePatch

<table>
<colgroup>
<col style="width: 17%" />
<col style="width: 5%" />
<col style="width: 8%" />
<col style="width: 68%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>amount</td>
<td>Opt</td>
<td>double</td>
<td><p>The transaction amount.</p>
<p>Pattern: ^\d+(\.\d{1,2})?$</p></td>
</tr>
<tr class="even">
<td>endDate</td>
<td>Opt</td>
<td>string</td>
<td><p>The end date for this automatic transaction schedule. A null
value means transactions will be scheduled indefinitely. Should not be
submitted alongside maximumTransactionCount.</p>
<p>Format: yyyy-MM-dd</p></td>
</tr>
<tr class="odd">
<td>endTransactionAmount</td>
<td>Opt</td>
<td>double</td>
<td><p>Amount for the last transaction. This will be provided by the
consumer if the last transaction amount is different from the rest of
the transactions in the recurring model.</p>
<p>Pattern: ^\d+(\.\d{1,2})?$</p></td>
</tr>
<tr class="even">
<td>frequency</td>
<td>Opt</td>
<td>string</td>
<td>The frequency that transactions will be scheduled to the payee.
Valid values:<br />
Weekly, Every2Weeks, Every4Weeks, TwiceAMonth, Monthly, Every2Months,
Every3Months, Every4Months, Every6Months, Annually</td>
</tr>
<tr class="odd">
<td>initiationDate</td>
<td>Opt</td>
<td>string</td>
<td>The date of the first scheduled transaction. Format: yyyy-MM-dd</td>
</tr>
<tr class="even">
<td>maximumTransactionCount</td>
<td>Opt</td>
<td>integer</td>
<td>The maximum number of transactions that this automatic transaction
model should process before ending. Should not be submitted alongside
endDate.</td>
</tr>
<tr class="odd">
<td>memo</td>
<td>Opt</td>
<td>string</td>
<td><p>Transaction memo. Maximum length: 34</p>
<p>Pattern: ^[a-zA-Z0-9_(){}&amp;@!+#.'$,%^ *-]*</p></td>
</tr>
<tr class="even">
<td>transactionScheduledAlert</td>
<td>Opt</td>
<td>boolean</td>
<td><p>Indicates if the consumer is notified when a transaction is
scheduled.</p>
<p>True - Notify the consumer when a transaction is scheduled.</p>
<p>False - Do not notify the consumer when a transaction is scheduled.
This is the default.</p></td>
</tr>
<tr class="odd">
<td>transactionSentAlert</td>
<td>Opt</td>
<td>boolean</td>
<td><p>Indicates if the consumer is notified when a transaction is
processed.</p>
<p>True - Notify the consumer when a transaction is processed.</p>
<p>False - Do not notify the consumer when a transaction is processed.
This is the default.</p></td>
</tr>
<tr class="even">
<td>recurringScheduleExpireAlert</td>
<td>Opt</td>
<td>boolean</td>
<td><p>Indicates if the consumer is notified when the final transaction
from a recurring model is scheduled.</p>
<p>True - Notify the consumer when the final transaction from the
recurring model is scheduled.</p>
<p>False - Do not notify the consumer when the final transaction from
the recurring model is scheduled. This is the default.</p></td>
</tr>
</tbody>
</table>

#### RecurringTransactionScheduleTypesSupported

<table>
<colgroup>
<col style="width: 22%" />
<col style="width: 4%" />
<col style="width: 19%" />
<col style="width: 53%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>frequency</td>
<td>Req</td>
<td>Array of string</td>
<td>The available frequency options for setting up a recurring payment.
Valid values:<br />
Weekly, Every2Weeks, Every4Weeks, TwiceAMonth, Monthly, Every2Months,
Every3Months, Every4Months, Every6Months, Annually</td>
</tr>
<tr class="even">
<td>duration</td>
<td>Req</td>
<td>Array of string</td>
<td><p>The available duration options for setting up a recurring
payment. Valid values:</p>
<p>UntilSubscriberCancels, XnumberOfPayments, SpecificEndDate</p></td>
</tr>
<tr class="odd">
<td>eligibleFundingAccounts</td>
<td>Req</td>
<td>Array of <a
href="#recurringeligiblefundingaccount">RecurringEligibleFundingAccount</a></td>
<td>A list of eligible funding accounts with the available automatic
transaction options.</td>
</tr>
</tbody>
</table>

#### Reminder

<table>
<colgroup>
<col style="width: 23%" />
<col style="width: 4%" />
<col style="width: 14%" />
<col style="width: 57%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>alertAtLeadTimeBeforePaymentDueDate</td>
<td>Req</td>
<td>boolean</td>
<td><p>Indicates if the consumer is reminded at lead time (see leadTime)
before a payment is due.</p>
<p>Valid values:</p>
<p>true – Remind the consumer at lead time before a payment is due.</p>
<p>false – Do not remind the consumer at lead time before a payment is
due.</p></td>
</tr>
<tr class="even">
<td>alertIfNoBillPaymentMadeByDueDate</td>
<td>Req</td>
<td>boolean</td>
<td><p>Indicates if the consumer is reminded if no payment has been made
by the due date.</p>
<p>Valid values:</p>
<p>true – Remind the consumer that a payment is due.</p>
<p>false – Do not remind the consumer that a payment is due.</p></td>
</tr>
<tr class="odd">
<td>alertWhenBillPayment Processes</td>
<td>Req</td>
<td>boolean</td>
<td><p>Indicates if the consumer is notified when a payment is
processed.</p>
<p>Valid values:</p>
<p>true – Notify the consumer when a payment has been marked as
processed.</p>
<p>false – Do not notify the consumer when a payment has been marked as
processed.</p></td>
</tr>
<tr class="even">
<td>amount</td>
<td>Req</td>
<td>number</td>
<td>The payment amount associated with this reminder.</td>
</tr>
<tr class="odd">
<td>currentDueDate</td>
<td>Req</td>
<td>string</td>
<td>Next date for the bill payment reminder in yyyy-MM-dd format.</td>
</tr>
<tr class="even">
<td>firstReminderDate</td>
<td>Req</td>
<td>string</td>
<td>First date for the bill payment reminder in yyyy-MM-dd format.</td>
</tr>
<tr class="odd">
<td>frequency</td>
<td>Req</td>
<td>string</td>
<td><p>The frequency of how often the reminder is automatically created.
Valid values:</p>
<p>Weekly<br />
Every2Weeks<br />
TwiceAMonth<br />
Every4Weeks<br />
Monthly<br />
Every2Months<br />
Every3Months<br />
Every4Months<br />
Every6Months<br />
Annually</p></td>
</tr>
<tr class="even">
<td>leadTime</td>
<td>Req</td>
<td>string</td>
<td><p>Number of days (excluding any risk management calculations)
before the due date that the reminder will be created. Valid values:</p>
<p>Lead03Days<br />
Lead05Days<br />
Lead10Days<br />
Lead14Days<br />
Lead21Days<br />
Lead28Days</p></td>
</tr>
<tr class="odd">
<td>payeeUri</td>
<td>Req</td>
<td>string</td>
<td>URI for the payee. This matches the payee URI returned in GET
Payees.</td>
</tr>
<tr class="even">
<td>self</td>
<td>Req</td>
<td>string</td>
<td>URI for the reminder model.</td>
</tr>
<tr class="odd">
<td>id</td>
<td>Req</td>
<td>string</td>
<td>Identifier for the reminder model.</td>
</tr>
</tbody>
</table>

#### ReminderList

| Parameter | Req | Data Type                      | Description |
|-----------|-----|--------------------------------|-------------|
| reminders | Req | Array of [Reminder](#reminder) |             |

#### ResultInfo

<table>
<colgroup>
<col style="width: 14%" />
<col style="width: 4%" />
<col style="width: 8%" />
<col style="width: 71%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>resultCategory</td>
<td>Req</td>
<td>string</td>
<td><p>Description of the kind of result. Valid values:</p>
<p>ERROR<br />
WARNING<br />
INFO</p></td>
</tr>
<tr class="even">
<td>code</td>
<td>Req</td>
<td>string</td>
<td>Code associated with the result of a request.</td>
</tr>
<tr class="odd">
<td>field</td>
<td>Opt</td>
<td>string</td>
<td>Matches the name of a field causing an error.</td>
</tr>
<tr class="even">
<td>fieldPath</td>
<td>Opt</td>
<td>string</td>
<td>Specifies the field causing an error. In addition to the field name,
FieldPath includes the tags that the field is nested in, starting with
the request level.</td>
</tr>
<tr class="odd">
<td>listItemId</td>
<td>Opt</td>
<td>string</td>
<td>For some requests, each item in a list has a unique ID so that the
response can be tied to the requested item in the list. If the request
resulted in an error in a list, this ID will tell the caller which item
in the list was in error.</td>
</tr>
<tr class="even">
<td>description</td>
<td>Req</td>
<td>string</td>
<td>A textual description of &lt;code&gt;.</td>
</tr>
</tbody>
</table>

#### ResultType

| Parameter  | Req | Data Type                          | Description                                                                       |
|-----------|-----|-------|-------------------------------------------------|
| success    | Req | boolean                            | Indicates whether a request succeeded or failed.                                  |
| resultInfo | Req | Array of [ResultInfo](#resultinfo) | A list of the results associated with attempting to perform the requested action. |

#### SensitiveInformationV2

| Parameter            | Req  | Data Type | Description                                                                                                                                                                                     |
|-----------|-----|-------|-------------------------------------------------|
| birthDate            | Req  | string    | Date of birth of the consumer.                                                                                                                                                                  |
| taxId                | Req  | string    | Consumer's ID for tax purposes.                                                                                                                                                                 |
| driversLicenseNumber | Opt  | string    | Driver's license number or state-issued ID number of the consumer.                                                                                                                              |
| driversLicenseState  | Cond | string    | State associated with the driver's license number or state-issued ID number. Condition: This field is required if the consumer's driver's license number or state-issued ID number is supplied. |
| securityQuestion     | Cond | string    | If provided at enrollment, a security question prompt for the consumer                                                                                                                          |
| securityAnswer       | Cond | string    | If provided at enrollment, an identifier used by the consumer for security purposes.                                                                                                            |

#### StringIpsListItemResponse

| Parameter  | Req | Data Type                 | Description                                                |
|-----------|-----|-------|-------------------------------------------------|
| listItemId | Req | integer                   | Serial number that was sent in the request for this payee. |
| result     | Req | [ResultType](#resulttype) | Result information.                                        |

#### ToDo

<table>
<colgroup>
<col style="width: 14%" />
<col style="width: 5%" />
<col style="width: 8%" />
<col style="width: 70%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>toDoType</td>
<td>Req</td>
<td>string</td>
<td><p>Identifies the item’s ToDo type. Valid values:</p>
<p>PotentialPayee<br />
Reminder<br />
UnpaidBill</p>
<p>UnpaidBill applies to both e-bills and bill due alerts.</p></td>
</tr>
<tr class="even">
<td>toDoDetailUri</td>
<td>Req</td>
<td>string</td>
<td>URI for the item that requires the consumer’s attention. For
example, the URI to a potential payee that needs to be
added/verified.</td>
</tr>
<tr class="odd">
<td>payeeUri</td>
<td>Opt</td>
<td>string</td>
<td>The corresponding payee URI for a Reminder or UnpaidBill
toDoType.</td>
</tr>
<tr class="even">
<td>verificationRequired</td>
<td>Cond</td>
<td>boolean</td>
<td><p>Condition: Required for toDoType = PotentialPayee. Indicates if
verification is required before adding the payee.</p>
<p>True – Verification required.</p>
<p>False – Verification not required.</p></td>
</tr>
<tr class="odd">
<td>actionDate</td>
<td>Opt</td>
<td>string</td>
<td><p>The date that the ToDo item requires action to avoid possible
negative consequences. Example: due date of a pending e-bill.</p>
<p>Format: yyyy-MM-dd</p>
<p>For Reminders, this is the due date entered by the consumer to be
used as the trigger for the reminder message.</p></td>
</tr>
<tr class="even">
<td>amount</td>
<td>Opt</td>
<td>double</td>
<td>The amount of the ToDo item if there is one. For e-bills and bill
due alerts this is the AmountDue (not minimum amount or balance). For
Reminders, this is the amount entered by the consumer to appear in the
reminder message.</td>
</tr>
<tr class="odd">
<td>maskedAccountNumber</td>
<td>Opt</td>
<td>string</td>
<td>The masked account number of the account.</td>
</tr>
<tr class="even">
<td>description</td>
<td>Req</td>
<td>string</td>
<td>A description relevant to the ToDo type. For PotentialPayees, this
will be the merchant name. For Reminder and UnpaidBill, this will be the
payee name.</td>
</tr>
<tr class="odd">
<td>self</td>
<td>Req</td>
<td>string</td>
<td>URI for the ToDo item.</td>
</tr>
<tr class="even">
<td>id</td>
<td>Req</td>
<td>string</td>
<td>Identifier for the ToDo item.</td>
</tr>
</tbody>
</table>

#### Transaction

<table>
<colgroup>
<col style="width: 14%" />
<col style="width: 5%" />
<col style="width: 8%" />
<col style="width: 72%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>listItemId</td>
<td>Req</td>
<td>integer</td>
<td>Serial number for this transaction that was sent in a request
containing multiple transactions. Use this number to identify the
response for each transaction in the request, because the sequence of
responses may not match the sequence of requests due to web services
infrastructure. Each listItemId must be unique within a request.</td>
</tr>
<tr class="even">
<td>amount</td>
<td>Req</td>
<td>double</td>
<td><p>The amount of the transaction. The transaction amount must be
within tenant/sponsor limits (minimum and maximum).</p>
<p>Pattern: ^\d+(\.\d{1,2})?$</p>
<p>A payment funded with a bank account that is Pending Confirmation
cannot be scheduled with an amount that exceeds the Fiserv cumulative
unconfirmed payment limit. This does not apply to SameDay
Payments.</p></td>
</tr>
<tr class="odd">
<td>deliveryDate</td>
<td>Req</td>
<td>string</td>
<td><p>The date for the transaction in yyyy-MM-dd format.</p>
<p>This date may not be in the past and may not be more than 365 days in
the future. This date may not be earlier than the earliest available
payment date for the selected payee.</p>
<p>Generally, the Fiserv system will not allow a payment to be scheduled
for a date that is not a bank processing date (weekends and Federal
Reserve Board recognized holidays). However, this will depend on the
merchant’s configuration. For example, some MoneyGram merchants accept
payments on weekends.</p>
<p>When the payee is Overnight Check (ONC) capable and the current time
is before the cutoff time for ONC, the payment will be added as an ONC
payment.</p>
<p>A payment funded with a bank account that is Pending Confirmation
cannot be scheduled with a payment date greater than 45 days after the
bank account was added.</p>
<p>For SameDay Payments, this date must be current date.</p></td>
</tr>
<tr class="even">
<td>fundingAccountUri</td>
<td>Req</td>
<td>string</td>
<td><p>The source funding account URI for the given transaction. Account
types that are eligible to be used are those enabled for Bill Payment
(service) for the tenant/sponsor.</p>
<p>If account confirmation is enabled for the tenant/sponsor, only
accounts that are Confirmed or Pending Confirmation can be used to
schedule a payment.</p></td>
</tr>
<tr class="odd">
<td>note</td>
<td>Opt</td>
<td>string</td>
<td><p>A consumer’s “note to self.” This note is not submitted to the
payee. Length: 0–255</p>
<p>Pattern:
^[\x2A-\x2E\x30-\x39\x40-\x5A\x5F\x5E\x61-\x7A\x20\x21\x23-\x25]+$</p>
<p>The note field only allows the following character sets: a-z, A-Z,
0-9, _ @!+#.$,%^*-</p>
<p>This note is a convenience feature. If the payment is scheduled
successfully but Fiserv is unable to store the payment note, you will
receive only a warning, not an error.</p></td>
</tr>
<tr class="even">
<td>destinationUri</td>
<td>Req</td>
<td>string</td>
<td><p>The destination URI for the given transaction. May identify the
payee, bill due alert, and/or e-bill to be paid.</p>
<ul>
<li><p>If the destination is a payee, the payee must exist and be active
for the consumer.</p></li>
<li><p>If the destination is an e-bill, the e-bill ID must be a valid
e-bill ID for the consumer.</p></li>
</ul></td>
</tr>
<tr class="odd">
<td>withdrawNow</td>
<td>Opt</td>
<td>boolean</td>
<td>Reserved for future use.</td>
</tr>
<tr class="even">
<td>memo</td>
<td>Opt</td>
<td>string</td>
<td>Transaction memo. Maximum of 34 characters.</td>
</tr>
<tr class="odd">
<td>cavv</td>
<td>Opt</td>
<td>string</td>
<td>Placeholder for 3-D Secure. Cardholder Authentication Verification
Value. Used if the transaction is funded by a card account and the
institution is participating in 3-D Secure.</td>
</tr>
</tbody>
</table>

#### TransactionCalendar

| Parameter                 | Req | Data Type                                          | Description                                                                 |
|------------|-----|----------|-----------------------------------------------|
| deliveryDate              | Req | string                                             | The date of delivery of the transaction. Format: yyyy-MM-dd                 |
| transactionDestinationUri | Req | string                                             | Transaction destination associated with this specific transaction calendar. |
| transactionOptions        | Req | Array of [TransactionOptions](#transactionoptions) | The various options that are provided for a transaction.                    |

#### TransactionList

| Parameter    | Req | Data Type                                                    | Description                                                                  |
|---------|----|-----------|-------------------------------------------------|
| transactions | Req | Array of [TransactionOutputDetail](#transactionoutputdetail) | List of transactions. There is an empty array if there is no data to return. |

#### TransactionOptions

<table>
<colgroup>
<col style="width: 26%" />
<col style="width: 9%" />
<col style="width: 13%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>debitDate</td>
<td>Opt</td>
<td>string</td>
<td>The date the funds are debited from the user’s account. Only applies
to card funded transactions. (format: yyyy-MM-dd)</td>
</tr>
<tr class="even">
<td>additionalInfoRequired</td>
<td>Opt</td>
<td>Array of string</td>
<td>Additional information that may be required to make a certain
payment date available. Valid value: “OvernightAddress”</td>
</tr>
<tr class="odd">
<td>cutOffTime</td>
<td>Opt</td>
<td>string</td>
<td><p>Cutoff date and time for the transaction in UTC format. Not
returned for future processing dates.</p>
<p>To determine if a transaction option is valid, compare current time
(in UTC format) to the provided cut off time:</p>
<ul>
<li><p>If &lt;current time in UTC&gt; is before &lt;cutoff time&gt; then
the date can be used.</p></li>
<li><p>If &lt;current time in UTC&gt; is after &lt;cutoff time&gt; then
that date is no longer valid.</p></li>
</ul></td>
</tr>
<tr class="even">
<td>deliveryMethod</td>
<td>Opt</td>
<td>string</td>
<td>The method of delivery of the transaction. Valid values:
"Electronic" "Paper" "Overnight"</td>
</tr>
<tr class="odd">
<td>fee</td>
<td>Opt</td>
<td>number</td>
<td>The fee amount for the transaction.</td>
</tr>
<tr class="even">
<td>fundingAccountUri</td>
<td>Req</td>
<td>string</td>
<td>The funding account for the transaction.</td>
</tr>
<tr class="odd">
<td>withdrawNow</td>
<td>Opt</td>
<td>boolean</td>
<td>Reserved for future use.</td>
</tr>
<tr class="even">
<td>instantDelivery</td>
<td>Opt</td>
<td>boolean</td>
<td>Reserved for future use.</td>
</tr>
<tr class="odd">
<td>isPreferredDate</td>
<td>Opt</td>
<td>boolean</td>
<td>When a transaction's due date is in the future, a value of true for
this parameter indicates that this transaction option is preferred. This
is the recommended option to pre-populate for a consumer.</td>
</tr>
<tr class="even">
<td>minAmount</td>
<td>Opt</td>
<td>number</td>
<td>The minimum amount of the transaction.</td>
</tr>
<tr class="odd">
<td>maxAmount</td>
<td>Opt</td>
<td>number</td>
<td>The maximum amount of the transaction.</td>
</tr>
</tbody>
</table>

#### TransactionOutput

<table>
<colgroup>
<col style="width: 24%" />
<col style="width: 4%" />
<col style="width: 8%" />
<col style="width: 63%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>self</td>
<td>Cond</td>
<td>string</td>
<td>URI pointing to the transaction itself. Condition: Always returned
for a successful response.</td>
</tr>
<tr class="even">
<td>id</td>
<td>Cond</td>
<td>string</td>
<td>Unique identifier for the transaction. Condition: Always returned
for a successful response.</td>
</tr>
<tr class="odd">
<td>confirmationNumber</td>
<td>Cond</td>
<td>string</td>
<td>Confirmation number for the payment transaction. Condition: Always
returned for a successful response.</td>
</tr>
<tr class="even">
<td>deliveryDate</td>
<td>Cond</td>
<td>string</td>
<td>The transaction expected delivery date, populated for pending and
completed transactions. Format: yyyy-MM-dd<br />
Condition: If the payment fails risk, deliveryDate is not returned.</td>
</tr>
<tr class="odd">
<td>nextAvailableTransactionDate</td>
<td>Cond</td>
<td>string</td>
<td>Next available transaction date. Condition: Returned only if the
payment fails risk (transaction date is too early).</td>
</tr>
<tr class="even">
<td>debitDate</td>
<td>Cond</td>
<td>string</td>
<td>Date that the consumer’s account is to be debited. Format:
yyyy-MM-dd<br />
Condition: Always returned for successful response.</td>
</tr>
<tr class="odd">
<td>legacyTransactionId</td>
<td>Req</td>
<td>string</td>
<td>Server transaction timestamp.</td>
</tr>
</tbody>
</table>

#### TransactionModifyOutput

<table>
<colgroup>
<col style="width: 23%" />
<col style="width: 6%" />
<col style="width: 8%" />
<col style="width: 61%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>self</td>
<td>Cond</td>
<td>string</td>
<td>URI pointing to the transaction itself. Condition: Always returned
for a successful response.</td>
</tr>
<tr class="even">
<td>id</td>
<td>Cond</td>
<td>string</td>
<td>Unique identifier for the transaction. Condition: Always returned
for a successful response.</td>
</tr>
<tr class="odd">
<td>confirmationNumber</td>
<td>Cond</td>
<td>string</td>
<td>Confirmation number for the payment transaction. Condition: Always
returned for a successful response.</td>
</tr>
<tr class="even">
<td>deliveryMethod</td>
<td>Cond</td>
<td>string</td>
<td>The delivery method for the transaction if the transaction has been
processed. Valid values: "Electronic" "Paper" Condition: If the payment
fails risk, deliveryMethod is not returned.</td>
</tr>
<tr class="odd">
<td>status</td>
<td>Cond</td>
<td>string</td>
<td>The status of the payment transaction. Valid values: "Pending",
"Complete", “InProcess”, "Failed", "Canceled" Condition: Always returned
for a successful response.</td>
</tr>
<tr class="even">
<td>statusUri</td>
<td>Cond</td>
<td>string</td>
<td>The URI to access the transaction status information. Condition:
Always returned for a successful response.</td>
</tr>
<tr class="odd">
<td>deliveryDate</td>
<td>Cond</td>
<td>string</td>
<td>The transaction expected delivery date, populated for pending and
completed transactions. Format: yyyy-MM-dd Condition: If the payment
fails risk, deliveryDate is not returned.</td>
</tr>
<tr class="even">
<td>cancelUri</td>
<td>Cond</td>
<td>string</td>
<td>The URI to access the transaction cancellation (if any). Condition:
Returned if payment can be canceled. Only Pending payments can be
canceled.<br />
Condition: If the payment fails risk, cancelUri is not returned.</td>
</tr>
<tr class="odd">
<td>transactionType</td>
<td>Cond</td>
<td>string</td>
<td>Type of transaction. Valid values: Standard, Overnight,
Expedited<br />
Condition: If the payment fails risk, transactionType is not
returned.</td>
</tr>
<tr class="even">
<td>deliveryTrackingNumber</td>
<td>Cond</td>
<td>string</td>
<td>Tracking number of the transaction. Condition: If the payment fails
risk, deliveryTrackingNumber is not returned.</td>
</tr>
<tr class="odd">
<td>nextAvailableTransactionDate</td>
<td>Cond</td>
<td>string</td>
<td>Next available transaction date. Condition: Returned only if the
payment fails risk (transaction date is too early).</td>
</tr>
<tr class="even">
<td>legacyTransactionId</td>
<td>Req</td>
<td>string</td>
<td>Server transaction timestamp.</td>
</tr>
</tbody>
</table>

#### TransactionOutputDetail

<table>
<colgroup>
<col style="width: 17%" />
<col style="width: 4%" />
<col style="width: 7%" />
<col style="width: 70%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>amount</td>
<td>Req</td>
<td>number</td>
<td>The amount of the transaction.</td>
</tr>
<tr class="even">
<td>automaticTransaction<br />
Uri</td>
<td>Opt</td>
<td>string</td>
<td>The URI to the automatic transaction model. If this field is
populated, this payment transaction is recurring.</td>
</tr>
<tr class="odd">
<td>billItemUri</td>
<td>Opt</td>
<td>string</td>
<td>The URI to the bill item associated with this transaction. Populated
only if the payment is associated with an e-bill or bill store
item.</td>
</tr>
<tr class="even">
<td>debitDate</td>
<td>Opt</td>
<td>string</td>
<td>Date that the consumer’s account was or is to be debited. Format:
yyyy-MM-dd</td>
</tr>
<tr class="odd">
<td>fee</td>
<td>Opt</td>
<td>number</td>
<td>The fee amount for this transaction.</td>
</tr>
<tr class="even">
<td>fundingAccountUri</td>
<td>Req</td>
<td>string</td>
<td>The source funding account URI for the given transaction.</td>
</tr>
<tr class="odd">
<td>note</td>
<td>Opt</td>
<td>string</td>
<td>A consumer’s “note to self.” This note is not submitted to the
payee.</td>
</tr>
<tr class="even">
<td>memo</td>
<td>Opt</td>
<td>string</td>
<td>Memo describing the payment. This text will be printed on the check
sent to the payee for this payment. A payment memo is only used for
payments that are to be processed via a paper check.</td>
</tr>
<tr class="odd">
<td>modifiableFields</td>
<td>Opt</td>
<td>Array of string</td>
<td>List of fields that can be changed for the transaction. For
transactions that are in Pending status only.</td>
</tr>
<tr class="even">
<td>payeeUri</td>
<td>Req</td>
<td>string</td>
<td>The URI to the payee that this transaction is associated with.</td>
</tr>
<tr class="odd">
<td>self</td>
<td>Cond</td>
<td>string</td>
<td>Relative Universal Resource Identifier pointing to the payment
transaction itself. Condition: Always returned for successful
response</td>
</tr>
<tr class="even">
<td>id</td>
<td>Cond</td>
<td>string</td>
<td>Unique identifier of the payment transaction. Condition: Always
returned for successful response</td>
</tr>
<tr class="odd">
<td>confirmationNumber</td>
<td>Req</td>
<td>string</td>
<td>Confirmation number for the payment transaction.</td>
</tr>
<tr class="even">
<td>deliveryMethod</td>
<td>Opt</td>
<td>string</td>
<td>The delivery method for the transaction if the transaction has been
processed. Valid values: "Electronic" "Paper"</td>
</tr>
<tr class="odd">
<td>status</td>
<td>Req</td>
<td>string</td>
<td><p>The status of the payment transaction. Valid values: "Pending",
"Complete", “InProcess”, "Failed", "Canceled"</p>
<p>Typically, immediately upon the scheduling of a transaction, it will
be in the Pending status. This is not the case when the transaction is
scheduled as expedited SameDay, or if the transaction is funded by
credit card (future release) or debit card for payment in the next five
processing days. In those cases, the transaction is scheduled directly
to InProcess. When a Pending transaction's delivery date becomes within
the payee’s lead days (how long it takes to route the money given the
delivery method), it moves to InProcess. When the funds are successfully
delivered to the transaction destination, the status becomes Completed.
If those funds are not delivered successfully, it instead becomes
Failed. Finally, if the transaction is successfully canceled in the
flow, it transitions to the Canceled status.</p></td>
</tr>
<tr class="even">
<td>statusUri</td>
<td>Req</td>
<td>string</td>
<td>The URI to access the transaction status information.</td>
</tr>
<tr class="odd">
<td>deliveryDate</td>
<td>Opt</td>
<td>string</td>
<td>The transaction expected delivery date, populated for pending and
completed transactions. Format: yyyy-MM-dd</td>
</tr>
<tr class="even">
<td>cancelUri</td>
<td>Cond</td>
<td>string</td>
<td><p>The URI to access the transaction cancellation (if any).</p>
<p>Condition: Returned if payment can be canceled. This field should
always be populated if a transaction is in Pending status. This field
will also be populated if a transaction is funded by credit card (future
release) or debit card, and the status is InProcess.</p></td>
</tr>
<tr class="odd">
<td>transactionType</td>
<td>Opt</td>
<td>string</td>
<td>Type of transaction. Valid values: Standard, Overnight,
Expedited<br />
If a transaction is scheduled to be paid with an overnight check, then
the transaction type is Overnight. If a transaction is a SameDay
electronic payment, then the type is Expedited. Otherwise, regardless of
whether the transaction is sent electronically or as a paper check, or
whether it is funded by a bank account or a card, it will have a
transaction type of Standard.</td>
</tr>
<tr class="even">
<td>deliveryTrackingNumber</td>
<td>Opt</td>
<td>string</td>
<td>Tracking number of the transaction. Only populated for a transaction
with the transaction type Overnight.</td>
</tr>
<tr class="odd">
<td>legacyTransactionId</td>
<td>Req</td>
<td>string</td>
<td>Server transaction timestamp.</td>
</tr>
<tr class="even">
<td>checkNumber</td>
<td>Cond</td>
<td>integer</td>
<td>Check number corresponding to completed paper payment. Condition:
This is returned when the status of the transaction is Complete and the
delivery method is Paper.</td>
</tr>
</tbody>
</table>

#### TransactionOutputIpsListItemResponse

| Parameter  | Req  | Data Type                               | Description                                                                                                                                                                                                                                       |
|-----------|-----|---------|------------------------------------------------|
| listItemId | Req  | integer                                 | Serial number that was sent in the request for a transaction. Use this number to identify the payment response to a payment request, because the sequence of responses may not match the sequence of requests due to web services infrastructure. |
| data       | Cond | [TransactionOutput](#transactionoutput) | Data about the specific transaction. Condition: Always returned for successful response                                                                                                                                                           |
| result     | Req  | [ResultType](#resulttype)               | Result associated with the specific transaction.                                                                                                                                                                                                  |

#### TransactionsNotModified

| Parameter | Req  | Data Type | Description                        |
|-----------|------|-----------|------------------------------------|
| self      | Cond | string    | URI pointing to the object itself. |
| id        | Cond | string    | Unique identifier for the object.  |

#### USAddress

<table>
<colgroup>
<col style="width: 10%" />
<col style="width: 6%" />
<col style="width: 8%" />
<col style="width: 75%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>address1</td>
<td>Req</td>
<td>string</td>
<td><p>Address line 1 information. Length: 1-32<br />
Cannot contain any of the following special characters:
&lt;&gt;=&amp;()</p>
<p>Pattern: ^(?=.*\w)^[a-zA-Z0-9_@!+#.$,/%^ *-]*</p>
<p>At least one character must be an alphanumeric character.</p></td>
</tr>
<tr class="even">
<td>address2</td>
<td>Opt</td>
<td>string</td>
<td><p>Any additional address information. Max length: 32</p>
<p>Cannot contain any of the following special characters:
&lt;&gt;=&amp;()<br />
Pattern: ^[a-zA-Z0-9_@!+#.$,/%^ *-]*</p></td>
</tr>
<tr class="odd">
<td>address3</td>
<td>Opt</td>
<td>string</td>
<td><p>Any additional address information. Max length: 32</p>
<p>Cannot contain any of the following special characters:
&lt;&gt;=&amp;()<br />
Pattern: ^[a-zA-Z0-9_@!+#.$,/%^ *-]*</p>
<p><strong>Payees GET</strong>: address3 is not currently returned in
the response.</p></td>
</tr>
<tr class="even">
<td>city</td>
<td>Req</td>
<td>string</td>
<td><p>The city name for the given address. Length: 1-32</p>
<p>Cannot contain any of the following special characters:
&lt;&gt;=&amp;()<br />
Pattern: ^[a-zA-Z0-9_ @!+#.$,%^*-]*</p>
<p>If the first three characters of the city name are "APO" or "FPO,"
the address is treated as a military address.</p></td>
</tr>
<tr class="odd">
<td>state</td>
<td>Req</td>
<td>string</td>
<td><p>The state for the given address. Valid characters: A–Z<br />
Length: 2</p>
<p>Pattern: ^[A-Z]*</p></td>
</tr>
<tr class="even">
<td>zipCode</td>
<td>Req</td>
<td>string</td>
<td><p>ZIP Code. Length: 5, 9, or 11</p>
<p>Pattern: ^(\d{5}|\d{9}|\d{11})$</p>
<p>Valid characters: 0–9</p>
<p>5 digits are required for this field. 9 or 11 digits are
optional.</p>
<p>Parsed: Chars 1-5 = Zip5 (must be a valid five-digit ZIP Code), Chars
6-9 = Zip4, Chars 10-11 = Zip2</p></td>
</tr>
</tbody>
</table>

#### UserV2

<table>
<colgroup>
<col style="width: 14%" />
<col style="width: 5%" />
<col style="width: 8%" />
<col style="width: 70%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>billingClass</td>
<td>Cond</td>
<td>string</td>
<td>If supplied, this field must contain a valid alphanumeric billing
class as supplied by Fiserv during implementation. Condition: This field
is required if Fiserv collects consumer fees on behalf of the
client.</td>
</tr>
<tr class="even">
<td>consumerTier</td>
<td>Opt</td>
<td>string</td>
<td><p>Tier level of consumer. Valid values:</p>
<p>1 (default)<br />
2<br />
3</p></td>
</tr>
<tr class="odd">
<td>isAllowedToSolicit</td>
<td>Opt</td>
<td>boolean</td>
<td><p>Indicates whether or not the consumer may be solicited by Fiserv.
This field allows a client to comply with state laws permitting a
resident to prohibit solicitation in writing or by telephone. Valid
values:</p>
<p>true – Yes, Fiserv can send information about additional products or
services to this consumer.</p>
<p>false – No, Fiserv may not send solicitation information. This is the
default.</p></td>
</tr>
<tr class="even">
<td>isEmployee</td>
<td>Opt</td>
<td>boolean</td>
<td><p>Identifies whether a consumer is an employee of the client. Valid
values:</p>
<p>true – Yes, an employee.</p>
<p>false – No, not an employee. This is the default.</p></td>
</tr>
<tr class="odd">
<td>sponsorBillingCategory</td>
<td>Cond</td>
<td>string</td>
<td>Clients that are doing their own billing but want to differentiate
between consumers can use this field to communicate the consumer
category to Fiserv. Condition: Based on sponsor setup.</td>
</tr>
<tr class="even">
<td>userId</td>
<td>Opt</td>
<td>string</td>
<td><p>ID used by the consumer to authenticate with their product(s).
Length: 1–48</p>
<p>This is only returned when the IdType in the request is
CheckFreeNextUserId.</p></td>
</tr>
<tr class="odd">
<td>hasPayees</td>
<td>Req</td>
<td>boolean</td>
<td>Indicates if the consumer has payees. This flag will return true
when the consumer has active and/or inactive payees.</td>
</tr>
<tr class="even">
<td>name</td>
<td>Req</td>
<td><a href="#name">Name</a></td>
<td>Consumer’s name.</td>
</tr>
<tr class="odd">
<td>businessName</td>
<td>Opt</td>
<td>string</td>
<td>Business name of the consumer. Length: 1–40</td>
</tr>
<tr class="even">
<td>contactEndPoints</td>
<td>Req</td>
<td><a href="#contactendpoint">Contact<br />
Endpoint</a></td>
<td>Contact details of the consumer, such as the consumer’s email
address, phone numbers, and address.</td>
</tr>
<tr class="odd">
<td>category</td>
<td>Opt</td>
<td>string</td>
<td>Client-assigned free-form category that identifies the group the
consumer belongs to. Length: 1-32</td>
</tr>
<tr class="even">
<td>sensitiveInformationUri</td>
<td>Req</td>
<td>string</td>
<td>URI for sensitive information.</td>
</tr>
<tr class="odd">
<td>timeZone</td>
<td>Req</td>
<td>integer</td>
<td>The time zone for the consumer. Defaults to “-05” for eastern
standard time (EST) unless a value is provided.</td>
</tr>
<tr class="even">
<td>country</td>
<td>Opt</td>
<td>string</td>
<td>The country of the consumer.</td>
</tr>
<tr class="odd">
<td>status</td>
<td>Req</td>
<td>string</td>
<td><p>The status of the consumer profile.</p>
<p>Valid values: Active, Inactive, FrozenFraud, FrozenOther,
CancelledFraud, CancelledOther, VerificationNeeded, Unspecified</p></td>
</tr>
<tr class="even">
<td>languageCode</td>
<td>Opt</td>
<td>string</td>
<td>Code representing the consumer’s language preference. Valid values:
“EN” (English), “ES” (Spanish). “EN” (English) is the default
value.</td>
</tr>
<tr class="odd">
<td>languageCountry</td>
<td>Opt</td>
<td>string</td>
<td>Country associated with the consumer’s language. Valid value: “US”
(United States). “US” is the default value.</td>
</tr>
<tr class="even">
<td>locale</td>
<td>Opt</td>
<td>string</td>
<td>The language code and associated language location in one combined
field. Format: XX-XX (languageCode and languageCountry separated by
hyphen).</td>
</tr>
<tr class="odd">
<td>newUser</td>
<td>Req</td>
<td>boolean</td>
<td>True if this is a new user; false if this is an existing user.</td>
</tr>
<tr class="even">
<td>occupation</td>
<td>Opt</td>
<td>string</td>
<td>The activity a user spends time performing to earn a living. Length:
1-50</td>
</tr>
<tr class="odd">
<td>modifiableFields</td>
<td>Req</td>
<td>Array of string</td>
<td>List of fields that can be changed for the user.</td>
</tr>
</tbody>
</table>

#### VerificationResponse

| Parameter                | Req | Data Type                                        | Description                                                                                                                                            |
|--------------|-----|----------|---------------------------------------------|
| payeeUri                 | Opt | string                                           | Provides a link to that payee resource. Provided when the potential payee matches an existing payee on the bill payment system.                        |
| maskedAccountNumber      | Opt | string                                           | Consumer’s masked account number with the merchant. Length: 1–32. Only the last 4 digits are displayed.                                                |
| unmaskedAccountNumberUri | Opt | string                                           | The URI to the full unmasked account number. See “[Get a Potential Payee’s Unmasked Account Number](#get-a-potential-payees-unmasked-account-number).” |
| additionalInfoRequired   | Req | boolean                                          | Indicates that additional information is required using VerificationToken information.                                                                 |
| merchantData             | Req | [MerchantData](#merchantdata)                    | Information about the merchant found.                                                                                                                  |
| verificationTokens       | Opt | Array of [VerificationToken](#verificationtoken) | Array of verification token information that the consumer needs to provide to verify the merchant relationship.                                        |
| verificationUri          | Opt | string                                           | The URI that will be used to verify the merchant relationship for the potential payee when verification is required.                                   |
| dormantAccount           | Opt | boolean                                          | Indicates if the payee account is considered dormant. These are accounts that have not been active, with a balance or had a payment for over a year.   |
| legacyPayeeId            | Opt | string                                           | Reserved for future use.                                                                                                                               |
| self                     | Req | string                                           | The URI to access the potential payee directly.                                                                                                        |
| id                       | Req | string                                           | The ID of the potential payee.                                                                                                                         |

#### VerificationToken

<table>
<colgroup>
<col style="width: 18%" />
<col style="width: 4%" />
<col style="width: 12%" />
<col style="width: 64%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Req</th>
<th>Data Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>description</td>
<td>Req</td>
<td>string</td>
<td>Description of an element that is part of the verification of the
merchant relationship. Max length: 1000<br />
Pattern: [a-zA-Z0-9_()\r|\n{}@!+#.$,%^ *-]{1,1000}$</td>
</tr>
<tr class="even">
<td>value</td>
<td>Cond</td>
<td>string</td>
<td><p>Value of the element. Max length: 1000<br />
Pattern: [a-zA-Z0-9_()\r|\n{}@!+#.$,%^ *-]{1,1000}$<br />
<br />
Condition: Required on input when passing in a verification token.</p>
<p>For the Potential Payees Get API, this defaults to null unless
specific data should be pre-populated.</p></td>
</tr>
<tr class="odd">
<td>type</td>
<td>Opt</td>
<td>string</td>
<td>The type of this value to provide a clear indication to the UI.
Valid values: String, Number, Currency</td>
</tr>
</tbody>
</table>

# Appendix A: Sample Use Case Implementation

## Making a Bill Payment

Use the CheckFree Next APIs to enable a custom UI to accept bill
payments for one or more businesses and submit the payments for
processing to the Fiserv payment services. Follow this recommended
sequence for the best results:

1.  [Authenticate](#authenticate)- Authenticate to get the access token
    and the refresh token.

2.  [Get User Info](#get-user-information): Get the details of the
    consumer including the contact information.

3.  [Get Payees](#get-payees-list): Get a list of businesses that the
    consumer has added as payees.

4.  [Get Bank Accounts](#get-bank-accounts-for-a-consumer-list): Get a
    list of accounts available for a consumer to fund a bill payment.

5.  [Get Transaction Calendar](#get-transaction-calendar): Get a list of
    all available dates to deliver a payment to the selected payee along
    with associated fees

6.  [Create a Transaction](#create-a-transaction): Schedule one or more
    payments from the consumer to the selected payee

>**Tip:**
After you follow a link, press Alt + Left Arrow (Windows) to go back to
the previous location.

## Find Bills Through Bill Discovery

CheckFree Next enables your end users to find their bills automatically
using the information from their credit report and/ or biller direct
statements and pay them conveniently without having to start from a
blank slate. After consumer eligibility has been confirmed, the UI must
record an explicit consent from the consumer in order to pull data from
credit bureaus and/or biller direct sources. After the consumer gives
consent, the API will return all found bills as a list. The UI should
display all results to the consumer and enable the consumer to select
one or more of the potential payees from the list to add as payees to
the bill pay. Payees added via bill discovery also may deliver bill
summaries to the consumer without the consumer having to sign up for
e‑bills.

1.  [Authenticate](#authenticate) – Authenticate to get the access token
    and the refresh token.

<!-- -->

2.  [Get Consumer Eligibility for Bill
    Discovery](#get-consumer-eligibility-for-bill-discovery) – If the
    sponsor is configured with eligibility rules, determine if the
    consumer is eligible for bill discovery and if there are outstanding
    potential payees.

3.  [Record User Consent](#record-users-consent-information) – If the
    consumer is eligible for bill discovery, capture the consent from
    the consumer for finding bills from bureau and billers.

4.  [Find Bills](#get-potential-payees-list) – Use the GET Potential
    Payees API to get the suggested billers for the consumer once the
    consumer has given the consent and has at least one outstanding
    potential payee. This will not return any results if the consent was
    either not provided by the consumer or the UI has not sent the same
    to the API. Display all potential payees to the consumer to select
    one or more for adding as payees.

5.  [Verify Potential Payee](#verify-potential-payee) – Once the
    consumer has selected one or more potential payees to add, use this
    API to add the billers as Payees and make them available to make a
    payment.

For more information, see the *Bill Discovery API Integration Guide*.

# 

# Appendix B: Response Codes

The attached spreadsheet contains the response codes and corresponding
messages that can be returned.

image.png

When a 0101 error (or equivalent) is returned, Fiserv returns the
parameter **field** in ResultInfo that contains the name of the field
that is missing or in error, if applicable. Fiserv also returns (if
applicable) the parameter **fieldPath** in ResultInfo that contains the
tags that the field causing the error is nested in.

For some requests, each item in a list has a unique ID so that the
response can be tied to the requested item in the list. If the request
resulted in an error in a list, the parameter **listItemId** tells the
caller which item in the list was in error.

POST Payees Example: If the ZIP Code is invalid, this is a sample
excerpt from the response:

    "result": {  
        "success": false,  
        "resultInfo": \[  
            {  
                "resultCategory": "Error",  
                "code": "0101",  
                "field": "ZipCode",  
                "fieldPath": "PayeeInfo.Address.ZipCode",  
                "listItemId": "1",  
                "description": "Invalid ZipCode. Expected 5, 9, or 11
digits."  
            }  
        \]  
    }

Note: If a ZIP Code is found to be invalid after USPS standardization
(code-1 validation), the error 1208 is returned and should be handled
the same way as the 0101 error.

The field that caused the error is specified as ZipCode, while the path
(fieldPath) shows that ZipCode is contained in PayeeInfo.
