# Sample Use Case Implementation

## Making a Bill Payment

Use the CheckFree Next APIs to enable a custom UI to accept bill
payments for one or more businesses and submit the payments for
processing to the Fiserv payment services. Follow this recommended
sequence for the best results:

1.  [Authenticate](./authenticate.md)- Authenticate to get the access token
    and the refresh token.

2.  [Get User Info](../apiDomains/users.md#get-user-information): Get the details of the
    consumer including the contact information.

3.  [Get Payees](../apiDomains/payees.md#get-payees-list): Get a list of businesses that the
    consumer has added as payees.

4.  [Get Bank Accounts](../apiDomains/bankAccounts.md#get-bank-accounts-for-a-consumer-list): Get a
    list of accounts available for a consumer to fund a bill payment.

5.  [Get Transaction Calendar](../apiDomains/transactionCalendar.md#get-transaction-calendar): Get a list of
    all available dates to deliver a payment to the selected payee along
    with associated fees

6.  [Create a Transaction](../apiDomains/transactions.md#create-a-transaction): Schedule one or more
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

1.  [Authenticate](./authenticate.md) – Authenticate to get the access token
    and the refresh token.

2.  [Get Consumer Eligibility for Bill Discovery](../apiDomains/potentialPayees.md#get-consumer-eligibility-for-bill-discovery) – If the
    sponsor is configured with eligibility rules, determine if the
    consumer is eligible for bill discovery and if there are outstanding
    potential payees.

3.  [Record User Consent](../apiDomains/users.md#record-users-consent-information) – If the
    consumer is eligible for bill discovery, capture the consent from
    the consumer for finding bills from bureau and billers.

4.  [Find Bills](../apiDomains/potentialPayees.md#get-potential-payees-list) – Use the GET Potential
    Payees API to get the suggested billers for the consumer once the
    consumer has given the consent and has at least one outstanding
    potential payee. This will not return any results if the consent was
    either not provided by the consumer or the UI has not sent the same
    to the API. Display all potential payees to the consumer to select
    one or more for adding as payees.

5.  [Verify Potential Payee](../apiDomains/potentialPayees.md#verify-potential-payee) – Once the
    consumer has selected one or more potential payees to add, use this
    API to add the billers as Payees and make them available to make a
    payment.
