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
| 1000 | ERROR | Tenant does not support Bill Discovery. | NULL | N | Business Rule Violation | If Tenant does not opt for bill discovery this error will return. |
| 1001 | ERROR | Bill Discovery feature disabled. | NULL | N | Business Rule Violation | Bill discovery is not enabled |

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