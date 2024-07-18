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