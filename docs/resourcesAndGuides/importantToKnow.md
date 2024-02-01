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