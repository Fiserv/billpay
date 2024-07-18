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