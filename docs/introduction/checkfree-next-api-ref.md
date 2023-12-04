# CheckFree<sup>®</sup> Next<sup>℠</sup>

API Reference

November 28, 2023

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

[Messages](#messages)

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
required/conditionally required.

| Claim | Description |
|-------|-------------|
| iss   | Issuer of the JWT |
| aud   | Audience for the JWT |
| iat   | Issue time for the JWT |
| exp   | Time that the JWT is set to expire |
| jti   | Nonce (unique identifier for the JWT) |
| fiserv.identity.billpay.sponsorId | Sponsor ID. Required if subscriberId or externalSubscriberId is provided. |
| fiserv.identity.billpay.subscriberId | Subscriber ID. Provide one of the following for a consumer: <br> - sponsorId AND subscriberId <br> - sponsorId AND externalSubscriberId <br> - checkfreeNext.userId |
| fiserv.identity.billpay.externalSubscriberId | External Subscriber ID. Provide one of the following for a consumer: <br> - sponsorId AND subscriberId <br> - sponsorId AND externalSubscriberId <br> - checkfreeNext.userId |
| fiserv.identity.billpay.originator | Originator of the request. Valid values: Subscriber, Sponsor |
| fiserv.identity.billpay.channel | Channel for the request. Valid values: Desktop, Mobile |
| fiserv.identity.checkfreeNext.userId | CheckFree Next user ID. Provide one of the following for a consumer: <br> - sponsorId AND subscriberId <br> - sponsorId AND externalSubscriberId <br> - checkfreeNext.userId |

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
"fiserv.identity.billpay.channel":"Desktop"  
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
"refresh_token":"d09234ab17c16af6d86cee5355cc91ca7fc4a4070bc80631a8fd7e857ade837f"
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
"refresh_token":"d09234ab17c16af6d86cee5355cc91ca7fc4a4070bc80631a8fd7e857ade837f"
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

| API Name        | Description            | Supported Operations |
|----------|-------------------------------|----------------|
| [Health](#health) | Perform health checks. | GET                  |
| [Users](#users)   | Provide access to a consumer’s details. | GET, POST, PATCH, DELETE for managed users <br> GET, PATCH for consumers <br> GET, POST for Consents sub-resource <br> GET for features <br> GET for sensitiveInformation <br> POST for billDeliveryFailure sub-resource |
| [PotentialPayees](#potential-payees) | Determine Bill Discovery eligibility and provide access to all potential payees found for a consumer. | GET, PUT <br> GET for eligibility sub-resource <br> GET for payeeAccountNumber sub-resource |
| [Payees](#payees) | Provide access to all payees added by a consumer within bill pay. | GET, POST, PATCH, DELETE <br> GET for payeeAccountNumber sub-resource <br> GET for automaticTransactionOptions sub‑resource |
| [PayeeGroups](#payee-groups) | Provide access to payee groups for a consumer. | GET, POST, PUT, DELETE |
| [Merchants](#merchants) | Provide access to all merchants in the managed merchant directory. | GET |
| [ProbableMerchants](#probable-merchants) | Provide access to probable merchants for a consumer. | GET |
| [Reminders](#reminders) | Provide access to reminder models for a consumer. | GET, POST, PATCH, DELETE |
| [ToDos](#todos) | Provide access to all to-do items for a consumer. | GET, PATCH |
| [Bills](#bills) | Provide access to all bills due for a consumer. | GET, PATCH <br> GET for detail sub-resource |
| [eBill Activation](#e-bill-activation) | Provide access to e-bill capability for a payee. | GET, POST, DELETE <br> PATCH for SuppressPaper sub-resource <br> GET for ebillCapability |
| [BankAccounts](#bankaccounts) | Provide access to all bank accounts set up for use with bill pay for a consumer. | GET, POST, PATCH, DELETE for managed users <br> GET, PATCH for consumers <br> GET for accountNumber sub-resource <br> GET for financialInstitution |
| [Card Accounts](#cardaccounts) | Provide access to all card accounts set up for use with bill pay for a consumer. | GET, POST, PATCH, DELETE <br> GET for CardAccountAddLimits sub-resource |
| [TransactionCalendar](#transactioncalendar) | Provide access to the transaction calendar applicable to a payee. | GET |
| [Transactions](#transactions) | Provide access to all transactions for a consumer. | GET, POST, PATCH |
| [AutomaticTransactions](#automatictransactions) | Provide access to all automatic payment plans for a payee. | GET, POST, PATCH, DELETE |
| [Messages](#messages) | Provide access to a consumer’s messages. | GET, POST, DELETE |

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

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| billingClass | Cond | body | string | Length: 1-9 <br> Pattern: ^[a-zA-Z0-9]+$ <br> If supplied, this field must contain a valid alphanumeric billing class as supplied by Fiserv during implementation. Condition: This field is required if Fiserv collects consumer fees on behalf of the client. |
| businessName | Opt | body | string | Length: 1-40 <br> Name of the business. Provide businessName if this is a business. |
| consumerTier | Opt | body | string | Tier level of consumer. Valid values: <br> 1 (default) <br> 2 <br> 3 <br> Pattern: (1\|2\|3){1}$ | 
| isAllowedToSolicit | Opt | body | boolean | Indicates whether or not the consumer may be solicited by Fiserv. This field allows a client to comply with state laws permitting a resident to prohibit solicitation in writing or by telephone. Valid values: <br> true – Yes, Fiserv can send information about additional products or services to this consumer. <br> false – No, Fiserv may not send solicitation information. This is the default. | 
| isEmployee | Opt | body | boolean | Identifies whether a consumer is an employee of the client. Valid values: <br> true – Yes, an employee. <br> false – No, not an employee. This is the default. |
| sponsorBillingCategory | Cond | body | string | Length: 1–2 (only alphanumeric characters allowed) <br> Pattern: ^[a-zA-Z0-9]+$ <br> Clients that are doing their own billing but want to differentiate between consumers can use this field to communicate the consumer category to Fiserv. Condition: Based on sponsor setup. |
| securityQuestion | Cond | body | string | Security question prompt. <br> Valid values: YourHighSchoolName, YourMothersMaidenName, YourFathersMiddleName, YourCityOfBirth <br> Condition: If Fiserv is providing first-tier Customer Care support for the client, securityQuestion and securityAnswer are required at enrollment. | 
| securityAnswer | Cond | body | string | Length: 4-32 <br> An identifier used by the consumer for security purposes. <br> Condition: If Fiserv is providing first-tier Customer Care support for the client, securityQuestion and securityAnswer are required at enrollment. |
| password | Opt | body | string | Length: 8 <br> Consumer's password. <br> Pattern:((?=.\*\[@#\$&\*\])(?=.\*\[a-zA-Z\\d\])\|(?=.\*\\d)(?=.\*\[a-zA-Z\])(?=.\*[a-zA-Z0-9\@#\$&\*\])\|(?=.\*\[a-z\])(?=.\*\[A-Z\])(?=.\*\[a-zA-Z0-9@#\$&\*\]))\[a-zA-Z0-9@#\$&\*\]+ <br> Character types allowed: Uppercase letters, lowercase letters, numbers, and special/punctuation characters (i.e., @#$&amp;\*) <br> Character type required combinations: Must contain at least two of the four character types. For example, the password "a4fs2jkl" contains two character types (lowercase letters and numbers), and thus meets the standard. <br> Specific Restrictions: Password CANNOT contain the user ID anywhere within it. |
| fiservBillpaySubscriberId | Req | body | string | Used by the user to authenticate with the user’s product(s). Length: 2–32 (no spaces allowed) <br> Pattern: ^\[A-Z0-9_(){}&amp;@!+#.'$,%^\*-\]\* <br> Best practices: This ID should contain at least nine characters. Do not include special characters (such as # or &amp;). Do not use a Social Security number. |
 | fiservBillpayExternalSubscriberId | Req | body | string | A unique customer ID, different from the fiservBillpaySubscriberId, that is used for logging the user in to a Fiserv product. Length: 2–48 (no spaces allowed) <br>  Pattern: ^\[a-zA-Z0-9_(){}&amp;@!+#.'$,%^\*-\]\* <br>  Condition: Required if any of the consumer’s products require it. <br> Best practices: This ID should contain at least nine characters. Do not include special characters (such as # and &amp;). Do not use a Social Security number. |
 | name | Req | body | [Name](#name) | Consumer’s name. |  
 | contactEndPoints | Req | body | [Contact Endpoint](#contactendpoint) | Contact details of the consumer, such as the consumer’s email address, phone numbers, and address. |
 | identityValidationInformation | Opt | body | [IdentityValidation Information](#identityvalidationinformation) | Information that may be used to help confirm the consumer’s identity. |
 | birthDate | Req | body | string | The date of birth of the consumer. The user must be 18 years of age or greater. <br> Format: yyyy-MM-dd |
 | category | Opt | body | string | Client-assigned free-form category that identifies the group the consumer belongs to. Length: 1-32 <br> Pattern: ^\[a-zA-Z0-9_(){}&amp;@!+#.'$,%^ \*-\]\* |
 | taxId | Req | body | string | This is the consumer’s ID for tax purposes, either the user’s Social Security number or the Federal Tax Identification number for their business. Max length: 9 <br> Pattern: ^(?!111111111\|222222222\|333333333\|444444444\|555555555\|777777777\|888888888\| <br> 123456789\|012345678)(?!666\|000\|9\d{2})\d{3}(?!00)\d{2}(?!0{4})\d{4}$ <br> Cannot contain special characters, spaces, or hyphens. <br> Must never contain all zeros in any group (000######, ###00####, #####0000). <br> Must never contain 666 or 900–999 in the first digit group (666######, 900######, 901######, etc.). <br> Must not be all repeated digits or a descending sequence. |
 | locale | Opt | body | string | The language code and associated language location in one combined field. Format: XX-XX (language code and language country separated by hyphen). <br> Valid values for language code: “EN”,”ES” <br> Pattern: \[A-Za-z\]{2}-\[A-Za-z\]{2}$ <br>  “US” is the default value for language country. |
 | occupation | Opt | body | string | The activity a user spends time performing to earn a living. Length: 1-50 <br> Pattern: ^\[a-zA-Z0-9_(){}&amp;@!+#.'$,%^ \*-\]\* |
 | userTimeZone | Cond | body | integer | The time zone for the consumer. Defaults to “-05” for eastern standard time (EST) unless a value is provided. <br> Condition: If Fiserv provides first-tier Customer Care for the client, this field is required. |

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

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| userId | Req | path | string | Identifier for the managed user. |
| idType | Opt | query | string | Identifies the user ID type. Valid values: SubscriberId, ExternalSubscriberId, CheckFreeNextUserId <br> CheckFreeNextUserId is the default. |
| password | Opt | body | string | Length: 8 <br> Consumer's password. <br> Pattern:((?=.\*\[@#\$&\*\])(?=.\*\[a-zA-Z\d\])\|(?=.\*\d)(?=.\*\[a-zA-Z\])(?=.\*\[a-zA-Z0-9@#\$&\*\])\|(?=.\*\[a-z\])(?=.\*\[A-Z\])(?=.\*\[a-zA-Z0-9@#\$&\*\]))\[a-zA-Z0-9@#\$&\*\]+ <br> Character types allowed: Uppercase letters, lowercase letters, numbers, and special/punctuation characters (i.e., @#\$&\*) <br> Character type required combinations: Must contain at least two of the four character types. For example, the password "a4fs2jkl" contains two character types (lowercase letters and numbers), and thus meets the standard. <br> Specific Restrictions: Password CANNOT contain the user ID anywhere within it. |
| billingClass | Cond | body | string | Length: 1-9 <br> Pattern: ^\[a-zA-Z0-9\]+$ <br> If supplied, this field must contain a valid alphanumeric billing class as supplied by Fiserv during implementation. Condition: This field is required if Fiserv collects consumer fees on behalf of the client. |
| businessName | Opt | body | string | Length: 1-40 <br> Name of the business. Provide businessName if this is a business. |
| consumerTier | Opt | body | string | Tier level of consumer. Valid values: <br> 1 <br> 2 <br> 3 <br> Pattern: (1\|2\|3){1}$ |
| isAllowedToSolicit | Opt | body | boolean | Indicates whether or not the consumer may be solicited by Fiserv. This field allows a client to comply with state laws permitting a resident to prohibit solicitation in writing or by telephone. Valid values: <br> true – Yes, Fiserv can send information about additional products or services to this consumer. <br> false – No, Fiserv may not send solicitation information. This is the default. |
| isEmployee | Opt | body | boolean | Identifies whether a consumer is an employee of the client. Valid values: <br> true – Yes, an employee. <br> false – No, not an employee. |
| sponsorBillingCategory | Cond | body | string | Length: 1–2 (only alphanumeric characters allowed) <br> Pattern: ^\[a-zA-Z0-9\]+$ <br> Clients that are doing their own billing but want to differentiate between consumers can use this field to communicate the consumer category to Fiserv. Based on client setup. |
| name | Opt | body | [Name](#name) | Consumer’s name. |
| contactEndPoints | Opt | body | [Contact Endpoint](#contactendpoint) | Contact details of the consumer, such as the consumer’s email address, phone numbers, and address. |
| birthDate | Opt | body | string | The date of birth of the consumer. The user must be 18 years of age or greater. <br> Format: yyyy-MM-dd |
| category | Opt | body | string | Client-assigned free-form category that identifies the group the consumer belongs to. Length: 1-32 <br> Pattern: ^\[a-zA-Z0-9_(){}&amp;@!+#.'$,%^ \*-\]\* |
| identityValidationInformation | Opt | body | [IdentityValidation Information](#identityvalidationinformation) | Information that may be used to help confirm the consumer’s identity. |
| locale | Opt | body | string | The language code and associated language location in one combined field. Format: XX-XX (language code and language country separated by hyphen). <br> Valid values for language code: “EN”,”ES” <br> Pattern: \[A-Za-z\]{2}-\[A-Za-z\]{2}$ <br> “US” is the default value for language country. |
| occupation | Opt | body | string | The activity a user spends time performing to earn a living. Length: 1-50 <br> Pattern: ^\[a-zA-Z0-9_(){}&amp;@!+#.'$,%^ \*-\]\*  |
| taxId | Opt | body | string | This is the consumer’s ID for tax purposes, either the user’s Social Security number or the Federal Tax Identification number for their business. Max length: 9 <br> Pattern: ^(?!111111111\|222222222\|333333333\|444444444\|555555555\|777777777\|888888888\|123456789\|012345678) <br> (?!666\|000\|9\d{2})\d{3}(?!00)\d{2}(?!0{4})\d{4}$ <br> Cannot contain special characters, spaces, or hyphens. <br> Must never contain all zeros in any group (000######, ###00####, #####0000). <br> Must never contain 666 or 900–999 in the first digit group (666######, 900######, 901######, etc.). <br> Must not be all repeated digits or a descending sequence. |
| timeZone | Opt | body | integer | The time zone for the consumer. Defaults to “-05” for eastern standard time (EST) unless a value is provided. <br> If Fiserv provides first-tier Customer Care for the client, this field is required. |

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

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| name | Opt | body | [Name](#name) | Consumer’s name. |
| contactEndPoints | Opt | body | [Contact Endpoint](#contactendpoint) | Contact details of the consumer, such as the consumer’s email address, phone numbers, and address. |
| birthDate | Opt | body | string | The date of birth of the consumer. The user must be 18 years of age or greater. <br> Format: yyyy-MM-dd |
| category | Opt | body | string | Client-assigned free-form category that identifies the group the consumer belongs to. Length: 1-32 <br> Pattern: ^\[a-zA-Z0-9_(){}&amp;@!+#.'$,%^ \*-\]\* |
| identityValidationInformation | Opt | body | [IdentityValidation Information](#identityvalidationinformation) | Information that may be used to help confirm the consumer’s identity. |
| locale | Opt | body | string | The language code and associated language location in one combined field. Format: XX-XX (language code and language country separated by hyphen). <br> Valid values for language code: “EN”,”ES” <br> Pattern: \[A-Za-z\]{2}-\[A-Za-z\]{2}$ <br> “US” is the default value for language country. |
| occupation | Opt | body | string | The activity a user spends time performing to earn a living. Length: 1-50 <br> Pattern: ^\[a-zA-Z0-9_(){}&amp;@!+#.'$,%^ \*-\]\* |
| taxId | Opt | body | string | This is the consumer’s ID for tax purposes, either the user’s Social Security number or the Federal Tax Identification number for their business. Max length: 9 <br> Pattern: ^(?!111111111\|222222222\|333333333\|444444444\|555555555\|777777777\|888888888\| <br> 123456789\|012345678)(?!666\|000\|9\d{2})\d{3}(?!00)\d{2}(?!0{4})\d{4}$ <br> Cannot contain special characters, spaces, or hyphens. <br> Must never contain all zeros in any group (000######, ###00####, #####0000). <br> Must never contain 666 or 900–999 in the first digit group (666######, 900######, 901######, etc.). <br> Must not be all repeated digits or a descending sequence. |
| timeZone | Opt | body | integer | The time zone for the consumer. Defaults to “-05” for eastern standard time (EST) unless a value is provided. <br> If Fiserv provides first-tier Customer Care for the client, this field is required. |

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

| Parameter | Req | Param Type | Data Type | Description | 
|-----------|-----|------------|-----------|-------------|
| userId | Req | path | string | Identifier for the user. |
| idType | Opt | query | string | Identifies the user ID type. Valid values: SubscriberId, ExternalSubscriberId, CheckFreeNextUserId <br> CheckFreeNextUserId is the default. | 

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

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| userId | Req | path | string | Identifier for the managed user. | 
| idType | Opt | query | string | Identifies the user ID type. Valid values: SubscriberId, ExternalSubscriberId, CheckFreeNextUserId <br> CheckFreeNextUserId is the default. |

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

| Parameter | Req | Data Type | Description |
|-----------|-----|-----------|-------------|
| billDiscoveryUserConsent | Req | boolean | Indicates if the user has given consent to the retrieval of bill information via Bill Discovery. <br> Will be set to true when the: <br> - Consumer has explicitly given consent for Bill Discovery. <br> Will be set to false when the: <br> - Consumer has explicitly refused consent for Bill Discovery. <br> - Consumer has no consent record to be returned. |
| creditBureauUserConsent | Req | boolean | Indicates if the user has given consent to the retrieval of bill information via credit bureau. <br> Will be set to true when the: <br> - Consumer has explicitly given consent to access credit bureau information for Bill Discovery. <br> Will be set to false when the: <br> - Consumer has explicitly refused consent to access credit bureau information for Bill Discovery. <br> - Consumer has no consent record to be returned |
| result | Req | [ResultType](#resulttype) | Result information. | 

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

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| id | Req | path | string | Identifier for the potential payee. | 
| verificationTokens | Cond | body | Array of [VerificationToken](#verificationtoken) | Array of verification token information provided by the consumer to verify the merchant relationship. <br> Condition: One or more verification tokens are required for this payee. If no verification tokens are required, this array is not required. |

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

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| returnInactivePayees | Opt | query | boolean | Indicates whether to return inactive payees in the response. <br> True – Return inactive payees <br> False – Do not return inactive payees. This is the default. |

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

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| overrideAddressValidation | Opt | query | boolean | Indicates whether address validation (city, state, ZIP Code, address combination) must be done or can be skipped. <br> True - Address validation will be skipped. <br> False - Address validation will be done. This is the default. |
| payeeInfo | Req | body | [PayeeInfo](#payeeinfo) | Payee information. |
| sourceUri | Cond | body | string | The URI from Potential Payees/ Merchant Search/ Payees / ProbableMerchants. <br> Blank for an unmanaged merchant.<br> Condition: If adding a payee by providing only name, account number, and sourceUri, this is required. (Fiserv must already have a relationship with the merchant.) |
| sourceUriPayeeZipCode | Cond | body | string | The payee ZIP Code. This is the ZIP Code that the consumer sees on their bill for the merchant. <br> Conditions: If the /api/v1/merchants/Search API returns a value of true for merchantZipRequired, this is required. If the /api/v1/me/probableMerchants API returns a value of true for merchantZipRequired, this is required. <br> Pattern: ^(\d{5}\|\d{9}\|\d{11})$ <br> Valid characters: 0–9 <br> Parsed: Chars 1-5 = Zip5, Chars 6-9 = Zip4, Chars 10-11 = Zip2 |

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

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| payeeId | Req | path | string | Identifier for the payee. |
| modifyPendingPayments | Opt | query | boolean | Indicates if the payee modification should be propagated to pending payments. Valid values: <br> true – Yes, propagate payee modification to pending payments. <br> false – No, do not propagate payee modification to pending payments. This is the default. |
| name | Opt | body | string | The name of the payee. <br> Pattern: ^\[\x20-\x5A\x5C\x5F-\x7E\]+$ <br> Length: 2–32 |
| nickname | Opt | body | string | The nickname of the payee. Length: 0-30 <br> Pattern: ^\[\x20\x2C-\x2E\x30-\x39\x41-\x5A\x61-\x7A\r\n\]+$ |
| accountNumber | Opt | body | string | The consumer’s account number with the payee. Length: 1–32 <br> Pattern: ^\[a-zA-Z0-9 !"#\$%&amp;-\]{1,32}$ |
| contactPhoneNumber | Opt | body | string | Phone number used to contact the payee if there are issues posting the payment. Length: 10-12 <br> Pattern:(^\[0-9\]{10}\$)\|(^\(?\[0-9\]{3}\\)?-\[0-9\]{3}-\[0-9\]{4}\$)\|(^\\(?\[0-9\]{3}\\)?\\s?\[0-9\]{3}-\[0-9\]{4}\$) <br> Must be numeric and may contain a dash or space between the numbers at the appropriate placement. The phone number must be valid based on the North American Numbering Plan (for example, the area code cannot begin with a 0 or 1). Example: 234-555-1212 | 
| address | Opt | body | [USAddress](#usaddress) | Payee address information. | 
| overnightAddress | Opt | body | [USAddress](#usaddress) | Address for overnight payments if different from the address. | 
| socialTokens | Opt | body | Array of SocialToken | Reserved for future use. |
| accountTokens | Opt | body | Array of [AccountTokenAddInfo](#accounttokenaddinfo) | Reserved for future use. | 

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

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| payeeId | Req | path | string | Identifier for the payee. | 
| cancelPendingTransactions | Opt | query | boolean | Indicates if pending transactions to the payee should be canceled. Valid values: <br> true – Yes, cancel pending transactions. <br> false – No, do not cancel pending transactions. This is the default. |

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

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| isVisible | Req | body | boolean | Indicates whether whether the payee group will be displayed. <br> True - Payee group will be displayed. This is the default. <br> False - Payee group will not be displayed. |
| name | Req | body | string | Unique name for the payee group to be added. Length: 1-32 <br> Pattern: ^\[\x20-\x5A\x5C\x5F-\x7E\]+$ |
| payeeGroupInfo | Opt | body | Array of [PayeeGroupPayeeItem](#payeegrouppayeeitem) | A list of payees to assign to the group. Any payees found to be invalid will not be added to the group. A warning will be returned in this case. |

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

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| id | Req | path | string | Identifier for the payee group. |
| isVisible | Req | body | boolean | Indicates whether whether the payee group will be displayed. <br> True - Payee group will be displayed. This is the default. <br> False - Payee group will not be displayed. |
| name | Req | body | string | Unique name for the payee group. Length: 1-32 <br> Pattern: ^\[\x20-\x5A\x5C\x5F-\x7E\]+$ |
| payeeGroupInfo | Opt | body | Array of [PayeeGroupPayeeItem](#payeegrouppayeeitem) | A list of payees to assign to the group. Any payees to be added to or retained in the group should be provided in this array. <br> Note that the following actions will remove **all** payees from a group: <br> - Submitting an empty array <br> - Submitting an array specifying “null” <br> - Not submitting an array. <br> Any payees found to be invalid will not be added to the group. A warning will be returned in this case. | 

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

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| name | Req | query | string | Length: 3–32 <br> The name of the merchant name to be found. The search characters entered shall be those contained within the merchant name (not a “starts with” search). |
| returnLocalMerchants | Opt | query | boolean | Indicates whether to return merchants that are relevant to the consumer’s ZIP Code. <br> True – If there is no match, return local merchants in the response. If there is a match or partial match, return those matches only. Default is true. <br> False – If no matches are found, do not return any local merchants. | 

### Response

| Parameter | Req | Data Type | Description |
|-----------|-----|-----------|-------------|
| data | Req | Array of [Merchant](#merchant) | List of merchants found based on the provided name search criteria and the returnLocalMerchants flag. Merchants matched are returned in the following order: <br> 1. Exact matches <br> 2. Merchants containing the search criteria <br> If no matches are found based upon the entered search criteria, merchants relevant to the consumer’s ZIP Code will be returned if the returnLocalMerchants flag is true. |
| result | Req | [ResultType](#resulttype) | Result information. |

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

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| alertAtLeadTimeBeforePaymentDueDate | Req | body | boolean | Indicates if the consumer is reminded at lead time (see leadTime) before a payment is due.<br> Valid values: <br> true – Remind the consumer at lead time before a payment is due. <br> false – Do not remind the consumer at lead time before a payment is due. | 
| alertIfNoBillPaymentMadeByDueDate | Req | body | boolean | Indicates if the consumer is reminded if no payment has been made by the due date. <br> Valid values: <br> true – Remind the consumer that a payment is due. <br> false – Do not remind the consumer that a payment is due. |
| alertWhenBillPaymentProcesses | Req | body | boolean | Indicates if the consumer is notified when a payment is processed. <br> Valid values: <br> true – Notify the consumer when a payment has been marked as processed. <br> false – Do not notify the consumer when a payment has been marked as processed. |
| amount | Req | body | number | The payment amount associated with this reminder. <br> Pattern: ^\\d+(\\.\\d{1,2})?$ |
| firstReminderDate | Req | body | string | Date for the first bill payment reminder in yyyy-MM-dd format. The first date of a reminder should be greater than today plus the number of lead days requested. |
| frequency | Req | body | string | The frequency of how often the reminder is automatically created. Valid values: <br> Weekly (should not be used in combination with leadTime of 10, 14, 21, or 28 days) <br> Every2Weeks (should not be used in combination with leadTime of 14, 21, or 28 days) <br> TwiceAMonth (should not be used in combination with leadTime of 21 or 28 days) <br> Every4Weeks <br> Monthly <br> Every2Months <br> Every3Months <br> Every4Months <br> Every6Months <br> Annually | 
| leadTime | Req | body | string | Number of days (excluding any risk management calculations) before the due date that the reminder will be created. Valid values: <br> Lead03Days <br> Lead05Days <br> Lead10Days (should not be used in combination with frequency Weekly) <br> Lead14Days (should not be used in combination with frequency Weekly or Every2Weeks) <br> Lead21Days (should not be used in combination with frequency Weekly, Every2Weeks, or TwiceAMonth) <br> Lead28Days (should not be used in combination with frequency Weekly, Every2Weeks, or TwiceAMonth) |
| payeeUri | Req | body | string | URI for the payee. This matches the payee URI returned in GET Payees. |

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

| Parameter | Req | Param Type | Data Type | Description | 
|-----------|-----|------------|-----------|-------------|
| id | Req | path | string | Identifier for the reminder model. |
| alertAtLeadTimeBeforePaymentDueDate | Opt | body | boolean | Indicates if the user is reminded at lead time (see LeadTime) before a payment is due. <br> Valid values: <br> true – Remind the user at lead time before a payment is due. <br> false – Do not remind the user at lead time before a payment is due. |
| alertIfNoBillPaymentMadeByDueDate | Opt | body | boolean | Indicates if the user is reminded if no payment has been made by the due date. <br> Valid values: <br> true – Remind the user that a payment is due. <br> false – Do not remind the user that a payment is due. |
| alertWhenBillPaymentProcesses | Opt | body | boolean | Indicates if the user is notified when a payment is processed. <br> Valid values: <br> true – Notify the user when a payment has been marked as processed. <br> false – Do not notify the user when a payment has been marked as processed. |
| amount | Opt | body | number | The payment amount associated with this reminder. <br> Pattern: ^\\d+(\\.\\d{1,2})?$ |
| firstReminderDate | Opt | body | string | First date for the bill payment reminder in yyyy-MM-dd format. The first date of a reminder should be greater than today plus the number of lead days requested. |
| frequency | Opt | body | string | The frequency of how often the reminder is automatically created. Valid values: <br> Weekly (should not be used in combination with leadTime of 10, 14, 21, or 28 days) <br> Every2Weeks (should not be used in combination with leadTime of 14, 21, or 28 days) <br> TwiceAMonth (should not be used in combination with leadTime of 21 or 28 days) <br> Every4Weeks <br> Monthly <br> Every2Months <br> Every3Months <br> Every4Months <br> Every6Months <br> Annually |
| leadTime | Opt | body | string | Number of days (excluding any risk management calculations) before the due date that the reminder will be created. Valid values: <br> Lead03Days <br> Lead05Days <br> Lead10Days (should not be used in combination with frequency Weekly) <br> Lead14Days (should not be used in combination with frequency Weekly or Every2Weeks) <br> Lead21Days (should not be used in combination with frequency Weekly, Every2Weeks, or TwiceAMonth) <br> Lead28Days (should not be used in combination with frequency Weekly, Every2Weeks, or TwiceAMonth) |

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

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| id | Req | path | string | Identifier for the bill. |
| note | Opt | body | string | Optional note entered by the consumer with information about the bill and its resolution. Length: 1-80 |
| reason | Req | body | string | Indicates the method of the bill’s resolution. Valid values: <br> NoneSpecified <br> Bank <br> Check <br> Cash <br> NotPaid <br> Other <br> BillerWebSite <br> Phone <br> Mail <br> Office <br> ZeroBalanceBill <br> ContestedBill |

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

| Parameter | Req | Data Type | Description |
|-----------|-----|-----------|-------------|
| data | Req | [EbillCapability](#ebillcapability) | Information required for e-bill activation. Empty if there is no data to return. |
| result | Cond | [ResultType](#resulttype) | Result information. <br> Condition: Only returned when the request fails. No result content returned for success (HTTP status code 200). (Known issue: result is currently being returned in response for success.) |

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

| Parameter | Req | Data Type | Description |
|-----------|-----|-----------|-------------|
| data | Req | [EbillAccountDetail](#ebillaccountdetail) | E-bill service information. |
| result | Cond | [ResultType](#resulttype) | Result information. <br> Condition: Only returned when the request fails. No result content returned for success. (Known issue: result is currently being returned in response for success.) |

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
| result | Cond | [ResultType](#resulttype) | Result information. <br> Condition: Only returned when the request fails. No content returned for success (HTTP status code 204). |

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

    - [Managed user](#get-bank-accounts-for-managed-user-list)

    - [Consumer](#get-bank-accounts-for-a-consumer-list)

-   Get a specific bank account

    - [Managed user](#get-a-bank-account-for-a-managed-user-single)

    - [Consumer](#get-a-bank-account-for-a-consumer-single)

-   [Add a bank account for a managed
    user](#add-a-bank-account-for-a-managed-user)

-   Update a bank account

    - [Managed user](#update-a-bank-account-for-a-managed-user)

    - [Consumer](#update-a-bank-account-for-a-consumer)

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

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| routingTransitNumber | Req | query | string | Routing and transit number for a bank account. Length: 9 <br> Pattern: ^[0-9]{9}$ |

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

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| userId | Req | path | string | Identifier for the managed user. |
| returnInactiveBankAccounts | Opt | query | boolean | Indicates whether to return inactive bank accounts in the response. <br> True – Return inactive bank accounts <br> False – Do not return inactive bank accounts. This is the default. |
| idType | Opt | query | string | Identifies the user ID type. Valid values: SubscriberId, ExternalSubscriberId, CheckFreeNextUserId <br> CheckFreeNextUserId is the default. |

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
Maintenance](#sample-api-usage-authentication-request-for-user-maintenance).

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
Maintenance](#sample-api-usage-authentication-request-for-user-maintenance).

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
| checkPrintAddress | Opt | body | [USAddress](#usaddress) | The U.S. address printed on the checks. If provided, this is the consumer’s address to be printed in the upper left corner of the check. |
| bankingOptions | Opt | body | [BankingOptions](#bankingoptions) | Banking options when banking service is enabled. |

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
Maintenance](#sample-api-usage-authentication-request-for-user-maintenance).

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
| checkPrintAddress | Opt | body | [USAddress](#usaddress) | The U.S. address printed on the checks. If provided, this is the consumer’s address to be printed in the upper left corner of the check. |
| bankingOptions | Opt | body | [BankingOptions](#bankingoptions) | Banking options when banking service is enabled. |

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
| checkPrintAddress | Opt | body | [USAddress](#usaddress) | The U.S. address printed on the checks. If provided, this is the consumer’s address to be printed in the upper left corner of the check. |
| bankingOptions | Opt | body | [BankingOptions](#bankingoptions) | Banking options when banking service is enabled. |

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
Maintenance](#sample-api-usage-authentication-request-for-user-maintenance).

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

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| startDate | Opt | query | string <br> &lt;date-time&gt; | The desired start date for the list of transactions. Format: yyyy-MM-dd <br> Default is 6 months in the past. |
| endDate | Opt | query | string <br> &lt;date-time&gt; | The desired end date for the list of transactions. Format: yyyy-MM-dd <br> Default is all transactions scheduled in the future. |
| sort | Opt | query | string | Sort the response based on the given input. These are the eligible values with which to sort: amount, deliveryDate, payeeName, deliveryTrackingNumber, confirmationNumber, debitDate, fee, note, memo, deliveryMethod, status, transactionType <br> A negative sign (-) before the parameter indicates that the response should be sorted in descending order. For example, “sort=-amount” |
| start | Opt | query | integer | Specifies the starting record to be fetched. |
| limit | Opt | query | integer | Specifies the number of records to be fetched. |
| payeeUri | Opt | query | string | The URI to the payee that this transaction is associated with. |
| status | Opt | query | string | The status of the payment transaction. Valid values: Pending, Complete, InProcess, Failed Canceled |
| fundingAccountUri | Opt | query | string | The funding account for the transaction. |
| minAmount | Opt | query | double | The minimum transaction amount. |
| maxAmount | Opt | query | double | The maximum transaction amount. | 

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

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| allowDuplicateTransaction | Opt | query | boolean | Indicates that duplicate transactions are allowed. <br> Valid values: <br> true – Allow duplicate transactions <br> false – Check for duplicate transactions. This is the default.<br> A transaction cannot be a duplicate unless the allowDuplicateTransaction flag is true. A duplicate is defined as having the same payee, transaction amount, and transaction date. <br> This does not apply to SameDay Payments. Duplicate transactions are not allowed for SameDay Payments. |
| transaction | Req | body | Array of [Transaction](#transaction) | List of transactions to be scheduled for the consumer. Limited to 30 or fewer per request. |

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

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| transactionId | Req | path | string | Identifier for the transaction. |
| amount | Opt | body | double | The amount of the transaction. <br> Pattern: ^\\d+(\\.\\d{1,2})?$ |
| deliveryDate | Opt | body | string | The date that the payment transaction is to be delivered to its destination in yyyy-MM-dd format. | 
| fundingAccountUri | Opt | body | string | The funding account URI for the transaction. Account types that are eligible to be used are those enabled for Bill Payment (service) for the tenant/sponsor. |
| memo | Opt | body | string | Transaction memo. Maximum of 34 characters. This text will be printed on the check sent to the payee for this payment. A payment memo is only used for payments that are to be processed via a paper check. |
| note | Opt | body | string | A consumer’s “note to self.” This note is not submitted to the payee. Length: 0–255 <br> Pattern: ^[\\x2A-\\x2E\\x30-\\x39\\x40-\\x5A\\x5F\\x5E\\x61-\\x7A\\x20\\x21\\x23-\\x25]+\$ <br> The note field only allows the following character sets: a-z, A-Z, 0-9, _ @!+#.$,%^\*- | 
| withdrawNow | Opt | body | boolean | Reserved for future use. |
| cavv | Opt | body | string | Placeholder for 3-D Secure. Cardholder Authentication Verification Value. Used if the transaction is funded by a card account and the institution is participating in 3-D Secure. |

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

| Parameter | Req | Param Type | Data Type | Description | 
|-----------|-----|------------|-----------|-------------|
| pageSize | Opt | query | integer | Specifies the number of items per page. |
| pageNumber | Opt | query | integer | Specifies the page number from which results should be returned. | 
| returnInactiveAutomaticTransactions | Opt | query | boolean | Indicates whether to return inactive automatic transactions in the response. <br> True – Return inactive automatic transactions <br> False – Do not return inactive automatic transactions. This is the default. |
| destinationUri | Opt | query | string | The destination of the automatic transaction. This is a URI for a payee. | 

### Response

| Parameter | Req | Data Type | Description | 
|-----------|-----|-----------|-------------|
| automaticTransactions | Req | Array of [AutomaticTransactionOutputDetail](#automatictransactionoutputdetail) | List of found automatic transactions. There is an empty array if there is no data to return. | 
| result | Cond | [ResultType](#resulttype) | Result information. Condition: Only returned when the request fails. No content returned for success (HTTP status code 200). |

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

| Parameter | Req | Data Type | Description| 
|-----------|-----|-----------|------------|
| automaticTransactions | Req | [AutomaticTransactionOutputDetail](#automatictransactionoutputdetail) | Details for the automatic payment plan. | 
| result | Cond | [ResultType](#resulttype) | Result information. Condition: Only returned when the request fails. No content returned for success (HTTP status code 200). |

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

| Parameter | Req | Param Type | Data Type | Description | 
|-----------|-----|------------|-----------|-------------|
| fundingAccountUri | Req | body | string | The source funding account URI for the automatic transaction. |
| destinationUri | Req | body | string | The destination of the automatic transaction. This is a URI for a payee. |
| billTransactionSchedule | Cond | body | [BillTransactionSchedule](#billtransactionschedule) | BillTransactionSchedule defines a transaction schedule for an e-bill automatic payment. <br> Condition: Either billTransactionSchedule or recurringTransactionSchedule is required. |
| recurringTransactionSchedule | Cond | body | [RecurringTransactionSchedule](#recurringtransactionschedule) | RecurringTransactionSchedule defines a transaction schedule for a payee. <br> Condition: Either billTransactionSchedule or recurringTransactionSchedule is required. | 
| cavv | Opt | body | string | Placeholder for 3-D Secure. Cardholder Authentication Verification Value. Used if the transaction is funded by a card account and the institution is participating in 3-D Secure. <br> Only applies to the first transaction generated from a recurring transaction schedule (if the transaction is within 90 days). | 

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

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| automaticTransactionId | Req | path | string | Identifier for the automatic transaction. |
| fundingAccountUri | Opt | body | string | The source funding account URI for the automatic transaction. |
| billTransactionSchedule | Opt | body | [BillTransactionSchedulePatch](#billtransactionschedulepatch) | BillTransactionSchedule defines a transaction schedule for an e-bill automatic payment. |
| recurringTransactionSchedule | Opt | body | [RecurringTransactionSchedulePatch](#recurringtransactionschedulepatch) | RecurringTransactionSchedule defines a transaction schedule for a payee. |
| cavv | Opt | body | string | Placeholder for 3-D Secure. Cardholder Authentication Verification Value. Used if the transaction is funded by a card account and the institution is participating in 3-D Secure. <br> Only applies to the first transaction generated from a recurring transaction schedule (if the transaction is within 90 days). |

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

| Parameter | Req | Param Type | Data Type | Description |
|-----------|-----|------------|-----------|-------------|
| automaticTransactionId | Req | path | string | Identifier for the automatic transaction. |
| cancelPendingTransactions | Opt | query | boolean | Indicates if pending transactions should be canceled. <br> true – Yes, cancel pending payments. <br> false – No, do not cancel pending payments. This is the default. <br> The option to cancel pending transactions only applies to automatic transactions that have a RecurringTransactionSchedule. This does not apply to e-bill automatic payments (BillTransactionSchedule). |

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

# Messages

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

| Parameter | Req | Data Type | Description |
|-----------|-----|-----------|-------------|
| billTransactionScheduleActive | Req | boolean | Indicates if a bill transaction schedule is already active (i.e., eBill AutoPay is active) for the payee. |
| billTransactionScheduleCapable | Req | boolean | Indicates when a payee is capable of an automatic transaction schedule for paying ebills (eBill AutoPay). This is true when: <br> - Payee is active. <br> - Payee is e-bill enabled. <br> - Biller supports AutoPay. <br> - The first e-bill has been received. | 
| billTransactionScheduleTypesSupported | Cond | [BillTransactionScheduleTypesSupported](#billtransactionscheduletypessupported) | Condition: Required when billTransactionScheduleCapable is true. <br> Lists the allowable payment schedule types for bill transaction schedule (eBill Autopay). | 
| recurringTransactionScheduleActive | Req | boolean | Indicates if a recurring transaction schedule (one or more) is already set up for the payee. | 
| recurringTransactionScheduleCapable | Req | boolean | Indicates when a payee is capable of an automatic transaction schedule as a recurring model. This is true when the payee is active. |
| recurringTransactionScheduleTypesSupported | Cond | [RecurringTransactionScheduleTypesSupported](#recurringtransactionscheduletypessupported) | Condition: Required when recurringTransactionScheduleCapable is true. <br> Lists the allowable payment schedule types for a recurring transaction schedule. |

#### AutomaticTransactionOutputDetail

| Parameter | Req | Data Type | Description |
|-----------|-----|-----------|-------------|
| modifiableFields | Req | Array of string | List of fields that can be changed for the automatic transaction. | 
| id | Req | string | The identifier of this automatic transaction. |
| self | Req | string | Relative URI pointing to the automatic transaction itself. |
| status | Req | string | The status of the recurring or automatic payment model. Valid values: <br> Active <br> Canceled <br> Completed <br> Active and Canceled apply to both recurring models and e-bill automatic payments. <br> Completed applies to recurring models only. |
| fundingAccountUri | Req | string | The source funding account URI for the automatic transaction. |
| destinationUri | Req | string | The destination of the automatic transaction. This is a URI for a payee. |
| billTransactionSchedule | Cond | [BillTransactionSchedule](#billtransactionschedule) | Returned if available. <br> BillTransactionSchedule defines a transaction schedule for an e-bill automatic payment. |
| recurringTransactionSchedule | Cond | [RecurringTransactionSchedule](#recurringtransactionschedule) | Returned if available. <br> RecurringTransactionSchedule defines a transaction schedule for a payee. |

#### BankAccount

| Parameter | Req | Data Type | Description |
|-----------|-----|-----------|-------------|
| accountType | Req | string | The type of bank account. Valid values: "Checking", "Savings", "InstallmentLoan", "IndividualRetirement", "CommercialLoan", "MoneyMarket", "LineOfCredit", "Brokerage", "SpecialDeposit" |
| maskedAccountNumber | Req | string | The account number in masked form, for presentment in a user interface. For example, “***********4656” |
| nickname | Opt | string | The nickname for the account, if one exists. Otherwise, returns null. <br> Pattern: ^[a-zA-Z0-9_(){}&@!+#.'\$, \%\*-]{1,30}$ |
| accountBalance | Opt | double | The account balance. |
| isPreferred | Req | boolean | Indicates if the account is the preferred account. The preferred account flag indicates the consumer’s preferred choice of bank account used to fund bill payment transactions. |
| unmaskedAccountNumberUri | Req | string | Link to the unmasked account number for the account. |
| routingTransitNumber | Req | string | Routing and transit number for the consumer's bank account. <br> Pattern: ^[0-9]{9}$ |
| isBusiness | Req | boolean | Indicates if this is a business account. True if it is a business account. |
| businessName | Cond | string | When isBusiness is true, this is the name of the business. <br> Pattern: ^[a-zA-Z0-9_(){}&@!+#.'$,%^ \*-]\* |
| primaryAccountOwner | Opt | string | Name of the primary account holder for this account. Used only for individual consumers. <br> Pattern: ^[a-zA-Z0-9_(){}&@!+#.'$,%^ \*-]\* |
| secondaryAccountOwner | Opt | string | Name of the secondary account holder, if applicable. This field allows a consumer to add an additional name on a printed check that appears on the line beneath the name in the PrimaryAccountOwner field. Used only for individual consumers. <br> Pattern: ^[a-zA-Z0-9_(){}&@!+#.'$,%^ \*-]\* |
| confirmationStatus | Opt | string | Account’s verification/confirmation status. Valid values: Confirmed, Failed, Pending, InProgress |
| accountStatus | Opt | string | Account status. Valid values: Active, Inactive, Pending, Rejected |
| checkPrintAddress | Opt | [USAddress](#usaddress) | The U.S. address printed on the checks. If provided, this is the consumer’s address to be printed in the upper left corner of the check. |
| bankingOptions | Opt | [BankingOptions](#bankingoptions) | Banking options when banking service is enabled. |

#### BankAccountAddInfo

| Parameter | Req | Data Type | Description |
|-----------|-----|-----------|-------------|
| routingTransitNumber | Req | string | Routing and transit number for the user’s bank account. Length: 9 <br> Pattern: ^[0-9]{9}$ <br> Must contain a valid number in the format 999999999. |
| accountType| Req | string | The type of bank account. Valid values: "Checking", "Savings", "InstallmentLoan", "IndividualRetirement", "CommercialLoan", "MoneyMarket", "LineOfCredit", "Brokerage", "SpecialDeposit" |
| accountNumber | Req | string | User’s bank account number. Account number as it appears on the MICR line of the user’s paper check. This number is used by the user as the bank account number, and it cannot be changed. Length: 1–22 <br> Pattern: ^[a-zA-Z0-9]{1,22}$ <br> Must contain at least one character. Must contain uppercase characters when alphabetic characters are part of the account number. |

#### BankAccountGetModel

| Parameter | Req | Data Type | Description| 
|-----------|-----|-----------|------------|
| self | Req | string | Relative URI pointing to the object itself. | 
| id | Req | string | Unique identifier of the object. |
| isBilling | Req | boolean | Establishes whether this account is the account from which user/subscriber billing fees (if billed by Fiserv) will be debited. <br> Valid values: <br> true (this account is set to be the account from which billing fees are debited) <br> false (this account is not set to debit billing fees from this account) <br> For a Sponsor/Tenant that does NOT have Fiserv bill the user on their behalf, this value will always be returned as false. |
| modifiableFields | Req | Array of string | List of fields that can be changed for the bank account. |
| accountType | Req | string | The type of bank account. Valid values: "Checking", "Savings", "InstallmentLoan", "IndividualRetirement", "CommercialLoan", "MoneyMarket", "LineOfCredit", "Brokerage", "SpecialDeposit" |
| maskedAccountNumber | Req | string | The account number in masked form, for presentment in a user interface. For example, “***********4656” |
| nickname | Opt | string | The nickname for the account, if one exists. Otherwise, returns null. <br> Pattern: ^[a-zA-Z0-9_(){}&@!+#.'\$, \%\*-]{1,30}$ |
| accountBalance | Opt | double | The account balance. Reserved for future use. |
| isPreferred | Req | boolean | Indicates if the account is the preferred account. The preferred account flag indicates the consumer’s preferred choice of bank account used to fund bill payment transactions. |
| unmaskedAccountNumberUri | Req | string | Link to the unmasked account number for the account. See “[Get an Unmasked Bank Account Number]("#get-an-unmasked-bank-account-number").” |
| routingTransitNumber | Req | string | Routing and transit number for the bank account. Length: 9 <br> Pattern: ^[0-9]{9}$ |
| isBusiness | Req | boolean | Indicates if the account is a business account. True if it is a business account. |
| businessName | Cond | string | When isBusiness is true, this is the name of the business. <br> Pattern: ^[a-zA-Z0-9_(){}&@!+#.'$,%^ \*-]\* |
| primaryAccountOwner | Opt | string | Name of the primary owner of this account.<br> Pattern: ^[a-zA-Z0-9_(){}&@!+#.'$,%^ \*-]\* |
| secondaryAccountOwner | Opt | string | Name of the secondary owner of this account, if applicable. <br> Pattern: ^[a-zA-Z0-9_(){}&@!+#.'$,%^ \*-]\* |
| confirmationStatus | Req | string | Account’s verification/confirmation status. Valid values: Confirmed, Failed, Pending, InProgress |
| accountStatus | Req | string | Account status. Valid values: Active, Inactive, Pending, Rejected, Historical <br> Active – Account verification was successful and account is active <br> Inactive – Account was deleted <br> Pending – Account verification has not been completed <br> Rejected – Account verification was completed and failed <br> Historical – This status is returned when “returnInactiveBankAccounts” is set to true in the request for a list of bank accounts and either: <br> - The consumer’s transaction history has been migrated from one sponsor to another sponsor. Only the transaction history is migrated, not the old bank account. However, calling apps are still able to show the bank account information associated with the transaction history. <br> <br> or <br> <br> - The sponsor supports extended transaction history and this bank account is associated with extended transaction history records. |
| checkPrintAddress | Opt | [USAddress](#usaddress) | The U.S. address printed on the checks. If provided, this is the consumer’s address to be printed in the upper left corner of the check. |
| bankingOptions | Opt | [BankingOptions](#bankingoptions) | Banking options when banking service is enabled. |

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

| Parameter | Req | Data Type | Description |
|-----------|-----|-----------|-------------|
| amountDue | Req | double | The current amount that the consumer owes to the biller. |
| balanceAmountDue | Opt | double | The total outstanding balance for the consumer’s account with the biller. |
| billerInfo | Req | [BillerInfo](#billerinfo) | Details about the biller. |
| billType | Req | string | Type of bill. <br> FromBiller indicates a distributed e-bill. <br> FromBillDueAlert indicates a bill due alert. |
| dateFirstViewed | Cond | string | Date that the e-bill was first viewed. Condition: The e-bill has been viewed. Format: yyyy-MM-dd. |
| destinationUrl | Req | string | The destination URL of the payee. |
| dueDate | Req | string | The date that the biller has indicated that the bill is due. If this date is displayed to the consumer on a UI, the date should be derived using eastern time (ET). Format yyyy-MM-dd. |
| actionByDate | Opt | string | The latest date and time that a transaction must be initiated by in order to be received by the biller on the dueDate. Not returned if the calculated date/time is earlier than the current date/time. |
| dueDateText | Opt | string | Text that describes the biller’s preferred method of expressing the date that payment is due, such as “Due Upon Receipt.” Maximum length: 20 |
| detailUrl | Req | string | Bill detail URL. The bill detail URL expires in a timeframe set by the biller. The default time is 30 minutes but may be as low as 10 minutes. Long-term storage of the URL is not possible because it is only valid for 30 minutes (default) because of encrypted tokens. |
| filedBillInfo | Opt | [FiledBillInfo](#filedbillinfo) | Filed bill details. Only provided if e-bill has been filed. |
| isDetailUrlIframeSupported | Req | boolean | Indicates whether the URL can be displayed in an iFrame. | 
| minimumAmountDue | Opt | double | The minimum amount that the consumer owes to the biller. |
| replacedBillId | Cond | string | The identifier for a replacement e-bill. Condition: The e-bill is a replacement bill. |
| status | Req | string | The current state of the e-bill, such as whether it is paid or unpaid. Valid values: <br> Paid <br> Unpaid <br> PaymentFailed <br> PaymentCanceled <br> Filed |
| useDueDateText | Req | boolean | Specifies whether to use the text provided in dueDateText. Valid values: <br> true (use the text) <br> false (do not use the text) |
| self | Req | string | URI pointing to the e-bill itself. |
| id | Req | string | Unique identifier for the e-bill. |

#### BillDiscoveryUserEligibility

| Parameter | Req | Data Type | Description |
|-----------|-----|-----------|-------------|
| creditReportEligible | Req | boolean | Indicates if the consumer is eligible for a credit report search. |
| billServiceProviderEligible | Req | boolean | Indicates if the consumer is eligible for a bill service provider search. |
| outstandingPotentialPayeesCount | Req | integer | Count of outstanding potential payees (i.e., potential payees that have already been identified and for which no action has been taken by the consumer). |

#### BillTransactionSchedule

| Parameter | Req | Data Type | Description |
|-----------|-----|-----------|-------------|
| amount | Cond | double | The transaction amount. Required if the transactionType is FixedAmount. <br> Pattern: ^\\d+(\\.\\d{1,2})?$ |
| maximumTransactionAmount | Opt | double | The upper limit of the transaction amount. If the delivered bill amount is greater than this value, then the transaction amount will equal the maximumTransactionAmount value. Only applies to TransactionType “AmountDue”, “MinimumAmountDue” or “AccountBalance”. <br> Pattern: ^\\d+(\\.\\d{1,2})?$ |
| scheduleDaysBefore | Cond | integer | Condition: Required if transactionInitiationType is DaysBeforeDueDate. <br> If present, will generate transactions the specified number of business days prior to the normally scheduled date. <br> Valid values: 1, 2, 3, 4, 5 |
| transactionInitiationType | Req | string | Specifies when a transaction should be generated based on bill information. Valid values: <br> DueDate <br> UponReceipt <br> DaysBeforeDueDate |
| transactionType | Req | string | The payment structure for the automatic transaction. Valid values: <br> FixedAmount <br> AmountDue <br> MinimumAmountDue <br> AccountBalance |
| transactionScheduledAlert | Req | boolean | Indicates if the consumer is notified when a transaction is scheduled. <br> True - Notify the consumer when a transaction is scheduled. <br> False - Do not notify the consumer when a transaction is scheduled. This is the default. |
| transactionSentAlert | Req | boolean | Indicates if the consumer is notified when a transaction is sent. <br> True - Notify the consumer when a transaction is processed. <br> False - Do not notify the consumer when a transaction is processed. This is the default. |

#### BillTransactionSchedulePatch

| Parameter | Req | Data Type | Description |
|-----------|-----|-----------|-------------|
| amount | Cond | double | The transaction amount. Required if the transactionType is FixedAmount. <br> Pattern: ^\\d+(\\.\\d{1,2})?$ |
| maximumTransactionAmount | Opt | double | The upper limit of the transaction amount. If the delivered bill amount is greater than this value, then the transaction will be scheduled at this amount. Only applies to TransactionType “AmountDue”, “MinimumAmountDue” or “AccountBalance”. <br> Pattern: ^\\d+(\\.\\d{1,2})?$ |
| scheduleDaysBefore | Cond | integer | Condition: Required if transactionInitiationType is DaysBeforeDueDate. <br> If present, will generate transactions the specified number of business days prior to the normally scheduled date. <br> Valid values: 1, 2, 3, 4, 5 |
| transactionInitiationType | Opt | string | Specifies when a transaction should be generated based on bill information. Valid values: <br> DueDate <br> UponReceipt <br> DaysBeforeDueDate |
| transactionType | Opt | string | The payment structure for the automatic transaction. Valid values: <br> FixedAmount <br> AmountDue <br> MinimumAmountDue <br> AccountBalance |
| transactionScheduledAlert | Opt | boolean | Indicates if the consumer is notified when a transaction is scheduled. <br> True - Notify the consumer when a transaction is scheduled. <br> False - Do not notify the consumer when a transaction is scheduled. This is the default. |
| transactionSentAlert | Opt | boolean | Indicates if the consumer is notified when a transaction is sent. <br> True - Notify the consumer when a transaction is processed. <br> False - Do not notify the consumer when a transaction is processed. This is the default. |

#### BillTransactionScheduleTypesSupported

| Parameter | Req | Data Type | Description |
|-----------|-----|-----------|-------------|
| transactionTypes | Req | Array of string | The payment structure for the automatic transaction. Valid values: <br> FixedAmount <br> AmountDue <br> MinimumAmountDue <br> AccountBalance |
| transactionInitiationTypes | Req | Array of string | Specifies when a transaction should be generated based on bill information. Valid values: <br> DueDate <br> UponReceipt <br> DaysBeforeDueDate |
| eligibleFundingAccounts | Req | Array of [EligibleFundingAccount]("#eligiblefundingaccount") | A list of eligible funding accounts with the available automatic transaction options. |

#### BillerInfo

| Parameter             | Req | Data Type | Description                                                                                                                                                                                                                                                                                                               |
|------------|-----|-------|-------------------------------------------------|
| billerLogoUrl         | Opt | string    | URL of the biller logo. The preferred logo size is 90 × 90 pixels.                                                                                                                                                                                                                                                        |
| billReferenceImageUrl | Opt | string    | URL where the bill reference image (teaser ad image) is stored. Used to display the image on the user interface. As a best practice, the image should be no larger than 80 x 24 pixels.                                                                                                                                   |
| billReferenceLinkUrl  | Opt | string    | URL for the teaser ad that the consumer may follow to receive additional information from the biller. This URL is used in conjunction with either the Bill Reference Image URL or the Bill Reference Text. The consumer can click on either the image or text to be transferred to the biller site specified by this URL. |
| billReferenceText     | Opt | string    | Text that can be used instead of the image link. Using this text may improve performance over downloading an image (teaser ad).                                                                                                                                                                                           |

#### CardAccountAddLimits

| Parameter | Req | Data Type | Description |
|-----------|-----|-----------|-------------|
| rollingPeriodDaysLimit | Req | integer | Number of days for which to enforce a rolling time period limit. |
| rollingPeriodDaysLimitModifiable | Req | boolean | Indicates if the number of days for the rolling time period can be changed. <br> true – Can be changed. <br> false – Cannot be changed. |
| rollingPeriodCardsLimit | Req | integer | Maximum number of card accounts that can be added during the rolling time period. |
| rollingPeriodCardsRemainingCount | Req | integer | Remaining number of card accounts that can be added by the consumer during the current rolling time period. |
| rollingPeriodCardsLimitModifiable | Req | boolean | Indicates if the number of card accounts that can be added during the rolling time period can be changed. <br> true – Can be changed. <br> false – Cannot be changed. |
| sameCardDeleteAddLimit | Req | integer | Number of times a consumer can add the same card account (delete/add again). |
| sameCardDeleteAddLimitModifiable | Req | boolean | Indicates if the number of times a consumer can add the same card account can be changed. <br> true – Can be changed. <br> false – Cannot be changed. |
| activeCardLimit | Req | integer | Maximum number of active card accounts. |
| activeCardRemainingCount | Req | integer | Remaining number of active card accounts that can be added by the consumer. |
| activeCardLimitModifiable | Req | boolean | Indicates if the number of active card accounts that can be added by the consumer can be changed. <br> true – Can be changed. <br> false – Cannot be changed. |

#### CardAccountGetModel

| Parameter | Req | Data Type | Description |
|-----------|-----|-----------|-------------|
| self | Req | string | Relative URI pointing to the object itself. |
| id | Req | string | Unique identifier of the object. | 
| addedBy | Opt | string | Indicates the entity that added the card account. Valid values: <br> Sponsor – Indicates the card was added by the FI <br> Subscriber – Indicates the card was added by the consumer | 
| cardNumberMasked | Req | string | The card account number in masked form. For example, "********1234". |
| modifiableFields | Req | Array of string | List of fields that can be changed for the card. | 
| deleteAllowed | Req | boolean | Indicates whether the originator of this GET call is allowed to delete this card. | 
| cardType | Req | string | Indicates the type of card. Valid values: AmericanExpress, Discover, MasterCard, Visa |
| isDebit | Req | boolean | Indicates if the card is a debit card. |
| billingAddress | Req | [USAddress](#usaddress) | The billing address for the card. | 
| expirationMonth | Req | integer | Month in which the card expires. |
| expirationYear| Req | integer | Year in which the card expires. |
| externalAccountDescription | Req | string | External account description that is free-form text, such as: “Bank Name” Visa | 
| nameOnCard | Req | string | Name printed on the card. | 
| nickname | Opt | string | A description of the card used to help identify it in a list. |

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

| Parameter | Req | Data Type | Description |
|-----------|-----|-----------|-------------|
| status | Req | string | The status of the contact endpoint. <br> Valid values: Unspecified, Active, Inactive, Suspended, NotFound, Invalid, Onhold | 
| endPointType | Req | string | The type of contact endpoint. <br> Valid values: Phone, Email, Address |
| address | Req | [Address](#address) | The address of the consumer. At least one address contact endpoint is required. |
| phone | Req | [Phone](#phone) | Contact phone number for the consumer. At least one phone contact endpoint is required. |
| email | Req | [Email](#email) | Contact email address for the consumer. At least one email contact endpoint is required. <br> Length: 5–100 <br> Pattern: ^([\_+A-Za-z0-9]+((\\.\|\\-)[\_+A-Za-z0-9]+)\*@[A-Za-z0-9]+((\\.\|\\-)[A-Za-z0-9]+)\*(\\.[A-Za-z]{2,8}))$ <br> This field must contain a valid Internet-style address. | 
| defaultEndpoint | Req | boolean | Indicates if the endpoint is considered the default for the user. For example, may be used to indicate the primary email address for the user to receive correspondence. | 

#### EbillAccountDetail

| Parameter | Req | Data Type | Description |
|-----------|-----|-----------|-------------|
| autoActivationType | Opt | string | Specifies the type of Auto Activation. <br> Valid values: EZActivation, BillerMerge, RegularBillerRules | 
| activationType | Opt | string | Specifies how the account was activated. <br> Valid values: AutoActivation, User | 
| billerThumbnailImageUrl | Opt | string | URL for the biller sample bill thumbnail image. | 
| billTransactionScheduleEligible | Req | boolean | Indicates if the account is eligible for Automatic Transactions (Bill Transaction Schedule). To be eligible, the following conditions must be true: <br> - The biller supports AutoPay. <br> - The first e-bill has been received from the biller. <br> - The payee is not in a trial period. | 
| emailAddressShare | Req | boolean | Determines whether the consumer’s email address may be sent to the biller. Valid values: <br> true – Service holder’s email address may be sent to the biller. <br> false – Do not send the service holder’s email address to the biller. | 
| firstEbillReceived | Req | boolean | Indicates if the first e-bill has been received. |
| name | Req | [IndividualName](#individualname) | The name of the consumer associated with the e-bill activation request. May be different from the consumer name. If not provided, the consumer name will be used. |
| isTrialPeriod | Req | boolean | Indicates if the biller has initiated a trial period. Valid values: <br> true – Trial period initiated. <br> false – No trial period. <br> An e-bill account is identified as being in a trial period when: <br> - The e-bill account is active <br> - The consumer should be asked to suppress paper <br> - Trial period end date is valid | 
| paperProcessingStatus | Req | string | Specifies the state of paper bill delivery. Valid values:<br> Unknown – Fiserv does not know if the biller is sending paper bills or not. <br> PaperBillsStillSent – Fiserv knows that the biller is still sending paper bills. <br> NoPaperBillsSent – Fiserv knows that the biller has stopped sending paper bills. <br> EbillDeactivationAfterTrialPeriod – Due to have e-bill service deactivated by the biller at the end of the trial period if paper suppression is not initiated.<br> PaperBillsStopAfterTrialPeriod – Pending to have paper bills stopped at the end of the trial period. |
| paperSuppressionIncentiveMessage | Opt | string | The actual incentive message that is currently active for the account. | 
| paperSuppressionTermsUrl | Opt | string | URL for the paper suppression terms. |
| promptPaperSuppression | Req | boolean | Indicates if the consumer should be prompted to indicate if the consumer would like to suppress paper bills. Valid values: <br> true – Prompt the consumer to indicate if the consumer would like to suppress paper bills. <br> false – Do not prompt the consumer to indicate if the consumer would like to suppress paper bills. |
| serviceAddress | Req | [USAddress](#usaddress) | Address of the service holder. |
| serviceBusinessName | Cond | string | Business name of the service holder. Condition: serviceHolderType is Business. |
| serviceDaytimePhone Number | Opt | string | Phone number where the service holder can be reached during normal business hours. |
| serviceEveningPhone Number | Opt | string | Phone number where the service holder can be reached after normal business hours. |
| serviceHolderType | Req | string | Identifies if the service holder is an individual or a business. Valid values: <br> Business <br> Consumer | 
| status | Req | string | Status of the e-bill service account. Valid values: <br> Pending <br> Active <br> Inactive <br> Rejected | 
| trialPeriodEnd | Cond | string | Date that the trial period ends for this account. Condition: isTrialPeriod is true. <br> Format: yyyy-MM-dd |
| trialPeriodStart | Cond | string | Date that the trial period started for this account. Condition: isTrialPeriod is true. <br> Format: yyyy-MM-dd |
| transactionScheduleTypesSupported | Req | Array of string | The payment structure for the automatic transaction. Valid values: <br> FixedAmount <br> AmountDue <br> MinimumAmountDue <br> AccountBalance | 
| self | Req | string | The URI to access the e-bill service item. |
| id | Req | string | The ID of the e-bill service item. This will always be empty (null). |

#### EbillCapability 

| Parameter | Req | Data Type | Description |
|-----------|-----|-----------|-------------|
| billerName | Opt | string | The Fiserv-determined value of the biller name. This may be different from the biller name that was passed in the request. |
| billerLogoUrl | Opt | string | The URL for the biller logo. The preferred logo size is 90 × 90 pixels. |
| billerSiteUrl | Opt | string | The URL for biller-provided additional information. |
| billerThumbnailImageUrl | Opt | string | The URL for the biller sample bill thumbnail image. | 
| paperSuppressionOptions | Opt | Array of [PaperSuppressionOption](#papersuppressionoption) | The paper suppression options that the biller allows. | 
| preAuthTokens | Opt | Array of [PreAuthToken](#preauthtoken) | List of any pre-authorization tokens that need to be provided by the consumer to activate e-bill service. <br> The values for the tokens must be sent in the Payee E-bill Service POST request. |
| restrictionText | Opt | string | Text regarding any restrictions that the biller may have. | 
| serviceAddressRequired | Req | boolean | Indicates if the service address is required for authentication by the biller. This is the address where the biller provides services. Often applies to utilities in which the address where services are provided may differ from the billing address. <br> Best practice is if this flag is true, the consumer should be presented with the opportunity to provide a service address that may be different than their consumer profile address. |
| serviceAddress | Cond | [USAddress](#usaddress) | Address of the service holder, pre-filled for the consumer. This information is taken from the consumer's profile. Condition: serviceAddressRequired is true. | 
| trialPeriodDays | Opt | integer | Time period of the biller’s trial period (in days). Only applicable for those billers supporting a finite trial period. Otherwise, will be zeros. |
| accountNumberEditable | Req | boolean | Indicates if the consumer can edit the account number in a subsequent Payee E-bill Service POST request. Valid values: <br> true – The account number can be edited. <br> false – The account number cannot be edited. |
| accountNumberHelpText | Opt | string | If available, biller-provided help text regarding the consumer’s account number. |
| self | Req | string | The URI to access the e-bill capability item.|
| id | Req | string | The ID of the e-bill capability item. | 

#### EbillServiceActivateInput

| Parameter | Req | Data Type | Description |
|-----------|-----|-----------|-------------|
| acceptedTC | Req | boolean | Indicates if the consumer has accepted the biller’s terms and conditions. Must be true to continue with the add function. |
| businessName | Opt | string | Name of the business associated with the e-bill activation request. |
| accountNumber | Cond | string | The consumer’s account number with the payee. <br> Length 1–32 <br> Pattern: ^[a-zA-Z0-9]{1,32}$ <br> <p>Condition: This field will be populated if Payee E-bill Capability GET returns true for accountNumberEditable. | 
| emailAddressShare | Opt | boolean | At the time of enrollment, the consumer is commonly presented with a check box that asks if the consumer wants to share their address with the biller. If the consumer opts in, the only time that Fiserv shares the address with the biller is at the time of e-bill enrollment. |
| name | Opt | [IndividualName](#individualname) | Name of the consumer associated with the e-bill activation request. May be different from the consumer name. If not provided, the consumer name will be used. |
| paperSuppressionOption | Req | string | Paper suppression option selected by the consumer. Valid values: <br> DualDelivery <br> PaperSuppression <br> Trial |
| preAuthTokens | Opt | Array of [PreAuthTokenInput](#preauthtokeninput) | List of any pre-authorization tokens that need to be provided by the consumer to activate e-bill service. |
| serviceAddress | Opt | [USAddress](#usaddress) | Address of the consumer associated with the e-bill activation request. Will default to the consumer address if not provided. However, best practice is that if EbillCapability.serviceAddressRequired is true, the service address should be collected from the consumer via UI interaction. |

#### EbillServiceActivateOutput

| Parameter | Req | Data Type | Description |
|-----------|-----|-----------|-------------|
| trialPeriodDays | Cond | integer | Duration of the trial period. Condition: In the request, paperSuppressionOption is Trial. |
| activationState | Req | string | Determines the state of the e-bill activation request transaction. This is not provided if the request is not able to be processed. ActivationState represents the different states of an e-bill activation request transaction. The initial state is Pending. All Pending requests are submitted to the biller for approval. The biller will either accept or reject the e-bill activation request. E-bill activation requests that have been accepted by the biller are set to an Accepted state. Those that are rejected will be set to a Rejected state. Valid values: <br> Pending <br> Accepted <br> Rejected <br> NotApplicable <br> <br> NotApplicable is returned when transaction processing failed, stopped, or was cancelled internally, so the transaction state cannot be updated. The calling application/UI should consider this as a failed state. |
| activationStateMessage | Opt | string | Optional message that may be associated with the activationState. |
| self | Req | string | The URI to access the e-bill service item. |
| id | Req | string | The ID of the e-bill service item. | 

#### EligibleFundingAccount

| Parameter         | Req | Data Type        | Description                                                                                                            |
|-----------|-----|-----------|----------------------------------------------|
| fundingAccountUri | Req | string           | The source funding account URI for the automatic transaction.                                                          |
| maxAmount         | Req | number ($double) | Maximum amount allowed for a bill transaction schedule. Applicable only for the transaction type value of FixedAmount. |
| minAmount         | Req | number ($double) | Minimum amount allowed for a bill transaction schedule. Applicable only for the transaction type value of FixedAmount. |

#### Email

| Parameter | Req | Data Type | Description |
|-----------|-----|-----------|-------------|
| value | Req | string | Email value. <br> Pattern: ^([\_+A-Za-z0-9]+((\\.\|\\-)[\_+A-Za-z0-9]+)\*@[A-Za-z0-9]+((\\.\|\\-)[A-Za-z0-9]+)\*(\\.[A-Za-z]{2,8}))$ |

#### Features

| Parameter | Req | Data Type | Description |
|-----------|-----|-----------|-------------|
| supportsBillDiscovery | Req | boolean | Indicates if the tenant/sponsor offers Bill Discovery. <br> true – Offers Bill Discovery <br> false – Does not offer Bill Discovery |
| supportsCardFundedBillPay | Req | boolean | Indicates if the tenant/sponsor offers card funded bill payment. <br> true – Offers card funded bill payment <br> false – Does not offer card funded bill payment |
| billDiscoveryProviderType | Cond | Array of string | Condition: Required when supportsBillDiscovery is true. <br> Specifies how bills are discovered—via BillServiceProvider, CreditBureau, or both. <br> Valid values: BillServiceProvider, CreditBureau |
| transactionHistoryMonths | Req | integer | Specifies the total months of transaction history supported by the tenant/sponsor (minimum is 6 months). |
| lddEnabled | Req | boolean | Indicates if LDD service is enabled/supported for the sponsor. |
| cardFundingOptions | Cond | [CardFundingOptions](#cardfundingoptions) |Condition: Required when supportsCardFundedBillPay is true. <br> Specifies the types of card funding supported. |
| supportsClassicView | Req | boolean | Indicates if the tenant/sponsor offers Classic View. <br> true – Offers Classic View <br> false – Does not offer Classic View |

#### FiledBillInfo

| Parameter | Req | Data Type | Description |
|-----------|-----|-----------|-------------|
| reason | Req | string | Indicates the method of the bill’s resolution. Valid values: <br> NoneSpecified, Bank, Check, Cash, NotPaid, Other, BillerWebSite, Phone, Mail, Office, ZeroBalanceBill, ContestedBill |
| note | Opt | string | Optional note entered by the consumer with information about the bill and its resolution. Length 1-80 | 

#### FinancialInstitutionModel

| Parameter                | Req | Data Type | Description                 |
|--------------------------|-----|-----------|-----------------------------|
| financialInstitutionName | Req | string    | Financial institution name. |

#### IdentityValidationInformation

| Parameter | Req | Data Type | Description |
|-----------|-----|-----------|-------------|
| idType | Req | string | Type of identification. Valid values: “StateIssuedDriverLicense”, “StateIssuedIdCard”, “Passport”, “MilitaryIdCard” |
| idNumber | Req | string | Identification number on ID. Length: 6-30 <br> Pattern: ^[a-zA-Z0-9_(){}&@!+#.'$,%^ \*-]\* <br> Cannot contain all zeroes<br> Cannot contain all repeating number | 
| idIssuingState | Cond | string | The state that issued identification; must be a valid US state. Length: 2 <br> Pattern: ^[A-Z]\* <br> Condition: Required when idType = “StateIssuedDriverLicense” or “StateIssuedIdCard”. |
| idIssuingCountry | Req | string | The country that issued identification; must be a valid country code (ISO 3166-1 alpha-3 codes). Length: 3 <br> Pattern: ^[a-zA-Z0-9_(){}&@!+#.'$,%^ \*-]\* |

#### IndividualName

| Parameter | Req | Data Type | Description |
|-----------|-----|-----------|-------------|
| first | Req | string | First name of the consumer. <br> Length: 1–32 <br> Pattern: ^[a-zA-Z][a-zA-Z0-9_(){}&@!+#.$,%\*-]\* |
| last | Req | string | Last name of the consumer. <br> Length: 1–32 <br> Pattern: ^[a-zA-Z][a-zA-Z0-9_(){}&@!+#. $,%\*-]\* <br> Last name is required, regardless of consumer type. <br> If this is an account for an individual, this is the individual consumer’s last name. If this is an account for a business, this is the last name of a principal owner of the business or a person to contact. |
| middle | Opt | string | Middle name of the consumer. <br> Length: 0–32 <br> Pattern: ^[a-zA-Z][a-zA-Z0-9_(){}&@!+#.$,%\*-]\* |
| nickname | Opt | string | Short name used to refer to the consumer. <br> For example, BOB is a nickname for ROBERT. <br> Length: 1–30 <br> Pattern: ^[a-zA-Z0-9_(){}&@!+#.'\$, \%\*-]{1,30}$ <br> When provided, at least one character is required. |
| prefix | Opt | string | A title relating to the consumer, such as DR., MR., or MS. <br> Length: 1–6 <br> Pattern: ^[a-zA-Z][a-zA-Z0-9_(){}&@!+#.$,%\*-]\* |

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

| Parameter | Req | Data Type | Description |
|-----------|-----|-----------|-------------|
| firstName | Req | string | First name of the consumer. Length: 1–32 <br> Pattern: ^[a-zA-Z][a-zA-Z0-9_(){}&@!+#.$,%\*-]\* |
| lastName | Req | string | Last name of the consumer. Length: 1–32 <br> Pattern: ^[a-zA-Z][a-zA-Z0-9_(){}&@!+#. $,%\*-]\* |
| middleName | Opt | string | Middle name of the consumer. Length: 0–32 <br> Pattern: ^[a-zA-Z][a-zA-Z0-9_(){}&@!+#.$,%\*-]\* |
| nickname | Opt | string | Short name used to refer to the consumer. Length: 0–30 <br> For example, BOB is a nickname for ROBERT. <br> Pattern: ^[a-zA-Z0-9_(){}&@!+#.'\$, \%\*-]{1,30}$ |
| prefix | Opt | string | A title relating to the consumer, such as DR., MR., or MS. Length: 0–6 <br> Pattern: ^[a-zA-Z][a-zA-Z0-9_(){}&@!+#.$,%\*-]\* |

#### PaperSuppressionOption

| Parameter | Req | Data Type | Description |
|-----------|-----|-----------|-------------|
| isEmbedded | Req | boolean | Indicates if the presenter UI needs to enforce the user’s reading of the actual terms and conditions (Ts&amp;Cs) consent text. <br> True - Present the actual Ts&amp;Cs to a consumer on the same page where account number, preauth tokens, service address, etc. are collected. <br> False – The presenter can include a URL link that the consumer can click on to access the Ts&amp;Cs. |
| option | Req | string | Paper suppression options applicable for the selected biller. The presenter sends the consumer’s choices back to Fiserv in the Payee E-bill Service POST request. Valid values: <br> DualDelivery <br> PaperSuppression <br> Trial |
| termsText | Opt | string | Text that applies to paper suppression. Each paper suppression type has text (“terms of use”) that should be displayed on the UI. Note that “terms of use” text is different than Ts&amp;Cs. |
| termsUrl | Opt | string | A URL pointing to the actual terms and conditions text. If isEmbedded is true, the contents of this URL are pulled into a scrollable box that is presented to the consumer. If isEmbedded is false, this URL is presented as a link. |

#### Payee

| Parameter | Req | Data Type | Description |
|-----------|-----|-----------|-------------|
| name | Req | string | The name of the payee. <br> Length: 2–32 |
| nickname | Opt | string | The nickname of the payee. Only populated if provided by the consumer. <br> Length: 1-30 |
| maskedAccountNumber | Cond | string | The masked account number for the payee. Not applicable for payees added as individuals. <br> Length: 1–32 <br> Condition: Populated only if the consumer has provided an account number when adding the payee. |
| unmaskedAccountNumberUri | Cond | string | The URI to the full, unmasked account number. See “[Get a Payee’s Unmasked Account Number](#get-a-payees-unmasked-account-number).” <br> Not applicable for payees added as individuals. <br> Condition: Populated only if the consumer has provided an account number when adding the payee. |
| category | Opt | string | The consumer-defined category of the payee. Applies to bill payment payees only. |
| contactPhoneNumber | Opt | string | Phone number used to contact the payee if there are issues posting the payment. |
| merchantUri | Opt | string | The URI to access the merchant directly. |
| automaticTransactionUris | Opt | Array of string | List of automatic payment models for the payee. |
| reminderModelUri | Opt | string | Reserved for future use. |
| address | Opt | [USAddress](#usaddress) | Payee address information. |
| overnightAddress | Opt | [USAddress](#usaddress) | Address for overnight payments if one exists for the payee. |
| lastUsedFundingAccountUri | Opt | string | The URI to access the most recent funding account used in a transaction with this payee. |
| socialTokens | Opt | Array of PayeeSocialToken | Reserved for future use. |
| accountTokens | Opt | Array of [PayeeAccountToken](#payeeaccounttoken) | Reserved for future use. |
| ebillServiceActivationUri | Opt | string | The URI used to begin e-bill activation for an e-bill-capable payee. Will only be provided if payee is e-bill-capable but not e-bill enabled; otherwise, null is returned. |
| ebillServiceActivationType | Req | string | Indicates the type of e-bill service activation for the payee. Valid values: Full, Lite, None |
| ebillServiceUri | Opt | string | The URI for information needed for activation. Will only be provided if the payee is e-bill-enabled. |
| loginCredentialsUri | Opt | string | Reserved for future use. |
| payeeLogoUrl | Opt | string | The URI for the payee logo. The preferred logo size is 90 × 90 pixels. |
| payeeLogoGeneric | Opt | boolean | Indicates if the logo returned for the payee is generic (not specific to the payee). |
| modifiableFields | Req | Array of string | List of fields that can be changed for the payee. |
| status | Req | string | Status of the payee. Valid values: Active, Inactive |
| source | Opt | string | Reserved for Fiserv use. |
| earliestStandardTransactionDate | Opt | string | Earliest date that a standard (non-expedited, non-overnight) transaction can be scheduled. |
| legacyPayeeId | Req | string | Legacy payee ID. |
| ebillServiceActivationStatus | Req | string | Indicates the ebill activation status of the Payee. This flag is always returned, even if the payee is not ebill capable. Valid values: NotApplicable, Pending, Capable, Active, Inactive |
| billTransactionScheduleActive | Req | boolean | Indicates if a bill transaction schedule is already active (i.e., eBill Autopay is active) for the payee. <br> true - Bill transaction schedule is enabled for the payee. <br> false – Bill transaction schedule is not enabled for the payee. |
| billTransactionScheduleCapable | Req | boolean | Indicates when a payee is capable of an automatic transaction schedule for paying ebills (eBill Autopay). Valid values: true, false <br> This is true when: <br> - Payee is active <br> - Payee is e-bill enabled <br> - Biller supports Autopay <br> - The first e-bill has been received |
| recurringTransactionScheduleActive | Req | boolean | Indicates if a recurring transaction schedule (one or more) is already set up for the payee. <br> true - Recurring transaction schedule is set up for the payee. <br> false – Recurring transaction schedule is not set up for the payee. |
| isPaperTransactionsEnabled | Req | boolean | Indicates whether transactions to this payee are typically processed by check/draft. <br> true – Transactions are typically processed by check/draft. <br> false – Transactions are not typically processed by check/draft. |
| isRushDeliveryAvailable | Req | boolean | Indicates if rush delivery of the payment is an option for the payee. Valid values: true, false <br> true – Rush delivery is an option. <br> false – Rush delivery is not an option. |
| self | Req | string | The URI to access the payee directly. |
| id | Req | string | The ID of the payee. |

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

| Parameter | Req | Data Type | Description |
|-----------|-----|-----------|-------------|
| name | Req | string | The name of the payee.<br> Pattern: ^[\\x20-\\x5A\\x5C\\x5F-\\x7E]+$ <br> Length: 2–32 |
| nickname | Opt | string | The nickname of the payee. Length: 1-30 <br> Pattern: ^[\\x20\\x2C-\\x2E\\x30-\\x39\\x41-\\x5A\\x61-\\x7A\\r\\n]+$ |
| accountNumber | Opt | string | The consumer’s account number with the payee. Length: 1–32 <br> Pattern: ^[a-zA-Z0-9 !"#\$\%&-]{1,32}$ |
| contactPhoneNumber | Cond | string | Phone number used to contact the payee if there are issues posting the transaction. Length: 10-12 <br> Pattern: (^[0-9]{10}\$)\|(^\\(?[0-9]{3}\\)?-[0-9]{3}-[0-9]{4}\$)\|(^\\(?[0-9]{3}\\)?\\s?[0-9]{3}-[0-9]{4}$) <br> Must be numeric and may contain a dash or space between the numbers at the appropriate placement. The phone number must be valid based on the North American Numbering Plan (for example, the area code cannot begin with a 0 or 1). Example: 234-555-1212 <br> Condition: Required when sourceUri is not supplied when adding the payee. |
| address | Cond | [USAddress](#usaddress) | Payee address information. <br> Condition: Required when sourceUri is not supplied when adding the payee. |
| overnightAddress | Opt | [USAddress](#usaddress) | Address for overnight payments if one exists for the payee. | 
| socialTokens | Opt | Array of SocialToken | Reserved for future use. |
| accountTokens | Opt | Array of [AccountTokenAddInfo](#accounttokenaddinfo) |Reserved for future use. |

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

| Parameter | Req | Data Type | Description |
|-----------|-----|-----------|-------------|
| phoneType | Req | string | The type of phone number. Valid values: Day, Evening, Mobile |
| value | Req | string | The phone number value. <br> Pattern:(^[0-9]{10}\$)\|(^\\(?[0-9]{3}\\)?-[0-9]{3}-[0-9]{4}\$)\|(^\\(?[0-9]{3}\\)?\\s?[0-9]{3}-[0-9]{4}$) | For a user enrollment, all non-numeric characters shall be removed and if there is a leading “1,” it is removed. It may contain a dash or space between the numbers at the appropriate placement. The phone number must be valid based on the North American Numbering Plan (for example, the area code cannot begin with a 0 or 1). |

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

| Parameter | Req | Data Type | Description |
|-----------|-----|-----------|-------------|
| amount | Req | double | The transaction amount. <br> Pattern: ^\\d+(\\.\\d{1,2})?$ |
| endDate | Opt | string | The end date for this automatic transaction schedule. A null value means transactions will be scheduled indefinitely. Should not be submitted alongside maximumTransactionCount. <br> Format: yyyy-MM-dd |
| endTransactionAmount | Opt | double | Amount for the last transaction. This will be provided by the consumer if the last transaction amount is different from the rest of the transactions in the recurring model. <br> Pattern: ^\\d+(\\.\\d{1,2})?$ |
| frequency | Req | string | The frequency that transactions will be scheduled to the payee. Valid values: <br> Weekly, Every2Weeks, Every4Weeks, TwiceAMonth, Monthly, Every2Months, Every3Months, Every4Months, Every6Months, Annually |
| initialTransactionAmount | Opt | double | Amount for the first transaction. This will be provided by the consumer if the first transaction amount is different from the rest of the transactions in the recurring model. <br> Pattern: ^\\d+(\\.\\d{1,2})?$ |
| initiationDate | Req | string | The date of the first scheduled transaction. Format: yyyy-MM-dd |
| nextTransactionDate | Opt | string | Date of the next payment transaction to be scheduled. If there is not a next payment (such as when a model is terminated due to reaching the end of the model), this field is not populated. <br> Note: Not used for POST. |
| maximumTransactionCount | Opt | integer | The maximum number of transactions that this automatic transaction model should process before ending. Should not be submitted alongside endDate. |
| remainingTransactionCount | Opt | integer | The number of transactions remaining before this automatic payment model ends. <br> Note: Not used for POST. |
| memo | Opt | string | Transaction memo. Maximum length: 34 <br> Pattern: ^[a-zA-Z0-9_(){}&@!+#.'$,%^ \*-]\* |
| transactionScheduledAlert | Req | boolean | Indicates if the consumer is notified when a transaction is scheduled. <br> True - Notify the consumer when a transaction is scheduled. <br> False - Do not notify the consumer when a transaction is scheduled. This is the default. |
| transactionSentAlert| Req | boolean | Indicates if the consumer is notified when a transaction is processed. <br> True - Notify the consumer when a transaction is processed. <br> False - Do not notify the consumer when a transaction is processed. This is the default. |
| recurringScheduleExpireAlert | Req | boolean | Indicates if the consumer is notified when the final transaction from a recurring model is scheduled. <br> True - Notify the consumer when the final transaction from the recurring model is scheduled. <br> False - Do not notify the consumer when the final transaction from the recurring model is scheduled. This is the default. |
| duration | Req | string | The duration of recurring model. Valid values: UntilSubscriberCancels, XNumberOfPayments, SpecificEndDate <br> Note: Not used for POST. |
| nextTransactionGenerationDate | Opt | string | Date when the next payment will be generated/spawned. This date may not correspond to the date of the next actual payment, because that payment could already have been spawned. This field will not be returned when there is no next payment to be spawned. <br> Note: Not used for POST. |

#### RecurringTransactionSchedulePatch

| Parameter | Req | Data Type | Description |
|-----------|-----|-----------|-------------|
| amount | Opt | double | The transaction amount. <br> Pattern: ^\\d+(\\.\\d{1,2})?$ |
| endDate | Opt | string | The end date for this automatic transaction schedule. A null value means transactions will be scheduled indefinitely. Should not be submitted alongside maximumTransactionCount. <br> Format: yyyy-MM-dd |
| endTransactionAmount | Opt | double | Amount for the last transaction. This will be provided by the consumer if the last transaction amount is different from the rest of the transactions in the recurring model. <br> Pattern: ^\\d+(\\.\\d{1,2})?$ |
| frequency | Opt | string | The frequency that transactions will be scheduled to the payee. Valid values: <br> Weekly, Every2Weeks, Every4Weeks, TwiceAMonth, Monthly, Every2Months, Every3Months, Every4Months, Every6Months, Annually |
| initiationDate | Opt | string | The date of the first scheduled transaction. Format: yyyy-MM-dd |
| maximumTransactionCount | Opt | integer | The maximum number of transactions that this automatic transaction model should process before ending. Should not be submitted alongside endDate. |
| memo | Opt | string | Transaction memo. Maximum length: 34 <br> Pattern: ^[a-zA-Z0-9_(){}&@!+#.'$,%^ \*-]\* |
| transactionScheduledAlert | Opt | boolean | Indicates if the consumer is notified when a transaction is scheduled. <br> True - Notify the consumer when a transaction is scheduled. <br> False - Do not notify the consumer when a transaction is scheduled. This is the default. |
| transactionSentAlert | Opt | boolean | Indicates if the consumer is notified when a transaction is processed. <br> True - Notify the consumer when a transaction is processed. <br> False - Do not notify the consumer when a transaction is processed. This is the default. |
| recurringScheduleExpireAlert | Opt | boolean | Indicates if the consumer is notified when the final transaction from a recurring model is scheduled. <br> True - Notify the consumer when the final transaction from the recurring model is scheduled. <br> False - Do not notify the consumer when the final transaction from the recurring model is scheduled. This is the default. |

#### RecurringTransactionScheduleTypesSupported

| Parameter | Req | Data Type | Description |
|-----------|-----|-----------|-------------|
| frequency | Req | Array of string | The available frequency options for setting up a recurring payment. Valid values: <br> Weekly, Every2Weeks, Every4Weeks, TwiceAMonth, Monthly, Every2Months, Every3Months, Every4Months, Every6Months, Annually | 
| duration | Req | Array of string | The available duration options for setting up a recurring payment. Valid values: <br> <p>UntilSubscriberCancels, XnumberOfPayments, SpecificEndDate |
| eligibleFundingAccounts | Req | Array of [RecurringEligibleFundingAccount](#recurringeligiblefundingaccount) | A list of eligible funding accounts with the available automatic transaction options. |

#### Reminder

| Parameter | Req | Data Type | Description |
|-----------|-----|-----------|-------------|
| alertAtLeadTimeBeforePaymentDueDate | Req | boolean | Indicates if the consumer is reminded at lead time (see leadTime) before a payment is due. <br> Valid values: <br> true – Remind the consumer at lead time before a payment is due. <br> false – Do not remind the consumer at lead time before a payment is due. |
| alertIfNoBillPaymentMadeByDueDate | Req | boolean | Indicates if the consumer is reminded if no payment has been made by the due date. <br> Valid values: <br> true – Remind the consumer that a payment is due. <br> false – Do not remind the consumer that a payment is due. | 
| alertWhenBillPaymentProcesses | Req | boolean | Indicates if the consumer is notified when a payment is processed. <br> Valid values: <br> true – Notify the consumer when a payment has been marked as processed. <br> false – Do not notify the consumer when a payment has been marked as processed. |
| amount | Req | number | The payment amount associated with this reminder. |
| currentDueDate | Req | string | Next date for the bill payment reminder in yyyy-MM-dd format. |
| firstReminderDate | Req | string | First date for the bill payment reminder in yyyy-MM-dd format. |
| frequency | Req | string | The frequency of how often the reminder is automatically created. Valid values: <br> Weekly <br> Every2Weeks <br> TwiceAMonth <br> Every4Weeks <br> Monthly <br> Every2Months <br> Every3Months <br> Every4Months <br> Every6Months <br> Annually | 
| leadTime | Req | string | Number of days (excluding any risk management calculations) before the due date that the reminder will be created. Valid values: <br> Lead03Days <br> Lead05Days <br> Lead10Days <br> Lead14Days <br> Lead21Days <br> Lead28Days |
| payeeUri | Req | string | URI for the payee. This matches the payee URI returned in GET Payees. |
| self | Req | string | URI for the reminder model. | 
| id | Req | string | Identifier for the reminder model. |

#### ReminderList

| Parameter | Req | Data Type                      | Description |
|-----------|-----|--------------------------------|-------------|
| reminders | Req | Array of [Reminder](#reminder) |             |

#### ResultInfo

| Parameter | Req | Data Type | Description |
|-----------|-----|-----------|-------------|
| resultCategory | Req | string | Description of the kind of result. Valid values: <br> ERROR <br> WARNING <br> INFO |
| code | Req | string | Code associated with the result of a request. |
| field | Opt | string | Matches the name of a field causing an error. |
| fieldPath | Opt | string | Specifies the field causing an error. In addition to the field name, FieldPath includes the tags that the field is nested in, starting with the request level. |
| listItemId | Opt | string | For some requests, each item in a list has a unique ID so that the response can be tied to the requested item in the list. If the request resulted in an error in a list, this ID will tell the caller which item in the list was in error. | 
| description | Req | string | A textual description of &lt;code&gt;. |

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

| Parameter | Req | Data Type | Description |
|-----------|-----|-----------|-------------|
| toDoType | Req | string | Identifies the item’s ToDo type. Valid values: <br> PotentialPayee <br> Reminder <br> UnpaidBill <br> UnpaidBill applies to both e-bills and bill due alerts. | 
| toDoDetailUri | Req | string | URI for the item that requires the consumer’s attention. For example, the URI to a potential payee that needs to be added/verified. |
| payeeUri | Opt | string | The corresponding payee URI for a Reminder or UnpaidBill toDoType. |
| verificationRequired | Cond | boolean | Condition: Required for toDoType = PotentialPayee. Indicates if verification is required before adding the payee. <br> True – Verification required. <br> False – Verification not required.|
| actionDate | Opt | string | The date that the ToDo item requires action to avoid possible negative consequences. Example: due date of a pending e-bill. <br> Format: yyyy-MM-dd <br> For Reminders, this is the due date entered by the consumer to be used as the trigger for the reminder message. |
| amount | Opt | double | The amount of the ToDo item if there is one. For e-bills and bill due alerts this is the AmountDue (not minimum amount or balance). For Reminders, this is the amount entered by the consumer to appear in the reminder message. |
| maskedAccountNumber | Opt | string | The masked account number of the account. |
| description | Req | string | A description relevant to the ToDo type. For PotentialPayees, this will be the merchant name. For Reminder and UnpaidBill, this will be the payee name. |
| self | Req | string | URI for the ToDo item. |
| id | Req | string | Identifier for the ToDo item. |

#### Transaction

| Parameter | Req | Data Type | Description |
|-----------|-----|-----------|-------------|
| listItemId | Req | integer | Serial number for this transaction that was sent in a request containing multiple transactions. Use this number to identify the response for each transaction in the request, because the sequence of responses may not match the sequence of requests due to web services infrastructure. Each listItemId must be unique within a request. | 
| amount | Req | double | The amount of the transaction. The transaction amount must be within tenant/sponsor limits (minimum and maximum). <br> Pattern: ^\\d+(\\.\\d{1,2})?$ <br> A payment funded with a bank account that is Pending Confirmation cannot be scheduled with an amount that exceeds the Fiserv cumulative unconfirmed payment limit. This does not apply to SameDay Payments. | 
| deliveryDate | Req | string | The date for the transaction in yyyy-MM-dd format. <br> This date may not be in the past and may not be more than 365 days in the future. This date may not be earlier than the earliest available payment date for the selected payee. <br> Generally, the Fiserv system will not allow a payment to be scheduled for a date that is not a bank processing date (weekends and Federal Reserve Board recognized holidays). However, this will depend on the merchant’s configuration. For example, some MoneyGram merchants accept payments on weekends. <br> When the payee is Overnight Check (ONC) capable and the current time is before the cutoff time for ONC, the payment will be added as an ONC payment. <br> A payment funded with a bank account that is Pending Confirmation cannot be scheduled with a payment date greater than 45 days after the bank account was added. <br> For SameDay Payments, this date must be current date. | 
| fundingAccountUri | Req | string | The source funding account URI for the given transaction. Account types that are eligible to be used are those enabled for Bill Payment (service) for the tenant/sponsor. <br> If account confirmation is enabled for the tenant/sponsor, only accounts that are Confirmed or Pending Confirmation can be used to schedule a payment. |
| note | Opt | string | A consumer’s “note to self.” This note is not submitted to the payee. Length: 0–255 <br> Pattern: ^[\\x2A-\\x2E\\x30-\\x39\\x40-\\x5A\\x5F\\x5E\\x61-\\x7A\\x20\\x21\\x23-\\x25]+$ <br> The note field only allows the following character sets: a-z, A-Z, 0-9, _ @!+#.\$,%^\*- <br> This note is a convenience feature. If the payment is scheduled successfully but Fiserv is unable to store the payment note, you will receive only a warning, not an error. |
| destinationUri | Req | string | The destination URI for the given transaction. May identify the payee, bill due alert, and/or e-bill to be paid. <br> If the destination is a payee, the payee must exist and be active for the consumer. <br> If the destination is an e-bill, the e-bill ID must be a valid e-bill ID for the consumer. |
| withdrawNow | Opt | boolean | Reserved for future use. |
| memo | Opt | string | Transaction memo. Maximum of 34 characters. |
| cavv | Opt | string | Placeholder for 3-D Secure. Cardholder Authentication Verification Value. Used if the transaction is funded by a card account and the institution is participating in 3-D Secure. |

#### TransactionCalendar

| Parameter                 | Req | Data Type                                          | Description                                                                 |
|------------|-----|----------|-----------------------------------------------|
| deliveryDate              | Req | string                                             | The date of delivery of the transaction. Format: yyyy-MM-dd                 |
| transactionDestinationUri | Req | string                                             | Transaction destination associated with this specific transaction calendar. |
| withdrawOnDate            | Opt | string                                             | Reserved for future use.                                                    |
| transactionOptions        | Req | Array of [TransactionOptions](#transactionoptions) | The various options that are provided for a transaction.                    |

#### TransactionList

| Parameter    | Req | Data Type                                                    | Description                                                                  |
|---------|----|-----------|-------------------------------------------------|
| transactions | Req | Array of [TransactionOutputDetail](#transactionoutputdetail) | List of transactions. There is an empty array if there is no data to return. |

#### TransactionOptions

| Parameter | Req | Data Type | Description |
|-----------|-----|-----------|-------------|
| cardDebitDate | Opt | string | The date the funds are debited from the user’s account. Only applies to card funded transactions. (format: yyyy-MM-dd) |
| additionalInfoRequired | Opt | Array of string | Additional information that may be required to make a certain payment date available. Valid value: “OvernightAddress” |
| cutOffTime | Opt | string | Cutoff date and time for the transaction in UTC format. Not returned for future processing dates. <br> To determine if a transaction option is valid, compare current time (in UTC format) to the provided cut off time: <br> If &lt;current time in UTC&gt; is before &lt;cutoff time&gt; then the date can be used. <br> If &lt;current time in UTC&gt; is after &lt;cutoff time&gt; then that date is no longer valid. |
| deliveryMethod | Opt | string | The method of delivery of the transaction. Valid values: "Electronic" "Paper" "Overnight" |
| fee | Opt | number | The fee amount for the transaction. |
| fundingAccountUri | Req | string | The funding account for the transaction. |
| withdrawNow | Opt | boolean | Reserved for future use. |
| instantDelivery | Opt | boolean | Reserved for future use. |
| isPreferredDate | Opt | boolean | When a transaction's due date is in the future, a value of true for this parameter indicates that this transaction option is preferred. This is the recommended option to pre-populate for a consumer. |
| minAmount | Opt | number | The minimum amount of the transaction. |
| maxAmount | Opt | number | The maximum amount of the transaction. |

#### TransactionOutput

| Parameter | Req | Data Type | Description |
|-----------|-----|-----------|-------------|
| self | Cond | string | URI pointing to the transaction itself. Condition: Always returned for a successful response. |
| id | Cond | string | Unique identifier for the transaction. Condition: Always returned for a successful response. |
| confirmationNumber | Cond | string | Confirmation number for the payment transaction. Condition: Always returned for a successful response. |
| deliveryDate | Cond | string | The transaction expected delivery date, populated for pending and completed transactions. Format: yyyy-MM-dd <br> Condition: If the payment fails risk, deliveryDate is not returned. |
| nextAvailableTransactionDate | Cond | string | Next available transaction date. Condition: Returned only if the payment fails risk (transaction date is too early). |
| debitDate | Cond | string | Date that the consumer’s account is to be debited. Format: yyyy-MM-dd <br> Condition: Always returned for successful response. |
| legacyTransactionId | Req | string | Server transaction timestamp. |

#### TransactionModifyOutput

| Parameter | Req | Data Type | Description |
|-----------|-----|-----------|-------------|
| self | Cond | string | URI pointing to the transaction itself. Condition: Always returned for a successful response. |
| id | Cond | string | Unique identifier for the transaction. Condition: Always returned for a successful response. |
| confirmationNumber | Cond | string | Confirmation number for the payment transaction. Condition: Always returned for a successful response. |
| deliveryMethod | Cond | string | The delivery method for the transaction if the transaction has been processed. Valid values: "Electronic" "Paper" Condition: If the payment fails risk, deliveryMethod is not returned. |
| status | Cond | string | The status of the payment transaction. Valid values: "Pending", "Complete", “InProcess”, "Failed", "Canceled" Condition: Always returned for a successful response. |
| statusUri | Cond | string | The URI to access the transaction status information. Condition: Always returned for a successful response. |
| deliveryDate | Cond | string | The transaction expected delivery date, populated for pending and completed transactions. Format: yyyy-MM-dd Condition: If the payment fails risk, deliveryDate is not returned. |
| cancelUri | Cond | string | The URI to access the transaction cancellation (if any). Condition: Returned if payment can be canceled. Only Pending payments can be canceled. <br> Condition: If the payment fails risk, cancelUri is not returned. |
| transactionType | Cond | string | Type of transaction. Valid values: Standard, Overnight, Expedited <br> Condition: If the payment fails risk, transactionType is not returned. |
| deliveryTrackingNumber | Cond | string | Tracking number of the transaction. Condition: If the payment fails risk, deliveryTrackingNumber is not returned. |
| nextAvailableTransactionDate | Cond | string | Next available transaction date. Condition: Returned only if the payment fails risk (transaction date is too early). |
| legacyTransactionId | Req | string | Server transaction timestamp. |

#### TransactionOutputDetail

| Parameter | Req | Data Type | Description |
|-----------|-----|-----------|-------------|
| amount | Req | number | The amount of the transaction. |
| automaticTransactionUri | Opt | string | The URI to the automatic transaction model. If this field is populated, this payment transaction is recurring. |
| billItemUri | Opt | string | The URI to the bill item associated with this transaction. Populated only if the payment is associated with an e-bill or bill store item. | 
| debitDate | Opt | string | Date that the consumer’s account was or is to be debited. Format: yyyy-MM-dd |
| fee | Opt | number | The fee amount for this transaction. |
| fundingAccountUri | Req | string | The source funding account URI for the given transaction. |
| note | Opt | string | A consumer’s “note to self.” This note is not submitted to the payee. |
| memo | Opt | string | Memo describing the payment. This text will be printed on the check sent to the payee for this payment. A payment memo is only used for payments that are to be processed via a paper check. |
| modifiableFields | Opt | Array of string | List of fields that can be changed for the transaction. For transactions that are in Pending status only. |
| payeeUri | Req | string | The URI to the payee that this transaction is associated with. |
| self | Cond | string | Relative Universal Resource Identifier pointing to the payment transaction itself. Condition: Always returned for successful response |
| id | Cond | string | Unique identifier of the payment transaction. Condition: Always returned for successful response |
| confirmationNumber | Req | string | Confirmation number for the payment transaction. |
| deliveryMethod | Opt | string | The delivery method for the transaction if the transaction has been processed. Valid values: "Electronic" "Paper" |
| status | Req | string | The status of the payment transaction. Valid values: "Pending", "Complete", “InProcess”, "Failed", "Canceled" <br> Typically, immediately upon the scheduling of a transaction, it will be in the Pending status. This is not the case when the transaction is scheduled as expedited SameDay, or if the transaction is funded by credit card (future release) or debit card for payment in the next five processing days. In those cases, the transaction is scheduled directly to InProcess. When a Pending transaction's delivery date becomes within the payee’s lead days (how long it takes to route the money given the delivery method), it moves to InProcess. When the funds are successfully delivered to the transaction destination, the status becomes Completed. If those funds are not delivered successfully, it instead becomes Failed. Finally, if the transaction is successfully canceled in the flow, it transitions to the Canceled status. |
| statusUri | Req | string | The URI to access the transaction status information. |
| deliveryDate | Opt | string | The transaction expected delivery date, populated for pending and completed transactions. Format: yyyy-MM-dd |
| cancelUri | Cond | string | The URI to access the transaction cancellation (if any). <br> Condition: Returned if payment can be canceled. This field should always be populated if a transaction is in Pending status. This field will also be populated if a transaction is funded by credit card (future release) or debit card, and the status is InProcess. |
| transactionType | Opt | string | Type of transaction. Valid values: Standard, Overnight, Expedited, ServiceFee <br> - If a transaction is scheduled to be paid with an overnight check, then the transaction type is Overnight. <br> - If a transaction is a SameDay electronic payment, then the type is Expedited. <br> - If a transaction corresponds to a service fee charged by an FI to a user, then the transaction type is ServiceFee. <br> <br> Otherwise, regardless of whether the transaction is sent electronically or as a paper check, or whether it is funded by a bank account or a card, it will have a transaction type of Standard. |
| deliveryTrackingNumber | Opt | string | Tracking number of the transaction. Only populated for a transaction with the transaction type Overnight. |
| legacyTransactionId | Req | string | Server transaction timestamp. |
| checkNumber | Cond | integer | Check number corresponding to completed paper payment. Condition: This is returned when the status of the transaction is Complete and the delivery method is Paper. |

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

| Parameter | Req | Data Type | Description |
|-----------|-----|-----------|-------------|
| address1 | Req | string | Address line 1 information. Length: 1-32 <br> Cannot contain any of the following special characters: &lt;&gt;=&() <br> Pattern: ^(?=.\*\\w)^[a-zA-Z0-9_@!+#.$,/%^ \*-]\* <br> At least one character must be an alphanumeric character. |
| address2 | Opt | string | Any additional address information. Max length: 32 <br> <p>Cannot contain any of the following special characters: &lt;&gt;=&() <br> Pattern: ^[a-zA-Z0-9_@!+#.$,/%^ \*-]\* |
| address3 | Opt | string | Any additional address information. Max length: 32 <br> Cannot contain any of the following special characters: &lt;&gt;=&() <br> Pattern: ^[a-zA-Z0-9_@!+#.$,/%^ \*-]\* <br> <strong>Payees GET</strong>: address3 is not currently returned in the response. |
| city | Req | string | The city name for the given address. Length: 1-32 <br> Cannot contain any of the following special characters: &lt;&gt;=&() <br> Pattern: ^[a-zA-Z0-9_ @!+#.$,%^\*-]\* <br> If the first three characters of the city name are "APO" or "FPO," the address is treated as a military address. |
| state | Req | string | The state for the given address. Valid characters: A–Z <br> Length: 2 <br> Pattern: ^[A-Z]\* |
| zipCode | Req | string | ZIP Code. Length: 5, 9, or 11 <br> Pattern: ^(\\d{5}\|\\d{9}\|\\d{11})$ <br> Valid characters: 0–9 <br> 5 digits are required for this field. 9 or 11 digits are optional. <br> Parsed: Chars 1-5 = Zip5 (must be a valid five-digit ZIP Code), Chars 6-9 = Zip4, Chars 10-11 = Zip2 |

#### UserV2

| Parameter | Req | Data Type | Description |
|-----------|-----|-----------|-------------|
| billingClass | Cond | string | If supplied, this field must contain a valid alphanumeric billing class as supplied by Fiserv during implementation. Condition: This field is required if Fiserv collects consumer fees on behalf of the client. |
| consumerTier | Opt | string | Tier level of consumer. Valid values: <br> 1 (default) <br> 2 <br> 3 |
| isAllowedToSolicit | Opt | boolean | Indicates whether or not the consumer may be solicited by Fiserv. This field allows a client to comply with state laws permitting a resident to prohibit solicitation in writing or by telephone. Valid values: <br> true – Yes, Fiserv can send information about additional products or services to this consumer. <br> false – No, Fiserv may not send solicitation information. This is the default. |
| isEmployee | Opt | boolean | Identifies whether a consumer is an employee of the client. Valid values: <br> true – Yes, an employee. <br> false – No, not an employee. This is the default. |
| sponsorBillingCategory | Cond | string | Clients that are doing their own billing but want to differentiate between consumers can use this field to communicate the consumer category to Fiserv. Condition: Based on sponsor setup. |
| userId | Req | string | ID used by the consumer to authenticate with their product(s). Length: 1–48 <br> This is also referred to as the CommonId. |
| hasPayees | Req | boolean | Indicates if the consumer has payees. This flag will return true when the consumer has active and/or inactive payees. |
| name | Req | [Name](#name) | Consumer’s name. | 
| businessName | Opt | string | Business name of the consumer. Length: 1–40 |
| contactEndPoints | Req | [ContactEndpoint](#contactendpoint) | Contact details of the consumer, such as the consumer’s email address, phone numbers, and address. |
| category | Opt | string | Client-assigned free-form category that identifies the group the consumer belongs to. Length: 1-32 | 
| sensitiveInformationUri | Req | string | URI for sensitive information. |
| timeZone | Req | integer | The time zone for the consumer. Defaults to “-05” for eastern standard time (EST) unless a value is provided. |
| country | Opt | string | The country of the consumer. |
| status | Req | string | The status of the consumer profile. <br> Valid values: Active, Inactive, FrozenFraud, FrozenOther, CancelledFraud, CancelledOther, VerificationNeeded, Unspecified |
| languageCode | Opt | string | Code representing the consumer’s language preference. Valid values: “EN” (English), “ES” (Spanish). “EN” (English) is the default value. |
| languageCountry | Opt | string | Country associated with the consumer’s language. Valid value: “US” (United States). “US” is the default value. |
| locale | Opt | string | The language code and associated language location in one combined field. Format: XX-XX (languageCode and languageCountry separated by hyphen). |
| newUser | Req | boolean | True if this is a new user; false if this is an existing user. |
| occupation | Opt | string | The activity a user spends time performing to earn a living. Length: 1-50 |
| modifiableFields | Req | Array of string | List of fields that can be changed for the user. |

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

| Parameter | Req | Data Type | Description |
|-----------|-----|-----------|-------------|
| description | Req | string | Description of an element that is part of the verification of the merchant relationship. Max length: 1000 <br> Pattern: [a-zA-Z0-9_()\\r\|\\n{}@!+#.\$,\%^ \*-]{1,1000}$ |
| value | Cond | string | Value of the element. Max length: 1000 <br> Pattern: [a-zA-Z0-9_()\\r\|\\n{}@!+#.\$,\%^ \*-]{1,1000}$ <br> Condition: Required on input when passing in a verification token. <br> For the Potential Payees Get API, this defaults to null unless specific data should be pre-populated. |
| type | Opt | string | The type of this value to provide a clear indication to the UI. Valid values: String, Number, Currency | 

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

2.  [Get Consumer Eligibility for Bill Discovery](#get-consumer-eligibility-for-bill-discovery) – If the
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


# Appendix B: Response Codes

This appendix contains the response codes and corresponding messages that can be returned.

## 0101 Errors
When a 0101 error (or equivalent) is returned, Fiserv returns the
parameter **field** in ResultInfo that contains the name of the field
that is missing or in error, if applicable. Fiserv also returns (if
applicable) the parameter **fieldPath** in ResultInfo that contains the
tags that the field causing the error is nested in.

For some requests, each item in a list has a unique ID so that the
response can be tied to the requested item in the list. If the request
resulted in an error in a list, the parameter **listItemId** tells the
caller which item in the list was in error.

POST Payees Example: If the Address1 is invalid, this is a sample
excerpt from the response:

    "result": {  
        "success": false,  
        "resultInfo": \[  
            {  
                "resultCategory": "Error",  
                "code": "0101",  
                "field": "Address1",  
                "fieldPath": "PayeeInfo.Address.Address1",  
                "listItemId": "1",  
                "description": "Invalid Address1. Cannot contain any of these special characters: <, >, =, &, (, or )."  
            }  
        \]  
    }

Note: If Address1 is found to be invalid after USPS standardization
(code-1 validation), the error 1207 is returned and should be handled
the same way as the 0101 error.

The field that caused the error is specified as Address1, while the path
(fieldPath) shows that Address1 is contained in PayeeInfo.

## CommonAPI Errors

Description: These errors may occur in multiple REST API responses.

| Error | Type | Error Message | Should user be invited to retry? (Y/N) |
|-------|------|---------------|----------------------------------------|
| 100 | ERROR | Indicates a required field was not provided in the request. | Y |
| 101 | ERROR | Messages will vary but all will indicate that an input was invalid, preventing the transaction from completing successfully. | Y |
| 375 | ERROR | Subscriber status prevents the transaction from being submitted. | N |

## Automatic Transactions

### POST AutomaticTransactions

| Error | Type | Error Message | Should user be invited to retry? (Y/N) | Error Category | Reason |
|-------|------|---------------|----------------------------------------|----------------|--------|
| 1110 | ERROR | Unable to find matching ebill. | N | Business Rule Violation | Either there is no matching ebill for the provided payee ID or the ebill is in rejected status |
| 1201 | ERROR|Payee not eligible for AutoPay | N | Business Rule Violation | Payee is not eligible for auto pay. Decided based on FirstEbillFlag or (TrialPeriodFlag and PaperQuestionIndicator) |
| 1341 | ERROR | Duplicate model already exists. | Y | Input Validation | User has already setup a similar automatic payment |
| 1403 | ERROR | Biller does not support auto-pay. | N | Business Rule Violation | Biller does not support auto-pay |
| 1404 | ERROR | Biller does not support requested auto-pay type. | N | Business Rule Violation|Transaction type is any of the AccountBalance, AmountDue and MinimumAmountDue and biller does not support particular transaction type of auto pay. |

### PATCH AutomaticTransactions

| Error | Type | Error Message | Should user be invited to retry? (Y/N) | Error Category | Reason |
|-------|------|---------------|----------------------------------------|----------------|--------|
| 1193 | ERROR | Modification not allowed as only the final payment is pending | Y | Business Rule Violation | The last payment date of the recurring model is on or after the final date of the recurring model |
| 1201 | ERROR | Payee is not eligible for auto-pay. | N | Business Rule Violation|Payee is not eligible for auto pay. Decided based on FirstEbillFlag or (TrialPeriodFlag and PaperQuestionIndicator) |
| 1403 | ERROR | Biller does not support auto-pay. | N | Business Rule Violation | Biller does not support auto-pay |
| 1404 | ERROR | Biller does not support requested auto-pay type. | N | Business Rule Violation | Transaction type is any of the AccountBalance, AmountDue and MinimumAmountDue and biller does not support particular transaction type of auto pay. |

### DELETE AutomaticTransactions

| Error | Type | Error Message | Should user be invited to retry? (Y/N) | Error Category | Reason |
|-------|------|---------------|----------------------------------------|----------------|--------|
| 1161 | ERROR | Cancel pending transactions is not allowed for bill automatic transactions | N | Business Rule Violation | Cancel pending transactions is not allowed for ebill automatic transactions |

## Bills

### PATCH Bills

| Error | Type | Error Message | Should user be invited to retry? (Y/N) | Error Category | Reason |
|-------|------|---------------|----------------------------------------|----------------|--------|
| 1422 | ERROR | Bill has not yet been viewed by the user | Y | Input Validation | Bill needs to be viewed by the User first before doing a patch which could be done by calling Get Bills |
| 1432 | ERROR | Invalid BillDueAlert status for dismissal. | N | Business Rule Violation|The Bill Due Alert is not found in the set of Unpaid Bill Due Alerts |

## ChallengerIdentity

### POST IdentityUserController

| Error | Type | Error Message | Should user be invited to retry? (Y/N) | Error Category |
|-------|------|---------------|----------------------------------------|----------------|
| 1701 | ERROR | Password encryption failed | N/A | N/A |
| 1702 | ERROR | Initiate OTP failed | N/A | N/A |
| 1703 | ERROR | OTP notification failed | N/A | N/A |
| 1704 | ERROR | User registration failed | N/A | N/A |
| 1706 | ERROR | Failed in storing accepted ldd id | N/A | N/A |

### GET MfaTokenController

| Error | Type | Error Message | Should user be invited to retry? (Y/N) | Error Category |
|-------|------|---------------|----------------------------------------|----------------|
| 1705 | ERROR | Inquire OTP failed | N/A | N/A |

### POST OtpAuthGrant

| Error | Type | Error Message | Should user be invited to retry? (Y/N) | Error Category |
|-------|------|---------------|----------------------------------------|----------------|
| 1800 | ERROR | Incorrect OTP | N/A | N/A |
| 1801 | ERROR | Expired OTP token | N/A | N/A |
| 1802 | ERROR | Exceeded the maximum number of OTP validation attempts | N/A | N/A |

## Ebill Activation

### GET Payees/{id}/ebillCapabilityGet

| Error | Type | Fields | Error Message | Should user be invited to retry? (Y/N) | Error Category | Reason |
|-------|------|--------|---------------|----------------------------------------|----------------|--------|
| 1405 | ERROR | NULL | Payee is not ebill capable | N | Business Rule Violation | Payee is not eligible for ebill |
| 1406 | ERROR | NULL | Biller did not respond | Y | Business Rule Violation | unexpected/general error |
| 1408 | ERROR | AccountNumber | The account number is not valid for the biller | Y | Input Validation | The account number does not match to the biller |
| 1409 | ERROR | NULL | Account number already active | N | Business Rule Violation | Same information already exists for the ebill |

### POST Payees/{id}/ebillService

| Error | Type | Fields | Error Message | Should user be invited to retry? (Y/N) | Error Category | Reason |
|-------|------|--------|---------------|----------------------------------------|----------------|--------|
| 1405 | ERROR | NULL | Payee is not ebill capable | N | Business Rule Violation | Payee is not eligible for ebill |
| 1406 | ERROR | NULL | Biller did not respond | Y | Business Rule Violation | unexpected/general error |
| 1408 | ERROR | AccountNumber | The account number is not valid for the biller | Y | Input Validation | The account number does not match to the biller |
| 1411 | ERROR | NULL | User did not accept terms and conditions | Y | Input Validation | Terms and conditions aren't accepted by the user |
| 1413 | ERROR | NULL | User canceled EBill activation | N | Business Rule Violation | User already canceled the ebill activation |
| 1415 | ERROR | NULL | Ebill is already active for this account number | N | Business Rule Violation | Same information already exists for the ebill |
| 1416 | ERROR | NULL | Ebill activation request timed out. Check again later. | Y | Input Validation | Time out issue. General error. |
| 1418 | ERROR | NULL | The account number is already activated for other subscriber | N | Input Validation | Valid account number is not given |

### DELETE Payees/{id}/ebillService

| Error | Type | Fields | Error Message | Should user be invited to retry? (Y/N) | Error Category | Reason |
|-------|------|--------|---------------|----------------------------------------|----------------|--------|
| 1405 | ERROR | NULL | Payee is not ebill capable | N | Improper implemention  | Payee is ineligible for ebill |
| 1406 | ERROR | NULL | Biller did not respond | Y | Business Rule Violation | unexpected/general error |
| 1412 | ERROR | NULL | Payee change pending | N | Business Rule Violation | unexpected/general error |
| 1417 | ERROR | NULL | Payee is not ebill active | Y | Input Validation | Payee is inactive for ebill |

### PATCH Payees/{id}/ebillService/SuppressPaper

| Error | Type | Fields | Error Message | Should user be invited to retry? (Y/N) | Error Category | Reason |
|-------|------|--------|---------------|----------------------------------------|----------------|--------|
| 1406 | ERROR | NULL | Biller did not respond | Y | Business Rule Violation | unexpected/general error |
| 1412 | ERROR | NULL | Payee change is pending | N | Business Rule Violation | unexpected/general error |
| 1417 | ERROR | NULL | Payee is not ebill active | Y | Input Validation | ebill is not enabled for payee |
| 1419 | ERROR | NULL | Payee not eligible for paper suppression | N | Business Rule Violation | Payee not eligible for paper supression |
| 1420 | ERROR | NULL | User canceled paper suppression | N | Business Rule Violation | User already canceled the paper suppression |
| 1421 | ERROR | AcceptedTC | User did not accept terms and conditions | Y | Input Validation | T&C needs to be accepted |

## Financial Accounts

### POST CardAccounts

| Error | Type | Error Message | Should user be invited to retry? (Y/N) | Error Category | Reason |
|-------|------|---------------|----------------------------------------|----------------|--------|
| 1337 | ERROR | Duplicate Card Found | Y | Input Validation | Card account already exists for the user | 
| 1350 | ERROR | Subscriber has reached same card add limit across multiple profiles | N | Business Rule Violation | If the same card is added in the multiple profiles exceeding the add limit |
| 1356 | ERROR | Card verification is not successful. | Y | Input Validation | When CVV verification fails for this card account |
| 1357 | ERROR | Unable to process request. This feature is not supported. | N | Business Rule Violation | When card payment is not supported. |
| 1358 | ERROR | Sponsor restrictions do not allow this action to be performed. | N | Business Rule Violation | When the sponsor setting is not enabled for card funded payment |
| 1359 | ERROR | Subscriber has reached rolling time frame card add limit. | Y | Business Rule Violation | When subscriber has reached the rolling time frame card add limit which will be set up in configuration,for example 1 card in 30 days. Customer care can be reached or for rolling time period you can add that after the time period is crossed, card will be allowed to add |
| 1360 | ERROR | Subscriber has reached active card add limit. | N | Business Rule Violation | When subscriber has reached the active card add limit which will be set up in configuration. |
| 1361 | ERROR | Subscriber has reached same card delete and re-add limit. | N | Business Rule Violation | When subscriber has reached the same card delete and re-add limit which will be set up in configuration. |

### PUT CardAccounts

| Error | Type | Error Message | Should user be invited to retry? (Y/N) | Error Category | Reason |
|-------|------|---------------|----------------------------------------|----------------|--------|
| 1336 | ERROR | Card Account Not Found | N | Improper implementation | Unable to locate card account with the given commonId |
| 1346 | ERROR | Credit Card Number Failed Scheme Validation | Y | Input Validation | Specified credit card number is invalid in the request |
| 1355 | ERROR | Operation is not allowed. Originator type does not match | N | Improper implementation | Access restricted for the user to modiify the card account details |
| 1356 | ERROR | Card verification is not successful. | Y | Input Validation | When CVV verification fails for this card account |
| 1358 | ERROR | Sponsor restrictions do not allow this action to be performed. | N | Business Rule Violation | Sponsor settings does not allows to add a card. Below are the settings that impact adding the card: FirstPartyDebit, FirstPartyCredit, SubscriberToAddFirstPartyCredit, SubscriberToAddFirstPartyDebit |

### DELETE CardAccounts

| Error | Type | Error Message | Should user be invited to retry? (Y/N) | Error Category | Reason |
|-------|------|---------------|----------------------------------------|----------------|--------|
| 1336 | ERROR | Card Account Not Found | N | Improper implementation | Unable to locate card account with the given commonId |
| 1355 | ERROR | Operation is not allowed. Originator type does not match | N | Improper implementation | Access restricted for the user to delete the card account details |
| 1358 | ERROR | Sponsor restrictions do not allow this action to be performed. | N | Business Rule Violation | When the sponsor setting is not enabled for card funded payment |

### GET CardAccounts

| Error | Type | Error Message | Should user be invited to retry? (Y/N) | Error Category | Reason |
|-------|------|---------------|----------------------------------------|----------------|--------|
| 1358 | ERROR | Sponsor restrictions do not allow this action to be performed. | N | Business Rule Violation | When the sponsor setting is not enabled for card funded payment |
### PATCH CardAccounts
| Error | Type | Error Message | Should user be invited to retry? (Y/N) | Error Category | Reason |
|-------|------|---------------|----------------------------------------|----------------|--------|
| 1358 | ERROR | Sponsor restrictions do not allow this action to be performed. | N | Business Rule Violation | When the sponsor setting is not enabled for card funded payment |

### POST BankAccounts

| Error | Type | Error Message | Should user be invited to retry? (Y/N) | Error Category | Reason |
|-------|------|---------------|----------------------------------------|----------------|--------|
| 1368 | ERROR | Bank account has to be added to user before Card account can be added. | Y | Input Validation | When user has not added a bank account and he tries to add a new card account. |
| 1370 | ERROR | This Account type cannot be set to the preferred account | Y | Input Validation | When the account type selected cannot be set as "Preferred" account |
| 1375 | ERROR | Duplicate Bank Account | Y | Input Validation | When the bank account details exist with the same routing number, account type and account number. |
| 1378 | ERROR | Maximum Number of Bank Account Services Exceeded | N | Business Rule Violation | The maximum bank account add limit has been reached |
| 1380 | ERROR | User Id does not exist | N | Improper implementation | Unable to locate the challenger user ID with the given commonId |

### GET BankAccounts

| Error | Type | Error Message | Should user be invited to retry? (Y/N) | Error Category | Reason |
|-------|------|---------------|----------------------------------------|----------------|--------|
| 1380 | ERROR | User Id does not exist | N | Improper implementation | Unable to locate the challenger user ID with the given commonId |

### PATCH BankAccounts

| Error | Type | Error Message | Should user be invited to retry? (Y/N) | Error Category | Reason |
|-------|------|---------------|----------------------------------------|----------------|--------|
| 1382 | ERROR | Unable to modify. User is allowed to modify only nick name for trusted bank account. | N | Business Rule Violation | Subscriber is allowed to modify only nick name for trusted bank accounts |
| 1385 | ERROR | StartingCheckNumber field is required when the Originator type is FiCustomerCare/FiservCustomerCare. | Y | Input Validation | When Originator type is FiCustomerCare/FIservCustomerCare, the field "StartCheckNumber" value is required while updating |
| 1386 | ERROR | The preferred account flag cannot be modified to false, set another account to true and this will change to false. | Y | Input Validation | When the user has single bank account and the preferred flag is changed to false |
| 1387 | ERROR | IsBusiness flag cannot be updated for confirmed accounts. | NA | Not applicable | Removed now, will not be returned |

### DELETE BankAccounts

| Error | Type | Error Message | Should user be invited to retry? (Y/N) | Error Category | Reason |
|-------|------|---------------|----------------------------------------|----------------|--------|
| 1383 | ERROR | Cannot delete last banking account | Y | Input Validation | When the user has single trusted bank account, it cannot be deleted. If another account is added, then this can be deleted |
| 1384 | ERROR | Bank Account does not exist to be deleted | N | Improper implementation | Unable to locate the bank account with the given commonId |
| 1391 | ERROR | The billing account cannot be deleted | Y | Input Validation | The billing account cannot be deleted. If the billing identifier is moved to another bank account, then this can be deleted. |

## Merchants

### GET Merchants

| Error | Type | Error Message | Should user be invited to retry? (Y/N) | Error Category | Reason |
|-------|------|---------------|----------------------------------------|----------------|--------|
| 1500 | ERROR | No payee found based on search criteria | Y | Input Validation | Unable to locate payee with the filter query parameters |
| 1504 | ERROR | Invalid Sponsor | N | Improper implemention  | Specified sponsor ID is invalid in the request |

## Messages

### POST TransactionMessages

| Error | Type | Field | Error Message | Should user be invited to retry? (Y/N) | Error Category | Reason |
|-------|------|-------|---------------|----------------------------------------|----------------|--------|
| 101 | Error | NULL | Could not add transaction message | Y | PMA Service error | Call to PMA fails |
| 102 | Error | TransactionID | Transaction is not in valid date range | N | Business rule validation | Expired transaction |
| 103 | Error | TransactionID | Transaction Inquiries cannot be made for in-process, canceled or pending payments | N | Business rule validation | Invalid transaction status |
| 104 | Error | NULL | Transaction ID does not exist | N | Input validation | Invalid transaction |

### DELETE Messages

| Error | Type | Field | Error Message | Should user be invited to retry? (Y/N) | Error Category | Reason |
|-------|------|-------|---------------|----------------------------------------|----------------|--------|
| 999 | Error  | NULL  | General server error | N | Input validation  | Message already deleted |
| 1104 | Error | NULL | Message ID does not exist | N | Input validation | Invalid message |
| 1105 | Error | NULL | Error on marking the delete message | Y | PMA Service error  | Call to PMA fails |

### GET Messages

| Error | Type | Field | Error Message | Should user be invited to retry? (Y/N) | Error Category | Reason |
|-------|------|-------|---------------|----------------------------------------|----------------|--------|
| 1102 | Error | NULL | Could not find message detail | Y | PMA Service error  | Call to PMA fails |
| 1103 | Error | TransactionID | Cannot find message header | N | Input validation | Transaction ID does not exist |
| 1104 | Error | NULL | Message ID does not exist | N | Input validation  | Invalid message |

## Payees

### POST Payees

| Error | Error Message | Field | ResultInfo.result Category | Error (0101 field validation error) | 0101 Field | Should user be invited to retry? (Y/N) | Error Category | Reason |
|-------|---------------|-------|----------------------------|-------------------------------------|------------|----------------------------------------|----------------|--------|
| 1207 (handle as 0101) | Payee Address1 is invalid | Address1 | Error | 0101 | Address1 | Y | Input Validation | Payee address1 is invalid <br> 1207 is Code-1 validation error |
| 1212 (handle as 0101) | City name is invalid | NULL | NULL | 0101 | City | Y | Input Validation | City name is invalid <br> 1212 is Code-1 validation error |
| 1213 (handle as 0101) | State code is invalid | State | Error | 0101 | State | Y | Input Validation | State code is invalid <br> 1213 is Code-1 validation error |
| 1214 | Payee is inactive. | NULL | NULL |   |   | N | Input Validation | Inactive Payee <br> Not a real world scenario. There is no need to handle this error in the UI. (General server error) |
| 1215 | Active payee already exists | NULL | NULL |   |   | Y | Input Validation | Payee already exists with the same information |
| 1216 | Merchant number is invalid | Merchant number | NULL |   |   | N | Input Validation | Merchant number is invalid <br> Not a real world scenario. If the client's UI is implemented correctly, there is no need to  handle this error in the UI. |
| 1218 | Account scheming failed | NULL | Error |   |   | Y | Business Rule Violation | More of an unexpected/general error |
| 1221 (handle as 0101) | Unit Number is missing in the Address | Address2 | Error | 0101 | Address2 | Y | Input Validation | Unit number is not passed in the address <br> 1221 is Code-1 validation error |
| 1222 (handle as 0101) | House or Street number is missing | Address1 | Error | 0101 | Address1 | Y | Input Validation | House or street number is not given <br> 1222 is Code-1 validation error |
| 1224 (handle as 0101) | Directional in Address field is missing | Address1 | Error | 0101 | Address1 | Y | Input Validation | Directional is not given <br> 1224 is Code-1 validation error |
| 1226 (handle as 0101) | Suffix in the Address field is missing (St-Street,RD-Road,CT-Court) | Address1 | Error | 0101 | Address1 | Y | Input Validation | Suffix is not given for the address <br> 1226 is Code-1 validation error |
| 1229 (handle as 0101) | Zipcode and Address is mismatch | NULL | NULL | 0101 | Address1 | Y | Input Validation | ZIP Code does not match with the address <br> 1229 is Code-1 validation error |
| 1230 | Invalid Payee Status | Payee Status | Error |   |   | N | Input Validation | Payee status is invalid. A different payee can be tried. <br> Not a real world scenario. If the client's UI is implemented correctly, there is no need to  handle this error in the UI. (General server error) |
| 1231 | Invalid VRU Name Code | VRU Name Code | Error |   |   | N | Input Validation | Not a real world scenario. There is no need to handle this error in the UI. (General server error) |
| 1254 | Payee was added successfully but does not have overnight capabilities - Address is not valid for overnight check | NULL | WARNING |   |   | Y | Input Validation | Payee address is invalid to support overnight check |
| 1256 | SourceUriPayeeZipcode must be provided for the provided SourceUri. | SourceUriPayee Zipcode | ERROR |   |   | Y | Input Validation | SourceUriPayeeZipcode is not provided for source URI |
| 1270 | Payee was added successfully but does not have overnight capabilities - Invalid zip code | Zip5 | WARNING |   |   | Y | Input Validation | ZIP Code is invalid and does not support overnight capabilities |
| 1271 | Payee was added successfully but does not have overnight capabilities - Overnight Payee Address1 is invalid | NULL | WARNING |   |   | Y | Input Validation | Address1 does not support overnight capabilities |
| 1272 | Payee was added successfully but does not have overnight capabilities - Invalid zip code. | NULL | WARNING |   |   | Y | Input Validation | ZIP Code is invalid and  does not support overnight capabilities |
| 1273 | Payee was added successfully but does not have overnight capabilities - City name is invalid. | NULL | WARNING |   |   | Y | Input Validation | City name is invalid and does not support overnight capabilities |
| 1274 | Payee was added successfully but does not have overnight capabilities - State code is invalid | NULL | WARNING |   |   | Y | Input Validation | State code is invalid and does not support overnight capabilities |
| 1275 | Payee was added successfully but does not have overnight capabilities - Unit number is missing in the address. | Address1 | WARNING |   |   | Y | Input Validation | Unit number, house or street number is not passed in the address |
| 1276 | Payee was added successfully but does not have overnight capabilities - Zipcode and Address is mismatch. | NULL | WARNING |   |   | Y | Input Validation | ZIP Code does not match with the address |
| 1277 | Payee was added successfully but does not have overnight capabilities - Overnight address cannot be military address. | OvernightAddress | WARNING |   |   | Y | Input Validation | Military address does not support overnight capabilities |

### DELETE Payees

| Error | Error Message | Field | ResultInfo.result Category | Should user be invited to retry? (Y/N) | Error Category | Reason | Real world scenario? |
|-------|---------------|-------|----------------------------|----------------------------------------|----------------|--------|-----------------------|
| 1214 | Payee is not active. | NULL | ERROR | N | Input Validation | Inactive Payee | No. If the client's UI is implemented correctly, there is no need to  handle this error in the UI. (General server error) |
| 1240 | Invalid Subscriber payee number | NULL | ERROR | N | Input Validation | Invalid payee number for subscriber | No. If the client's UI is implemented correctly, there is no need to  handle this error in the UI. (General server error) |
| 1243 | Cannot cancel a payee with pending payments when the CancelPayments indicator is false | NULL | ERROR | N | Input Validation | Payee with pending payments cannot be deleted as the indicator is set to false | No. If the client's UI is implemented correctly, there is no need to  handle this error in the UI. (General server error) |
| 1244 | Cannot cancel ebill enabled payee. | NULL | ERROR | N | Business Rule Violation |   |  |
| 1258 | Payee cannot be deleted as ebill service cancellation failed | NULL | ERROR | Y | Business Rule Violation | Unexpected/general error | This is a system failure error and cannot be tested. (General server error) |
| 1259 | Payee cannot be deleted as recurring transaction model delete failed | NULL | ERROR | Y | Business Rule Violation | Unexpected/general error | This is a system failure error and cannot be tested. (General server error) |
| 1260 | Payee cannot be deleted as Cancel Ebill Autopay Failed | NULL | ERROR | Y | Business Rule Violation | Unexpected/general error | This is a system failure error and cannot be tested. (General server error) |
| 1264 | Ebill Pending Payees cannot be deleted | NULL | ERROR | N | Business Rule Violation | Pending ebill cannot be deleted| |

### PATCH Payees

| Error | Error Message | Field | ResultInfo.result Category | Error (0101 field validation error) | 0101 Field | Should user be invited to retry? (Y/N) | Error Category | Reason |
|-------|---------------|-------|----------------------------|-------------------------------------|------------|----------------------------------------|----------------|--------|
| 1205 (handle as 0101) | Invalid payee account number | AccountNumber | ERROR | 0101 | AccountNumber | Y | Input Validation | Payee account number is invalid |
| 1207 (handle as 0101) | Invalid payee address1 | Address1 | ERROR | 0101 | Address1 | Y | Input Validation | Payee address1 is invalid <br> 1207  is Code-1 validation error |
| 1208 (handle as 0101) | Invalid zip code | Zipcode | ERROR | 0101 | Zipcode | Y | Input Validation | ZIP Code is invalid <br> 1208 is Code-1 validation error |
| 1213 (handle as 0101) | State code is invalid | State | ERROR | 0101 | State | Y | Input Validation | State code is invalid <br> 1213 is Code-1 validation error |
| 1214 | Payee is inactive | NULL | ERROR |   |   | Y | Input Validation | Payee is inactive <br> Not a real world scenario. If the client's UI is implemented correctly, there is no need to  handle this error in the UI. (General server error) |
| 1216 | Merchant number is invalid | NULL | ERROR |   |   | Y | Input Validation | Incorrect merchant number is given <br> Not a real world scenario. If the client's UI is implemented correctly, there is no need to  handle this error in the UI. |
| 1222 (handle as 0101) | House or Street number is missing | Address1 | ERROR | 0101 | Address1 | Y | Input Validation | House or street number is not given <br> 1222 is Code-1 validation error |
| 1224 (handle as 0101) | Directional in Address field is missing | Address1 | ERROR | 0101 | Address1 | Y | Input Validation | Directional is not given <br> 1224 is Code-1 validation error |
| 1226 (handle as 0101) | Suffix in the Address field is missing (St-Street,RD-Road,CT-Court) | Address1 | ERROR | 0101 | Address1 | Y | Input Validation | Suffix is not given for the address <br> 1226 is Code-1 validation error |
| 1228 (handle as 0101) | Street Name or Street Suffix is invalid | Address1 | ERROR | 0101 | Address1 | Y | Input Validation | Valid street name or street suffix is invalid <br> 1228  is Code-1 validation error |
| 1229 (handle as 0101) | Zipcode and Address is mismatch | Address | ERROR | 0101 | Address1 | Y | Input Validation | ZIP Code does not match with the address <br> 1229 is Code-1 validation error |
| 1233 | Target bank account is missing or invalid | NULL | ERROR |   |   | N | Input Validation | Provided bank account common ID is invalid <br> Not a real world scenario. If the client's UI is implemented correctly, there is no need to  handle this error in the UI. (General server error) |
| 1240 | Invalid Subscriber payee number | NULL | ERROR |   |   | Y | Input Validation | Payee number is not valid <br> Not a real world scenario. If the client's UI is implemented correctly, there is no need to  handle this error in the UI. |
| 1249 | Payee was modified successfully but the overnight address did not change - Address is not valid for overnight check | OvernightAddress | WARNING |   |   | Y | Input Validation | Address is invalid |
| 1250 | Payee was modified successfully but the overnight capabilities were not added/modified - Overnight address cannot be military address. | OvernightAddress | WARNING |   |   | Y | Input Validation | Payee was modified successfully but the military address is not valid for overnight check |
| 1253 | The request contained non modifiable fields. | NULL | ERROR |   |   | Y | Improper implemention  | Non-modifiable fields cannot be edited |
| 1255 | Payee was modified successfully but the overnight address did not change - Overnight Payee Address1 is invalid. | Address1 | WARNING |   |   | Y | Input Validation | Payee was modified successfully but the address is not valid for overnight check |
| 1257 | Payee was modified successfully but the overnight address did not change - Invalid zip code. | Zip5 | WARNING |   |   | Y | Input Validation | Payee was modified successfully but the address is not valid for overnight check |
| 1265 | Cannot modify payee | NULL | ERROR |   |   | Y | Business Rule Violation | Unexpected/general error |
| 1268 | Cannot modify personal payees | NULL | ERROR |   |   | N | Business Rule Violation | Unexpected/general error |
| 1278 | Payee was modified successfully but the overnight address did not change - City name is invalid. | City | WARNING |   |   | Y | Input Validation | Payee was modified successfully but the address is not valid for overnight check |
| 1278 | Overnight city name is missing or invalid | City | ERROR |   |   | Y | Input Validation | City name is missing or invalid |
| 1279 | Payee was modified successfully but the overnight address did not change - State code is invalid. | State | WARNING |   |   | Y | Input Validation | Payee was modified successfully but the address is not valid for overnight check |
| 1279 | Overnight state code is missing or invalid | State | ERROR |   |   | Y | Input Validation | State code is missing or invalid |
| 1280 | Payee was modified successfully but the overnight address did not change - Zipcode and Address is a mismatch. | NULL | WARNING |   |   | Y | Input Validation | Payee was modified successfully but the address is not valid for overnight check |

### GET Payees

| Error | Error Message | Field | ResultInfo.result Category | Should user be invited to retry? (Y/N) | Error Category | Real world scenario? |
|-------|---------------|-------|----------------------------|----------------------------------------|----------------|-----------------------|
| 375 | Subscriber status prevents this action from being completed | Null | ERROR | N | Input Validation | No. If the client's UI is implemented correctly, there is no need to  handle this error in the UI. |

## Payee Groups

### POST PayeeGroups

| Error | Type | Error Message | Field | Should user be invited to retry? (Y/N) | Error Category | Reason |
|-------|------|---------------|-------|----------------------------------------|----------------|--------|
| 101 | ERROR | Payee group already exists. | NULL | Y | Input Validation | A user cannot have duplicate payee group names |
| 101 | ERROR | Payee does not exist. | NULL | Y | Input Validation | All payees in the payee array must exist |
| 101 | ERROR | Payee is invalid. | NULL | Y | Input Validation | Invalid data passed into payee array |
| 101 | ERROR | Payee exists in another group. | NULL | Y | Input Validation | A payee cannot be assigned to more than one payee group |
| 101 | ERROR | Error creating payee group. | NULL | Y | Input Validation | An error occurred while attempting to create the payee group |
| 1214 | ERROR | Payee is inactive. | NULL | Y | Input Validation | All payees in the payee array must be active |

### DELETE PayeeGroups

| Error | Type | Error Message | Field | Should user be invited to retry? (Y/N) | Error Category | Reason |
|-------|------|---------------|-------|----------------------------------------|----------------|--------|
| 101 | ERROR | Error deleting payee group. | NULL | Y | Input Validation | The “id” in the request is invalid or missing |

### PUT PayeeGroups

| Error | Type | Error Message | Field | Should user be invited to retry? (Y/N) | Error Category | Reason |
|-------|------|---------------|-------|----------------------------------------|----------------|--------|
| 101 | ERROR | Payee Group ID is invalid | NULL | Y | Input Validation | The “id” in the request is invalid or missing |
| 101 | ERROR | Payee Group name already exists. | NULL | Y | Input Validation | A user cannot have duplicate payee group names |
| 101 | ERROR | Payee does not exist. | NULL | Y | Input Validation | All payees in the payee array must exist |
| 101 | ERROR | Payee is invalid. | NULL | Y | Input Validation | Invalid data passed into payee array |
| 101 | ERROR | Payee exists in another group. | NULL | Y | Input Validation | A payee cannot be assigned to more than one payee group |
| 1214 | ERROR | Payee is inactive. | payeeUri | Y | Input Validation | All payees in the payee array must be active |

## Potential Payees

### GET PotentialPayees	

| Error | Type | Error Message | Field | Should user be invited to retry? (Y/N) | Error Category | Reason |
|-------|------|---------------|-------|----------------------------------------|----------------|--------|
| 1401 | ERROR | No valid consents were found | NULL | Y | Input Validation | User has not accepted the Consents before making a Get call to Potential payees. |
| 1433 | WARNING | There was an error enrolling the user in BillDiscovery | NULL | Y | Business Rule Violation | The payee added however subscriber was not successfully enrolled in bill discovery |
| 1434 | WARNING | There was an error enrolling the user in Bill Due Alerts | NULL | Y | Business Rule Violation | The payee was added and enrolled in bill discovery however subscriber was not enrolled in bill due alerts for payee |

### PUT PotentialPayees/{id}/Verify

| Error | Type | Error Message | Field | Should user be invited to retry? (Y/N) | Error Category | Reason |
|-------|------|---------------|-------|----------------------------------------|----------------|--------|
| 1427 | ERROR | Provided account information is invalid. | NULL | Y | Input Validation | Verification failed due to incorrect information given. |
| 1428 | WARNING | Cannot add potential payee - active payee already exists. | NULL | N | Business Rule Violation | Payee already exists with the same information for current potential payee. |
| 1430 | WARNING | Error updating potential payee. | NULL | Y | Business Rule Violation | payee was added but the potential payee was not inactivated which could result in the potential payee still being returned or some other unexpected error. |
| 1433 | WARNING | There was an error enrolling the user in BillDiscovery | NULL | Y | Business Rule Violation | The payee added however subscriber was not successfully enrolled in bill discovery |
| 1434 | WARNING | There was an error enrolling the user in Bill Due Alerts | NULL | Y | Business Rule Violation | The payee was added and enrolled in bill discovery however subscriber was not enrolled in bill due alerts for payee |

## Potential Payee Scheming

### POST PotentialPayeeSchemingService

| Error | Type | Error Message | Should user be invited to retry? (Y/N) | Error Category | Reason |
|-------|------|---------------|----------------------------------------|----------------|--------|
| 1435 | ERROR | Search for credit bureau data unsuccessful. | N | Business Rule Violation | Unable to fetch or retrive data from credit bureau bill task |
| 1436 | ERROR | Search for bill discovery data unsuccessful. | N | Business Rule Violation | Unable to fetch or retrive data from bill discovery bill task |
| 1437 | ERROR | Search for merchant unsuccessful. | N | Business Rule Violation | Unable to retrieve Merchant Info with the given Merchant Name, Account Number and Zip details |

## Reminders

### POST Reminders

| Error | Type | Error Message | Field | Should user be invited to retry? (Y/N) | Error Category | Reason |
|-------|------|---------------|-------|----------------------------------------|----------------|--------|
| 101 | ERROR | A reminder already exists for this payee | PayeeUri | N | Business Rule Validation | A payee can only have one reminder model |
| 101 | ERROR | Cannot add a reminder because ebill is active for this payee | PayeeUri | N | Business Rule Validation | A reminder cannot be set up if ebill is enabled for the payee |
| 101 | ERROR | Cannot add a reminder because a recurring transaction model exists for this payee | PayeeUri | N | Business Rule Violation | A reminder cannot be set up if an automatic (recurring) payment model is enabled for the payee |
| 101 | ERROR | Invalid payee | PayeeUri | Y | Input Validation | The payee does not exist |
| 101 | ERROR | Invalid PayeeUri | PayeeUri | Y | Input Validation | An invalid PayeeUri has been provided |
| 376 | ERROR | Subscriber email status prevents this action from being completed | NULL | N | Business Rule Violation | A valid email address is required for the user if one or more of the reminder alerts has been selected |

### DELETE Reminders

| Error | Type | Error Message | Field | Should user be invited to retry? (Y/N) | Error Category | Reason |
|-------|------|---------------|-------|----------------------------------------|----------------|--------|
| 101 | ERROR | A reminder model does not exist for this payee | NULL | N | Input Validation | A reminder model does not exist for this payee |
| 101 | ERROR | This reminder model was deleted due to ebill activation | NULL | N | Input Validation | Reminder models are deleted when ebill service is activated for the payee |
| 101 | ERROR | Invalid reminder Id | NULL | Y | Input Validation | An invalid reminder ID has been provided |

### PATCH Reminders

| Error | Type | Error Message | Field | Should user be invited to retry? (Y/N) | Error Category | Reason |
|-------|------|---------------|-------|----------------------------------------|----------------|--------|
| 101 | ERROR | Invalid reminder id | NULL | Y | Input Validation | An invalid reminder ID has been provided |
| 101 | ERROR | This reminder model does not exist due to ebill activation | NULL | Y | Input Validation | Reminder models are deleted when ebill service is activated for the payee |
| 101 | ERROR | A reminder model does not exist for this payee | NULL | Y | Input Validation | A reminder model does not exist for this payee |
| 376 | ERROR | Subscriber email status prevents this action from being completed | NULL | N | Business Rule Violation | A valid email address is required for the user if one or more of the reminder alerts has been selected |
| 2000 | ERROR | A reminder model does not exist for this payee | NULL | Y | Input Validation | A Reminder model does not exist for the payee |

## ToDos

### PATCH ToDos

| Error | Type | Error Message | Should user be invited to retry? (Y/N) | Error Category | Reason |
|-------|------|---------------|----------------------------------------|----------------|--------|
| 1432 | ERROR | Invalid todo status for dismissal. | N | Improper implemention  | 1. When potential payee is not active status.  Generally should not receive this because an inactive potential payee / todo will not come back in get. <br> 2. When bill payment status other than Unpaid. |

## Transactions

### POST Transactions

| Error | Type | Error Message | Should user be invited to retry? (Y/N) | Error Category | Reason |
|-------|------|---------------|----------------------------------------|----------------|--------|
| 0 | INFO | Transaction could have been made, but not attempted due to validation failure of other transaction(s). | Y | N/A | This is a successful transaction in a call where other transactions have failed. No action is required on a transaction with this response code. |
| 1100 | ERROR | There was an error processing one or more of the transaction inputs.  Check individual results for details. | Y | Input Validation | This error will return when one or more transactions in the request had an error. None of the transactions were posted successfully. Review all transactions for potential errors, correct the data, and retry. |
| 1101 | WARNING | One or more transactions failed.  Examine individual items for detailed errors or scheduled transactions. | Y | Input Validation | This error will return when one or more transactions in the request had an error. One or more of the transactions posted successfully, but some failed and may need to be retried. Check the additional errors returned for indication on if the failed transactions can be retried. |
| 1107 | ERROR | Payee not eligible for overnight transaction due to invalid address. | Y | Input Validation | If payee is marked as ONC with no address this exception will occur |
| 1113 | ERROR | Duplicate transaction rejected. | Y | Input Validation | Similar transaction already exists |
| 1121 | ERROR | Transaction failed risk assessment. | N | Business Rule Violation | Failed in Fraud Assessment (Potential Fraud) |
| 1122 | ERROR | Transaction Denied. Daily limit exceeded. | Y | Business Rule Violation | Can be tried the next day |
| 1131 | ERROR | Error scheduling transaction in expedited system. | N | Business Rule Violation | This error will thrown if any exception occurs while attempting to validate expedited payment |
| 1153 | WARNING | Unable to add transaction note. | N | System Failure | This warning is thrown when some issue is faced adding a note for the transaction. |
| 1155 | WARNING | There was an error updating the bill status on Bill Discovery | N | Business Rule Violation | If any exceptions happened while updating bill due status; this is a warning and not an error. |
| 1156 | ERROR | An error occurred debiting funds from card. | N | Business Rule Violation | There was an issue while debiting funds from the card account |
| 1157 | ERROR | Error fully processing card-funded transaction. | N | Business Rule Violation | If any failure in updating transaction status this error will return |
| 1158 | ERROR | An error occurred scheduling card-funded transaction on provided delivery date. | N | Business Rule Violation | The payment date and backend processing date do not match for card funded transaction |
| 1159 | ERROR | Card account type unsupported for transaction destination. | Y | Business Rule Violation | This card type is not supported. Can be retried with different card type. |
| 1160 | ERROR | Card transaction unsupported. | Y | Business Rule Violation | Card payments not allowed. Can be retried with bank account. |

### PATCH Transactions

| Error | Type | Error Message | Should user be invited to retry? (Y/N) | Error Category | Reason |
|-------|------|---------------|----------------------------------------|----------------|--------|
| 101 | ERROR | Unable to find matching payee. | Y | Input Validation | Incorrect payee |
| 1113 | ERROR | Duplicate transaction rejected. | Y | Input Validation | Similar transaction already exists |
| 1121 | ERROR | Transaction failed risk assessment. | N | Business Rule Violation | Failed in Fraud Assessment (Potential Fraud) |
| 1143 | ERROR | Invalid transaction status. | N | Improper Implementation | The transaction status does not allow it to be modified |
| 1146 | ERROR | Invalid Ebill Id. | N | Business Rule Violation | Ebill ID passed with transaction is not valid |
| 1151 | ERROR | The request contained changes to non-modifiable fields. | Y | Input Validation | Request to be changed to remove changes to non-modifiable fields |
| 1152 | WARNING | No modifications in request. | Y | Input Validation | Correcting the request would fix the issue. There should be proper valid values to be modified. |
| 1153 | WARNING | Transaction note not updated. | N | Business Rule Violation | Some issue faced while updating note of transaction |
| 1214 | ERROR | Payee is inactive. | N | Improper Implementation | Error is thrown when Inactive Payee is modified. |

## Transaction Calendar

### GET TransactionCalendar

| Error | Type | Error Message | Should user be invited to retry? (Y/N) | Error Category | Reason |
|-------|------|---------------|----------------------------------------|----------------|--------|
| 100 | ERROR | TransactionDestinationUri or TransactionUri must be provided | N | Improper Implementation | Invalid data in request |
| 100 | ERROR | Tenant/User information missing in request | N | Improper Implementation | Invalid data in request |
| 1140 | ERROR | Transaction cannot be found. | N | Improper Implementation | Unable to locate the Transaction Calender with the provided CommonId |

## Users

### GET Users

| Error | Type | Error Message | Field | Should user be invited to retry? (Y/N) | Error Category | Reason |
|-------|------|---------------|-------|----------------------------------------|----------------|--------|
| 1000 | ERROR | Tenant does not support Bill Discovery. | NULL | N | Business Rule Violation | If Tenant does not opt for billdiscovery this error will return. |
| 1001 | ERROR | Bill Discovery feature disabled. | NULL | N | Business Rule Violation | Bill dicovery is not enabled |

### POST Users

| Error | Type | Error Message | Field | Should user be invited to retry? (Y/N) | Error Category | Reason |
|-------|------|---------------|-------|----------------------------------------|----------------|--------|
| 1002 | ERROR | User enrollment failed due to taxId match of an existing, cancelled user. | NULL | Y | Input Validation | Similar user already exists |
| 1003 | ERROR | Legal document agreement add failed | NULL | N | Business Rule Violation | LegalDisclosureAgreementAdd to get the legal document for the accepted ldd ID |
| 1004 | ERROR | Legal document not found | NULL | N | Business Rule Violation | Particular document is not found causes this error |
| 1005 | ERROR | Invalid legal document ID | NULL | N | Business Rule Violation | Supplied document ID is incorrect. |
| 1006 | ERROR | Failed in access token revocation | NULL | Y | Business Rule Violation | More of an unexpected / general error |
| 1007 | ERROR | Failed in access token update | NULL | Y | Business Rule Violation | Accessing the token get failed. |
| 1008 | ERROR | ContactEndpoint type for email must be submitted | ContactEndpoints.Email | Y | Input Validation | Type of Contactendpoint must be specified. |
