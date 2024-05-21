# Getting Started

## Welcome to Bill Pay!

To get started with the Bill Pay APIs, follow these steps:
 
## Register

Once agreements have been signed, a financial institution will work with Fiserv Professional services to set up certificates and a client ID.


## Authenticate

Authentication to the Bill Pay APIs is achieved using OAuth 2.0 via a custom grant. To obtain an access token, a JSON Web Token (JWT) must be constructed with some  minimum information: 
Here is a sample JWT payload:

{

"iss":"Second National Bank",

"aud":"Fiserv",

"iat":"1543861896", "jti":"b5d2bcef-0a8b-4218-9f09-cc9c8a24d98c", "exp":"1543862196", "fiserv.identity.billpay.sponsorId":"45678", 

"fiserv.identity.billpay.subscriberId":"98796234567", "fiserv.identity.billpay.originator":"Subscriber", "fiserv.identity.billpay.channel":"Desktop”

}


## Start Making Requests

After authentication you will be given an access token and a refresh token. The access token should be used in the authorization header: “Authorization: Bearer <access_token>”.
The refresh token should be used within the configurable refresh interval (set during client implementation, default 10 minutes).


## Refresh an Access Token

To renew the access token the standard OAuth refresh token process should be used.


## Session Termination

To end a session before the token expires, initiate a token revocation request.


___



 
