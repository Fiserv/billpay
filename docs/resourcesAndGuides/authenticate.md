## Authenticate

Authentication to the CheckFree Next APIs is achieved using OAuth 2.0
via a custom grant. To obtain an access token, a JSON Web Token (JWT)
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
The JWT must be signed with your certificate and encrypted with Fiserv's
public key. You will need to provide the public key of your signing
certificate to Fiserv. Please work with your project manager for full
requirements and installation. Your project manager will provide
Fiserv's public key. This JWT then must be signed and encrypted with a
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
| grant\_type | Req | body       | "ChallengerGrant" |                                                             |
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
JWT: eyJhb...  
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
JWT: eyJhb...  
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
JWT: eyJhb...  
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